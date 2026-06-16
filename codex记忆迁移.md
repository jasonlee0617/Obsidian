# Codex记忆迁移

当前任务目标：持续把 `auto_calibration_collector` 调到能在 Gazebo 中稳定采集、通过 coverage/observability/sanity，并最终保存有效手眼标定结果。已完成多轮诊断，当前问题已从“初始看不到/偏心”推进到“候选调度和 gate 通过”。

以下内容可直接复制到新聊天作为上下文。

````
# auto_calibration_collector 阶段性总结

## 工作区

- Repo: `/home/robot/S622_robotarm/src`
- 主要文件：
  - `hand_eye_calibration/config/auto_calibration_collector.yaml`
  - `hand_eye_calibration/scripts/auto_calibration_collector.py`
  - `hand_eye_calibration/scripts/collector_execution.py`
  - `hand_eye_calibration/scripts/sample_manager.py`
  - `hand_eye_calibration/scripts/collector_config.py`
  - `hand_eye_calibration/scripts/vision_quality_gate.py`
  - `hand_eye_calibration/scripts/collect_auto_calibration_diagnostics.sh`

## 当前总体策略

当前 collector 已经从旧的 Look-At / adaptive 方案，收敛为：

- 手动启动采集：
  - 到 `original_place`
  - 等待 Enter 或 `s`
  - 采集中 `q` 返回 original_place
- 不再依赖 Gazebo xacro 真值 `ee_T_cam` 生成候选
- 运行时使用 YAML 中的 approximate seed：
  - `seed_camera_xyz_m`
  - `seed_camera_rpy_deg`
- 采样方案为 spherical-shell base-offset：
  - `sphere_anchor`
  - `sphere_height`
  - `sphere_shell`
  - `sphere_roll_coverage`
- 保留安全机制：
  - image-level ArUco quality gate
  - fresh successful observation
  - stable frames
  - camera model self-check
  - recenter
  - sample count +1 校验
  - coverage gate
  - observability gate
  - sanity / save gate

## 当前关键配置状态

`auto_calibration_collector.yaml` 当前重要配置：

```yaml
camera_info_topic: /camera/camera/color/camera_info
camera_fps: 30
camera_image_width: 1280
camera_image_height: 720

original_place_xyz: [0.230, 0.020, 0.230]
original_place_rpy_deg: [0.0, 180.0, 90.0]

min_successful_samples: 15
max_successful_samples: 18
absolute_max_successful_samples: 24

min_coverage_xy_span_m: 0.04
min_coverage_z_span_m: 0.06
min_coverage_rotation_span_deg: 25.0

min_pitch_span_deg: 4.0
min_yaw_span_deg: 2.0
min_roll_span_deg: 10.0
min_sphere_anchor_samples: 4
min_sphere_height_samples: 3
min_sphere_shell_samples: 4

max_center_error_px: 80.0
min_corner_margin_px: 70.0
min_marker_side_px: 40.0
````

## 已解决的问题

### 1. OpenCV ArUco 模块问题

之前 pip OpenCV 混装导致 `cv2.aruco` 不可用。已通过代码 fallback/环境绕开，当前日志能看到：

```
Image-level ArUco quality gate enabled
```

### 2. 手动模式和进程退出

之前采集结束后进程可能不退出，手动模式键盘轮询也有问题。后续已改成固定手动待命模式：

```
Press Enter or s to start a collection session
q stops current collection and returns to original_place
```

### 3. Gazebo use_sim_time

曾经 `use_sim_time=false` 导致 TF 时间外推错误。现在诊断中已确认：

```
use_sim_time=True
clock_topic_present=True
```

### 4. 去真值依赖

曾经候选生成使用 `lookup_tf(ee_frame, camera_color_optical_frame)`，等于在 Gazebo 中用 xacro 真值辅助标定。现已改成：

- candidate 不再使用真实 `ee_T_cam`
- xacro mount 只作为 Gazebo 成像和最终误差验收
- 运行时依赖 YAML seed + 图像闭环

### 5. Look-At 改为 base-offset / spherical-shell

旧 Look-At 候选导致 IK 无解和 seed 依赖过强。当前方案已改为：

- 以实际到达的 `original_place` 的 `base_T_ee` 为基准
- 叠加 base-frame offset
- 不构造 camera look-at pose
- 不用 `inv(seed_ee_T_cam)` 反解末端位姿

### 6. 初始 original_place 偏心

之前日志：

```
center=(748.5,275.5)
err=137.5px > 80px
```

调整为：

```
original_place_xyz: [0.230, 0.020, 0.230]
```

`case_20260616_1328` 中已验证通过：

```
center=(681.5,342.5)
err=45.0px
Initial sampling-quality gate passed
camera model error=0.1px
```

## 最新有效诊断：case_20260616_1328

路径：

```
/home/robot/tmp/case_20260616_1328
```

运行结果：

```
Collection complete: 19/15
Coverage summary:
  xyz_span=(0.035,0.032,0.040)m
  xy_span=0.047m PASS
  z_span=0.040m FAIL
  rot_span=28.0deg PASS

Observability:
  pitch_span=8.0 PASS
  yaw_span=4.0 PASS
  roll_span=28.0 PASS
  sphere_anchor=8 PASS
  sphere_height=0 FAIL
  sphere_shell=11 PASS

Final:
  Coverage gate FAIL
  Observability gate FAIL
  Skip compute/save calibration
