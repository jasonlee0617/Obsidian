**安装PlotJuggler**：
```
sudo apt install ros-humble-plotjuggler*
```

**启动PlotJuggler**：
```
ros2 run plotjuggler plotjuggler
```

**使用ros2 bag录制数据**：
```
# 创建一个目录存放bag文件
mkdir bag_files
cd bag_files
# 开始录制指定的调试话题
指定录制路径
ros2 bag record -o ~/bags/LADRC_sample_data1 \
/servo_error_xyyaw \
/servo_pid_terms \
/servo_cmd_stages \
/servo_ff_vel_filt \
/servo_latency_trace \
/vision_latency_trace \
/servo_act_latency_trace \
/servo_ladrc_debug \

```
