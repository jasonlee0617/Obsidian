# Codex记忆迁移

# case_20260615_2252 v8 实施计划

## Summary

当前任务是核查 v7 是否真正生效、分析 `/home/robot/tmp/case_20260615_2252` 失败原因，并给出可直接实施的下一版修改细节。本轮已完成只读核查，未修改文件。

v7 部分到位：
- `tf_mount` seed 已成功：`Seed ee_T_cam from TF mount: xyz=(0.0325,-0.0250,-0.0494)`。
- `coverage_roll` 已独立成 family，不再显示为 `safe_lateral`。
- `solver_core` 从 v6 的 2/6 成功提升到 6/8 成功。
- `min_solver_core_samples: 4` 已生效，最终 solver_core 数量满足门槛。

v7 仍未闭环：
- easy_handeye2 仍有 55 次 marker TF future extrapolation，`Got a sample` 为 31 次。
- collector 的 `_take_sample()` 仍只检查样本数 +1，没有调用 `/easy_handeye2/calibration/get_current_transforms` 做一致性门控。
- 最终仍只用 `OpenCV/Tsai-Lenz`，没有 Park/Horaud 多算法比较。
- subset 函数虽已重命名为 residual subset，但实际仍只调用一次 `find_best_geometric_subset()`。
- `what_changed.md` 仍是空模板。

## Failure Cause

本次不是“solver_core 数量不足”，而是“采样时间和求解算法没有被验证”：
- easy_handeye2 v7 默认改成 `now` 后，marker TF 发布频率跟不上当前时间，导致大量 `Requested time > latest data`。
- collector 没有把 easy_handeye2 的当前 transform 查询失败、时间外推、远端样本与本地姿态不一致纳入拒收条件。
- 31 个样本里仍包含大量单轴 depth/lateral/coverage_roll 样本，coverage gate 被刷满，但对手眼求解条件数贡献低。
- Tsai-Lenz 对这组样本继续发散：full set 结果约 `z=1.462m`，`translation_norm=1.469m`；subset 后仍 `translation_norm=1.418m`。
- 当前 subset 只移除 `[22,23,25]` 并重算一次，没有真正枚举可移除组合，也没有换算法。

## Implementation Changes

### easy_handeye2 采样时间

修改 `easy_handeye2/easy_handeye2/easy_handeye2/handeye_sampler.py`：
- 将默认 `sample_time_policy` 从 `"now"` 改为 `"latest_common"`。
- 在 `__init__()` 中声明并读取参数：
  - `sample_time_policy`，默认 `"latest_common"`。
  - `sample_time_offset_sec`，默认 `0.0`。
  - `sample_lookup_timeout_sec`，默认 `1.0`。
- `_get_transforms()` 不再用函数默认值决定策略，而是默认读取实例参数。
- `current_transforms()` 和 `take_sample()` 都走同一套 `_get_transforms()`。
- `lookup_transform()` timeout 使用 `sample_lookup_timeout_sec`。
- 将每次采样打印 `all frames` 改成只在失败时打印，减少日志噪声。

保留 `handeye_server.py` 中 `take_sample()` 失败不增长样本列表的行为；由于 `TakeSample.srv` 没有 success 字段，collector 继续用样本数和 current transforms 判断失败。

### collector 远端一致性门控

修改 `hand_eye_calibration/scripts/collector_config.py`：
- 在 `CollectorFramesConfig` 增加：
  - `get_current_transforms_service`
  - `set_algorithm_service`
- YAML 默认值：
  - `get_current_transforms_service: /easy_handeye2/calibration/get_current_transforms`
  - `set_algorithm_service: /easy_handeye2/calibration/set_algorithm`

修改 `auto_calibration_collector.py`：
- `_setup_services()` 新增：
  - `self.get_current_transforms_cli = self.create_client(TakeSample, ...)`
  - `self.set_algorithm_cli = self.create_client(SetAlgorithm, ...)`

修改 `collector_execution.py`：
- 新增 `_get_remote_current_sample()`：调用 `get_current_transforms`，要求返回 exactly 1 个 sample。
- 新增 `_transform_msg_to_matrix()` 和 `_remote_sample_consistency_status()`。
- `_take_sample()` 流程改为：
  - 在 `sample_consistency_timeout` 内循环等待 remote current 可用。
  - 读取本地 `base_frame -> ee_frame` 和 `tracking_base_frame -> tracking_marker_frame`。
  - 比较 remote current 与 local TF：平移差 <= `0.002m`，旋转差 <= `0.5deg`。
  - 调用 `take_sample` 后确认样本数 +1。
  - 再取远端最后一个 sample，与 preflight remote current 比较，防止采样瞬间机器人/marker 已变化。
  - 任一失败则拒收样本，并日志输出 `local_vs_remote`、`preflight_vs_taken` 差异。

### 多算法求解和真正 residual subset

修改 `collector_execution.py`：
- 增加本地 OpenCV 求解函数 `_solve_local_handeye(samples, algorithm)`，算法固定比较：
  - `Park`
  - `Horaud`
  - `Tsai-Lenz`
- 输入直接使用 `sample_manager.accepted_sample_poses` 与 `accepted_tracking_poses`，调用 `cv2.calibrateHandEye()`，输出 `ee_T_cam`。
- 新增 `_score_local_solution()`：计算 `translation_norm`、marker residual `span_norm/rmse/max`、可选 tf_mount error。
- `_finalize_calibration()` 先跑本地多算法；没有本地 sanity PASS 时，不调用远端 save。
- 本地选出算法后，调用 `set_algorithm_service` 切换 easy_handeye2 到 `OpenCV/<winner>`，再远端 compute；远端结果必须与本地结果在 `2mm/0.5deg` 内一致，否则不保存。

