- [[#**Nav2 的定位是：|**Nav2 的定位是：]]
- [[#Nav2 的核心思想是：|Nav2 的核心思想是：]]

# 核心链路
```
机器人模型/传感器
    ↓
Gazebo 仿真
    ↓
/scan、/odom、/tf、/clock
    ↓
SLAM 建图 或 AMCL 定位
    ↓
地图 map + 机器人位姿
    ↓
Nav2 全局规划 + 局部控制 + 避障
    ↓
/cmd_vel
    ↓
Gazebo diff_drive 插件
    ↓
小车运动

```

# Navigation2 的大框架
## **Nav2 的定位是：
它为移动机器人提供感知、规划、控制、定位、可视化等能力，用来让机器人在复杂环境中完成用户指定的任务**

## Nav2 的核心思想是：
```
用户给目标点
    ↓
行为树 BT Navigator 组织整个导航任务
    ↓
Planner Server 规划全局路径
    ↓
Controller Server 跟踪路径并输出速度
    ↓
Costmap 负责障碍物和地图代价
    ↓
Recovery / Behavior Server 处理失败恢复行为
    ↓
最终发布 /cmd_vel

```
1.**Nav2 使用行为树来组织导航行为，并通过多个模块化 server 完成路径计算、控制、恢复等任务；
2.Nav2 期望输入 TF、地图、行为树 XML 和传感器数据，最终输出机器人底盘可执行的速度命令**
3.**Nav2 不是直接让小车动的程序。**
4.**Nav2 是接收目标点后，综合地图、定位、雷达、代价地图、规划器、控制器，然后生成 /cmd_vel 的系统。**
