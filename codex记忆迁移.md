# Codex记忆迁移

## 对话主题总览
本轮对话围绕 `hand_eye_calibration/auto_calibration_collector` 的持续整改，目标是把 Gazebo/真机共用的自动手眼标定采集流程从“能跑”推进到“可诊断、可维护、可收敛、可防坏保存”。

主线经历了以下几个阶段：
1. 修复 OpenCV `cv2.aruco` 缺失与图像质量门控问题。
2. 修复启动阶段 `corner margin` 过严导致刚启动就退出的问题，区分 `startup_visibility` 与 `sampling_quality`。
3. 增加 coverage gate、sanity check、防坏 `.calib` 保存。
4. 将超长单文件拆分为：
   - `auto_calibration_collector.py`
   - `collector_config.py`
   - `collector_geometry.py`
   - `collector_execution.py`
   - `vision_quality_gate.py`
   - `sample_manager.py`
   - `calibration_validator.py`
5. 将交互流程收敛为手动待命模式：到 `original_place` 后，按 `Enter/s` 启动一轮采集，按 `q` 中止并返回 `original_place`。
6. 去除 Gazebo 真值 TF 对候选生成的直接依赖，引入 `seed_camera_xyz_m / seed_camera_rpy_deg` 作为粗略先验，仅用于运行期近似，不把 xacro 真值当作标定输入。
7. 将候选生成从 `Look-At + inv(seed_ee_T_cam)` 改成 `original_place` 基准下的 `base_offsets` 固定偏移方案，保留视觉闭环 `recenter`。
8. 重写诊断包文档与脚本，最终收敛为只记录终端1/终端2日志的最小诊断流。
9. 基于多轮 case（尤其 `case_20260615_1729`）继续重构样本治理、候选家族治理和最终保存链。

---

## 已确认的重要工程结论

### 1. Gazebo 中的相机 mount 真值不能作为采样执行输入
`s622_handeye_camera_mount.xacro` 中的 `grasp_frame -> camera_color_optical_frame` 是仿真 ground truth，只能用于：
- Gazebo 成像
- 最终误差评估

不能用于：
- 候选位姿生成
- recenter 反解
- 运行时假定 `ee_T_cam` 已知

因此运行时已改为只使用 YAML seed：
- `seed_camera_xyz_m`
- `seed_camera_rpy_deg`

Gazebo 与真机共享同一套 seed 假设，保证仿真和真机的运行前提一致。

### 2. `original_place` 现在是唯一初始理想观测位姿来源
此前存在一套运行时“自动调 ideal original-place”的逻辑，导致：
- 启动阶段行为过复杂
- 额外 MoveIt 运动过多
- 易受 stale frame / TF 抖动影响

现已收敛为：
- 机械臂移动到 `original_place`
- 直接在该位姿检查 `startup_visibility / sampling_quality / stable frames / camera model check`
- 不再为初始理想位姿做二次修正

如果 `original_place` 本身不满足采样门槛，直接报错并要求人工调整 `original_place_xyz / original_place_rpy_deg`。

### 3. 候选生成主方案已固定为 `base_offsets`
当前主策略不是 marker-centric Look-At，而是：
- 以实际到达的 `original_place` 的 `base_T_ee` 作为参考位姿
- 在 `base_frame` 下施加固定小偏移：`base_x / base_y / base_z / roll`
- 由 MoveIt 执行到位
- 到位后通过 `recenter` 进行小范围视觉闭环修正

保留的安全机制包括：
- `_move_with_visibility_guard()`
- `_recover_last_good_pose()`
- `_recenter_marker()`
- `_wait_for_stable_marker()`
- `_camera_model_self_check()`
- diversity 过滤
- coverage gate
- calibration sanity check
- sample count `+1` 校验

### 4. 分段插值候选动作已被取消，采样动作为一步到位
之前候选执行存在 `interpolated_transforms()` / segment move，日志中大量出现 `segment k/n`。

已经改为：
- 候选动作一次 MoveIt 直达
- 运动前后做 freshness / visibility / sampling quality / recenter / stable-frame gate
- 删除采样插值专用参数：
  - `segment_step_m`
  - `segment_step_deg`
  - `segment_settle_time`

### 5. 诊断脚本已改成最小闭环
`collect_auto_calibration_diagnostics.sh` 目前的推荐流程是：
- `prepare`
- 运行生成的 `commands/run_launch.sh`
- 运行生成的 `commands/run_collector.sh`
- `finalize`

当前重点是：
- 自动记录终端1 `launch.log`
- 自动记录终端2 `collector.log`
- 若没有日志，`finalize` 直接判失败

不再默认采集大量无关运行快照或源码副本。

---

## 代码结构演进后的当前职责边界

### `auto_calibration_collector.py`
角色：ROS node facade
负责：
- 参数加载后的模块装配
- ROS 回调入口
- ArUco worker
- 键盘状态机
- executor 生命周期