替换 `_try_residual_subset_optimization()`：
- 不再调用 `find_best_geometric_subset()` 作为唯一候选。
- 枚举 removable 样本组合，`max_remove=6`。
- 不允许移除 `anchor_pose` 和 `solver_core`。
- 每个候选 subset 必须保留：
  - `min_successful_samples`
  - coverage PASS
  - observability PASS
  - `min_solver_core_samples`
- 对每个 subset 跑 `Park/Horaud/Tsai-Lenz`，按 `span_norm -> rmse -> translation_norm -> 样本数多` 排序。
- 找到 PASS 后，再按降序调用远端 `remove_sample`，切换算法，compute/save。

### base_offsets 和停止策略

修改 `hand_eye_calibration/config/auto_calibration_collector.yaml`：
- 保留当前成功的 6 个 solver_core，删除 v7 中实际 too-close 的两个：
  - `p+1.5/y-2.0/r+4.5/base_y=-0.003/base_z=0.004`
  - `p-1.5/y+2.0/r-4.5/base_x=-0.003/base_z=-0.004`
- 删除高失败率 yaw 候选：
  - `yaw +3.0/+3.5/+4.0`
  - `yaw -4.0/-5.0/-6.0`
  - 对应 expansion 中 `+3.0/+3.5/-3.5/-4.5`
- 保留低风险 yaw：
  - `yaw -3.0`
  - `yaw +2.5 base_y=0.010 base_z=-0.020`
  - `yaw +2.0 base_y=0.008 base_z=-0.018`
- `depth_span` 缩减为 4 个：
  - `base_z: +0.030`
  - `base_z: -0.030`
  - `base_z: +0.040`
  - `base_z: -0.040`
- `lateral_span` 缩减为 4 个：
  - `base_x: +0.020`
  - `base_x: +0.030`
  - `base_y: +0.020`
  - `base_y: +0.030`
- `coverage_roll` 缩减为 2 个：
  - `roll: +15.0`
  - `roll: -15.0`
- 新增参数：
  - `max_successful_samples: 22`
  - `coverage_stop_require_margin: false`
  - `calibration_algorithms: [Park, Horaud, Tsai-Lenz]`
- 当 `min_successful_samples + dual gate + solver_core gate` 都满足时，立即跑一次 provisional local solver；若 PASS，停止继续采样；若 FAIL，只允许继续采 `depth/lateral/coverage_roll` 中尚未满足的最小补样，达到 `max_successful_samples` 后强制进入求解，不再堆样本。

### 冗余清理

- 将 family order/label/removable/intent 统一放到 `sample_manager.py`，`collector_config.py` 只引用，不再维护重复表。
- 删除或停用 `find_best_geometric_subset()` 旧入口；所有 subset 统一走 residual subset。
- 修正日志：`Geometric subset sanity PASS` 改为 `Residual subset sanity PASS`。
- 删除 solver_core 注释中“Positive pitch only”的错误描述，因为当前实际包含正负 pitch。
- 诊断脚本 `collect_auto_calibration_diagnostics.sh finalize` 增加校验：如果 `notes/what_changed.md` 仍是模板内容，输出 WARN，提示本轮实验记录无效。

## Test Plan

静态检查：
- `rg "now\\(\\) - rclpy.time.Duration|200000000" easy_handeye2` 不应再命中旧采样偏移。
- `rg "_try_geometric_subset|Geometric subset" hand_eye_calibration/scripts` 不应再命中执行路径。
- `rg "get_current_transforms_cli|set_algorithm_cli|calibration_algorithms|max_successful_samples" hand_eye_calibration/scripts` 必须命中。

构建检查：
- 运行 `colcon build --packages-select easy_handeye2 easy_handeye2_msgs hand_eye_calibration gazebo_launch`。
- 重新 source `install/setup.bash` 后再跑诊断 case。

运行验收：
- collector 日志必须出现 `Seed ee_T_cam from TF mount`。
- 每个成功样本必须有 sample consistency PASS 日志。
- `handeye_server` 的 extrapolation 错误应接近 0；如仍出现，collector 不得接收对应样本。
- 成功样本数量目标为 15-22，不应再达到 31。
- solver 日志必须显示至少比较 `Park/Horaud/Tsai-Lenz`。
- 保存前必须满足：
  - `translation_norm < 0.30m`
  - `marker_span_norm <= 0.02m`
  - `rmse <= 0.02m`
- 若仍失败，日志必须明确给出每个算法和最佳 subset 的残差表，而不是只输出 Tsai-Lenz 的单个失败结果。

## Assumptions

- 继续以 `base_offsets` 为核心，不改随机采样。
- Gazebo 中真实相机安装仍以 TF/xacro 为准：`ee_T_cam ~= [0.0325,-0.0250,-0.0494]`，RPY 约 `[0,0,-180]`。
- 下一版优先保证远端采样可信和求解可验证，暂时不追求更多样本数量。




终端1启动/home/robot/tmp/case_20260615_2252/commands/run_launch.sh
终端2启动/home/robot/tmp/case_20260615_2252/commands/run_collector.sh
其中保存好的数据在/tmp/case_20260615_2252中
1.以上保存的数据为按照你提供的v7方案进行修改过后的
2.检查是否有按照v7方案修改到位
3.请你分析失败的原因并且提出相应的优化改进方案,并且调整修改的细节要具体列举出来
4.相关的代码冗余的部分可以删除