```

## 当前失败原因

`case_1328` 的失败不是 initial marker，不是 MoveIt，也不是 RGB camera_info。

根因是：

- `sphere_height` 候选没有执行到
- Z 覆盖只靠 shell 中 `z±0.020`
- 所以 `z_span` 只能到 `0.040m`
- 但门槛是 `0.060m`
- 同时 `sphere_height=0/3 FAIL`

日志显示运行时仍是旧顺序：

```
sphere_anchor -> sphere_shell
```

虽然当前源码已经改成：

```
sphere_anchor -> sphere_height -> sphere_shell -> sphere_roll_coverage
```

所以 `case_1328` 很可能没有跑到最新安装版本。

## 当前源码检查结论

当前源码中已经看到以下整改：

### sample_manager.py

已有：

```
gate_deficits()
```

family order 当前为：

```
["sphere_anchor", "sphere_height", "sphere_shell", "sphere_roll_coverage"]
```

### collector_execution.py

已有：

```
_is_gate_deficit_critical(candidate, source, deficits)
```

soft-cap 已不再只看 XY，而是按 gate deficit 放行：

- `sphere_height` 对应 `z` / `height`
- `sphere_shell` 对应 `xy` / `z`
- `sphere_roll_coverage` 对应 `rot`
- `sphere_anchor` 对应 `pitch/yaw/roll`

但仍需清理：

- 日志里还有 `hard cap` 文案，应改为 `soft cap / absolute cap`
- `gate_deficits()` 应补 `shell` 字段，保持所有 gate 都有缺口表示
- `coverage_stop_*` 旧参数已从 YAML 删除，但要确认 `collector_config.py` 和逻辑中无残留

## 下一步优先整改

### 1. 必须先重新 build

当前最重要的是确保最新源码真正进入 install：

```
cd /home/robot/S622_robotarm
source /opt/ros/humble/setup.bash
colcon build --packages-select hand_eye_calibration --cmake-args -DBUILD_TESTING=OFF
source install/setup.bash
```

下一轮日志必须看到：

```
candidate 09 family=sphere_height
```

如果 candidate 09 仍然是 `sphere_shell`，说明仍在跑旧版本。

### 2. 补齐 gate_deficits

在 `sample_manager.py` 中增加：

```
"shell": obs_m["sphere_shell_count"] < self.min_sphere_shell_samples,
```

在 `collector_execution.py` 中增加：

```
if source == "sphere_shell" and deficits.get("shell"):
    return True
```

### 3. 修改日志文案

把：

```
hard cap 18
```

改为：

```
soft cap 18, absolute cap 24
```

### 4. 保持 height 参数不变

当前无需扩大 Z 动作。已有：

```
sphere_height:
  - {base_z: 0.030}
  - {base_z: -0.030}
  - {base_z: 0.045}
  - {base_z: -0.045}
```

只要执行到，理论上足够通过：

```
z_span >= 0.060
sphere_height >= 3
```

### 5. 检查 shell coverage supplement

当前：

```
base_x/base_y 0.020 与 0.025 差值只有 0.005
```

而：

```
sample_min_translation_delta_m: 0.006
```

所以 `0.025` 可能被 generation-time dedup 去掉。

建议之后把 `0.025` 改为 `0.030`，但这不是当前最高优先级。

## 下一轮验收标准

下一轮诊断应满足：

```
Initial sampling-quality gate passed
Candidate sweep includes sphere_height
sphere_height >= 3
z_span >= 0.060
xy_span >= 0.040
rot_span >= 25.0
Coverage gate PASS
Observability gate PASS
```

如果 dual gate PASS 后仍保存失败，再进入下一层分析：

- SaveSamples 是否成功
- ComputeCalibration 是否成功
- local sanity 是否失败
- subset optimizer 是否错误删除了 height 样本

## 诊断数据提供方式

继续使用：

```
bash /home/robot/S622_robotarm/src/hand_eye_calibration/scripts/collect_auto_calibration_diagnostics.sh prepare \
  --case-dir /home/robot/tmp/case_$(date +%Y%m%d_%H%M)
```

然后：

```
/home/robot/tmp/case_YYYYMMDD_HHMM/commands/run_launch.sh
/home/robot/tmp/case_YYYYMMDD_HHMM/commands/run_collector.sh
```

结束后：

```
bash /home/robot/S622_robotarm/src/hand_eye_calibration/scripts/collect_auto_calibration_diagnostics.sh finalize \
  --case-dir /home/robot/tmp/case_YYYYMMDD_HHMM
```

发给 Codex 时只需要告诉 case 路径，例如：

```
/home/robot/tmp/case_20260616_xxxx
```

## 注意事项

- 不要再用 `/tmp/case_xxx` 和 `/home/robot/tmp/case_xxx` 混用。
- 每次改源码后必须重新 `colcon build`。
- 当前失败已不是 original_place 问题，除非初始 center error 再次超过 80px，否则不要继续调 original_place。
- 当前不建议放宽 coverage / observability 门槛。


终端1启动/home/robot/tmp/case_20260616_0941/commands/run_launch.sh
终端2启动/home/robot/tmp/case_20260616_0941/commands/run_collector.sh
其中保存好的数据在/tmp/case_20260616_0941中
1.以上保存的数据为按照你提供的球面壳采样重构方案“”方案进行修改过后的
2.检查是否有按照“球面壳采样重构方案”方案修改到位
3.请你分析失败的原因并且提出相应的优化改进方案,并且调整修改的细节要具体列举出来
4.相关的代码冗余的部分可以删除