### `collector_config.py`
角色：唯一参数库存
负责：
- 读取 `auto_calibration_collector.yaml`
- 组织 frames / motion / sampling config dataclass
- 保持 YAML 与代码默认值同步

### `collector_geometry.py`
角色：纯几何与候选构造
负责：
- TransformMatrix
- base-offset 候选生成
- TF/矩阵/姿态变换辅助

### `vision_quality_gate.py`
角色：图像质量门控
负责：
- startup / camera-model / sampling 三层门槛
- fresh frame / successful observation 判断
- stable-frame 检查
- ArUco 观测缓存

### `sample_manager.py`
角色：候选与样本集治理中心
现状已大幅重构，包含：
- `CandidateFamily`
- `CandidateSpec`
- `AcceptedSampleQuality`
- `AcceptedSampleRecord`
- accepted sample set 的统一记录
- coverage / diversity / removable subset 支撑

### `calibration_validator.py`
角色：标定结果合理性验证
负责：
- translation norm 检查
- marker residual / span / RMSE
- 可选 TF mount 对比
- sanity 状态输出

### `collector_execution.py`
角色：执行编排层
负责：
- original_place / MoveIt readiness / standby loop
- 固定候选执行
- recenter、stable-frame、take-sample 编排
- coverage gate / finalize
- 样本集优化与远端 apply

---

## 多轮诊断后的关键问题演化

### 早期问题
1. `cv2.aruco` 缺失，导致图像级质量门控不可用。
2. 初始 `corner margin=98px < 100px`，启动自检直接退出。
3. 采样覆盖不足，Tsai 发散，出现严重错误 `.calib`，例如 `z=-9676m`。
4. MoveIt 未 ready 仍继续自动运动，存在明显工程风险。
5. 手动模式输入逻辑不闭环，程序可能空转不退出。

### 中期问题
1. `original_place` ideal tune 过硬，运行时反复调位，实际收益低。
2. 候选采用 Look-At 反解，强依赖 seed 精度，造成大量 IK fail / 少量移动就丢 marker。
3. fixed-offset 早期版本的候选粒度与 diversity gate、coverage gate 不匹配：
   - 候选理论跨度就不足以通过 coverage
   - 很多点执行成功后才被 `too close` 拒绝
4. recenter 对负向 lateral 候选在第二步容易发散。

### 后期问题
在 `case_20260615_1729` 已确认：
- 采样数不是问题，已到 `23/15`
- coverage 与 redundancy 也不是问题
- 真正失败点在最终 sanity/save：
  - full set 残差大
  - 3轮简单 prune 后仍是系统性坏簇，不是单点 outlier
- 低价值候选的模式稳定：
  - 负向 `base_x/base_y` 候选频繁 `recenter_error_not_decreasing`
  - `tilt_y` 没有稳定贡献
  - 大角度 `tilt_x` 风险高、收益低

结论：问题已经从“采样能不能跑通”切换成“样本集治理与最终保存链是否合理”。

---

## 最新一轮已实施的激进收口重构（基于 `case_20260615_1729`）

### 1. 候选改为分层 family，而不是平铺列表
新增候选家族语义：
- `core_translation_roll`
- `coverage_expansion`
- `provisional_risky`

当前策略：
- `center / 小中幅 roll / 小中幅正向 base_x/base_y / 常规 base_z` -> core
- 更大覆盖补充 -> coverage
- 负向 lateral、tilt -> risky

默认 sweep 中：
- `tilt_y` 已关闭
- `tilt_x` 已降级，不进入默认主 sweep
- 负向 lateral 仍保留少量补覆盖用途，但不再与正向候选同权

### 2. accepted samples 升级为带元数据的记录集
不再维护多套松散列表，而是每个 accepted sample 都记录：
- `base_T_ee`
- `cam_T_marker`
- candidate family
- candidate spec
- quality snapshot
- recenter 是否发生
- recenter 是否严格收敛
- 是否 removable

这为后续：
- coverage
- diversity
- subset optimization
- risky family 排除
提供了统一数据基础。

### 3. 最终保存链改为“本地子集优化 -> 远端应用”
此前逻辑：
- full set sanity fail
- 逐轮删一个 outlier
- 重新 compute

问题：
- 对系统性坏簇无效
- 日志混杂，不清楚到底 full set 还是 prune 后失败

现在逻辑：
1. 本地基于 accepted sample set 搜索最优子集
2. 目标函数按 `(span_norm, rmse, max_error)` 字典序最小化
3. 仅允许删除 `removable=true` 样本，优先删 risky family
4. 先 greedy backward elimination
5. 再做 `k<=3` 的组合搜索
6. 确定最佳子集后，才映射为远端 `RemoveSample` 调用
7. 再 `ComputeCalibration` / `SaveCalibration`

如果最优子集仍然不通过 sanity：
- 明确报 `best subset still failed`
- 不再把旧的 full-set sanity 文本混在最终错误里

### 4. risky family 的采样准入收紧
当前策略：
- 对 `provisional_risky` 家族，如果 post-move 触发 recenter，必须在首轮 recenter 就满足：
  - `sign PASS`
  - `improvement PASS`
- 否则直接拒绝该候选，不进入 accepted set

对 core family 维持相对宽松：
- 若 sampling quality 已通过，可以跳过 recenter
- recenter 一轮不改善但仍满足 sampling quality 时，可继续采样

目的：保留覆盖能力，但减少系统性坏簇污染。

### 5. 代码可读性与冗余收口
已做的收口包括：
- 删除 `collector_execution` 中无独立价值的 `base_xyz / base_rpy` 会话状态
- 删除无效 import
- 把 `sample_subset_optimizer.py` 正式加入安装规则，避免源码可跑、安装态导入失败
- 继续保持 `auto_calibration_collector.py` 只做 façade，不把执行逻辑吸回主文件

---

## 当前配置面的重要状态
配置主文件：`hand_eye_calibration/config/auto_calibration_collector.yaml`

当前原则：
1. YAML 是唯一控制面。
2. `collector_config.py` 是参数库存，新增行为参数必须同步入 YAML。
3. base-offset 方案保留，seed 仍保留。
4. 默认主 sweep 已禁用 tilt 主路径。

当前重要趋势：
- `sampling_tilt_x_offsets_deg: [0.0]`
- `sampling_tilt_y_offsets_deg: [0.0]`
- `sampling_roll_offsets_deg` 保留
- `sampling_base_x/y/z_offsets_m` 继续作为主覆盖变量
- `sample_min_translation_delta_m` 已逐轮下调到更匹配 fixed-offset 粒度
- `recenter_*` 参数保持保守，避免 lateral 发散

---

## 诊断脚本与 case 使用规范
有效诊断包要求至少有：
- `logs/launch.log`
- `logs/collector.log`

否则该 case 应视为无效轮次。

推荐流程：
1. `bash collect_auto_calibration_diagnostics.sh prepare --case-dir /home/robot/tmp/case_YYYYMMDD_HHMM`
2. 终端1运行生成的 `commands/run_launch.sh`
3. 终端2运行生成的 `commands/run_collector.sh`
4. 一轮结束后执行 `finalize`
5. 分析时优先看 `collector.log`

最新阶段的 case 演进中，真正有价值的几个 case：
- `case_20260615_1729`：证明采样数和 coverage 已不再是主问题，样本治理和保存链成为瓶颈

---

## 当前系统的阶段性结论
当前 collector 已经不再是“靠 Gazebo 真值作弊”的版本，而是：
- 使用 rough seed
- 使用 original_place
- 使用 fixed base-offset sweep
- 使用视觉闭环 recenter
- 使用 coverage gate / sanity gate
- 使用 subset optimizer 防止坏样本集直接写坏 `.calib`

也就是说，系统定位已经从：
- “Gazebo 自动采样演示脚本”
推进为：
- “可用于继续逼近真机首轮自动标定能力的工程原型”

但仍未完全达到“真实机器人首次标定即稳定可靠”的程度，当前主要剩余风险仍在：
1. risky lateral 候选对 recenter 的鲁棒性仍有限
2. fixed-offset sweep 仍需要进一步筛掉低价值族群
3. subset optimizer 解决了保存前治理，但并不替代更高质量的采样分布设计
4. 真机首次部署时，仍需要严格核对：
   - original_place 是否稳定看见 marker
   - seed 是否足够接近真实安装
   - TF / CameraInfo / ArUco 观测时基是否稳定

---

## 后续可以直接承接的工作方向

### 方向1：继续保留 base-offset，进一步压缩低价值候选
建议：
- 默认继续保持 tilt 关闭
- 继续弱化负向 lateral
- 将 coverage 扩展更多交给 `base_z + 正向 lateral + roll`

### 方向2：继续增强 subset optimizer
建议：
- 把 candidate family / quality snapshot 进一步纳入评分
- 例如对 `recenter_strict_converged=false` 的样本加惩罚
- 把 family 先验直接融合进 removable 优先级

### 方向3：真机场景准备
建议：
- 保持 Gazebo 与真机共用同一 seed 配置结构
- 真机只调整 seed 数值，不改运行逻辑
- 采集前先单独验证 `original_place` 下的视觉稳定性与 sampling quality

### 方向4：进一步瘦身执行层
建议：
- `collector_execution.py` 继续只保留 orchestration
- 将 subset planning / risky-admission policy / quality snapshot policy 继续下沉到独立 helper
- 目标是让执行层只关心“调谁、何时调、失败怎么收尾”

---

## 一句话迁移结论
本轮长期对话的最终落点是：

**`auto_calibration_collector` 已从依赖 Gazebo 真值、逻辑分散、容易保存坏标定的试验脚本，重构为基于 `original_place + base_offsets + recenter + coverage/sanity/subset-gate` 的可维护工程原型；当前下一阶段的重点不是再修启动链，而是继续优化候选家族治理与 accepted sample subset 治理。**
