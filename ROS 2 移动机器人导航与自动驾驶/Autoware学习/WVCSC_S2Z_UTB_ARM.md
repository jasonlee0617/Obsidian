- [[#项目地址|项目地址]]
- [[#依赖安装|依赖安装]]
- [[#2.1 工作区总览：7个包总结构|2.1 工作区总览：7个包总结构]]
- [[#2.2 7个包的依赖关系图|2.2 7个包的依赖关系图]]
- [[#2.3 4层架构分层图|2.3 4层架构分层图]]
- [[#2.4 系统数据流全景图|2.4 系统数据流全景图]]
- [[#2.5 核心话题通信矩阵|2.5 核心话题通信矩阵]]
- [[#包1：`can_bridge` — CAN通信桥梁|包1：`can_bridge` — CAN通信桥梁]]
- [[#包2：`wtb_car_driver` — 底盘驱动和控制|包2：`wtb_car_driver` — 底盘驱动和控制]]
- [[#包3：`lidar_ros2` — 激光雷达驱动|包3：`lidar_ros2` — 激光雷达驱动]]
- [[#包4：`my_cartographer` — SLAM 建图|包4：`my_cartographer` — SLAM 建图]]
- [[#包5：`my_navigation2` — 自主导航|包5：`my_navigation2` — 自主导航]]
- [[#7.1 两套栈的总体架构|7.1 两套栈的总体架构]]
- [[#7.2 Nav2完整数据流|7.2 Nav2完整数据流]]
- [[#7.3 Autoware完整数据流|7.3 Autoware完整数据流]]
- [[#7.4 逐层对比：相同点和不同点|7.4 逐层对比：相同点和不同点]]
- [[#7.5 核心架构差异的可视化对比|7.5 核心架构差异的可视化对比]]
- [[#7.6 消息类型对比|7.6 消息类型对比]]
- [[#7.7 核心差异根源 — 设计哲学|7.7 核心差异根源 — 设计哲学]]
- [[#7.8 总结表|7.8 总结表]]
- [[#项目地址|项目地址]]
- [[#依赖安装|依赖安装]]
- [[#2.1 工作区总览：7个包总结构|2.1 工作区总览：7个包总结构]]
- [[#2.2 7个包的依赖关系图|2.2 7个包的依赖关系图]]
- [[#2.3 4层架构分层图|2.3 4层架构分层图]]
- [[#2.4 系统数据流全景图|2.4 系统数据流全景图]]
- [[#2.5 核心话题通信矩阵|2.5 核心话题通信矩阵]]
- [[#包1：`can_bridge` — CAN通信桥梁|包1：`can_bridge` — CAN通信桥梁]]
	- [[#包1：`can_bridge` — CAN通信桥梁#C++、Python实现方法总结对比表|C++、Python实现方法总结对比表]]
- [[#包2：`wtb_car_driver` — 底盘驱动和控制|包2：`wtb_car_driver` — 底盘驱动和控制]]
	- [[#包2：`wtb_car_driver` — 底盘驱动和控制#`wtb_car_driver`与`can_bridge`包的关系|`wtb_car_driver`与`can_bridge`包的关系]]
	- [[#包2：`wtb_car_driver` — 底盘驱动和控制#数据流全过程：一次"前进0.5m/s"命令的完整旅程|数据流全过程：一次"前进0.5m/s"命令的完整旅程]]
	- [[#包2：`wtb_car_driver` — 底盘驱动和控制#CAN协议编解码|CAN协议编解码]]
	- [[#包2：`wtb_car_driver` — 底盘驱动和控制#阿克曼里程计计算（wtb_car.cpp:528-591）|阿克曼里程计计算（wtb_car.cpp:528-591）]]
	- [[#包2：`wtb_car_driver` — 底盘驱动和控制#为什么发送3遍命令？（wtb_car.cpp:357-366）|为什么发送3遍命令？（wtb_car.cpp:357-366）]]
	- [[#包2：`wtb_car_driver` — 底盘驱动和控制#EKF 融合配置（ekf_wtb_fdimu.yaml）|EKF 融合配置（ekf_wtb_fdimu.yaml）]]
- [[#包3：`lidar_ros2` — 激光雷达驱动|包3：`lidar_ros2` — 激光雷达驱动]]
- [[#包4：`my_cartographer` — SLAM 建图|包4：`my_cartographer` — SLAM 建图]]
- [[#包5：`my_navigation2` — 自主导航|包5：`my_navigation2` — 自主导航]]
	- [[#包5：`my_navigation2` — 自主导航#阶段一：启动机器人底层驱动|阶段一：启动机器人底层驱动]]
	- [[#包5：`my_navigation2` — 自主导航#阶段二：遥控测试|阶段二：遥控测试]]
	- [[#包5：`my_navigation2` — 自主导航#阶段三：SLAM建图|阶段三：SLAM建图]]
	- [[#包5：`my_navigation2` — 自主导航#阶段四：自主导航|阶段四：自主导航]]
	- [[#包5：`my_navigation2` — 自主导航#启动依赖关系总结|启动依赖关系总结]]
- [[#7.1 两套栈的总体架构|7.1 两套栈的总体架构]]
- [[#7.2 Nav2完整数据流|7.2 Nav2完整数据流]]
- [[#7.3 Autoware完整数据流|7.3 Autoware完整数据流]]
- [[#7.4 逐层对比：相同点和不同点|7.4 逐层对比：相同点和不同点]]
- [[#7.5 核心架构差异的可视化对比|7.5 核心架构差异的可视化对比]]
- [[#7.6 消息类型对比|7.6 消息类型对比]]
- [[#7.7 核心差异根源 — 设计哲学|7.7 核心差异根源 — 设计哲学]]
- [[#7.8 总结表|7.8 总结表]]

# 一、 WVCSC 自主导航机器人系统 — 完整教学分析

## 项目地址

```
git clone https://codeup.aliyun.com/63c792c88de064fd5324f4e7/WorldSkillsCompetition/WVCSC_S2Z_UTB_ARM.git
```

## 依赖安装

```bash
## CAN分析仪权限设置
sudo vi /etc/udev/rules.d/99-myusb.rules
## 写入
ACTION=="add",SUBSYSTEMS=="usb", ATTRS{idVendor}=="04d8", ATTRS{idProduct}=="0053", GROUP="users", MODE="0777"

## 环境安装
sudo apt-get install ros-humble-can-msgs
```

---

# 二、系统总览：这是什么？

这是一个**基于 ROS2 的阿克曼（Ackermann）转向无人车自主导航系统**。它的核心能力是：

> **建图**：遥控小车在环境中跑一圈，激光雷达扫描周围环境，Cartographer 算法生成 2D 栅格地图

> **导航**：给定目标点，小车自主规划路径，避开障碍物，自动驶向目标

为了实现这个目标，工程被拆分为 **7 个 ROS2 功能包**，形成一条清晰的数据管道。

---

# 三、系统整体架构图

## 2.1 工作区总览：7个包总结构

```
/home/robot/WVCSC_S2Z_UTB_ARM/src/
│
├── serial/                          ← 串口通信库（纯C++库，无ROS依赖）
│   ├── src/serial.cc                ← 跨平台串口实现（pimpl架构）
│   ├── include/serial/serial.h      ← 公共API（open/read/write/close）
│   ├── src/impl/unix.cc             ← Linux实现（termios + pselect）
│   └── src/impl/list_ports_linux.cc ← Linux串口设备枚举
│
├── fdilink_ahrs_ROS2/               ← FDILink AHRS/IMU/GPS 传感器驱动
│   ├── src/ahrs_driver.cpp          ← 主驱动节点（串口帧解析+坐标变换）
│   ├── src/imu_tf.cpp               ← IMU→TF广播节点
│   ├── include/fdilink_data_struct.h← 二进制协议结构体定义
│   ├── include/crc_table.h          ← CRC8/CRC16校验表
│   ├── config/ahrs_params.yaml      ← 串口参数配置（/dev/FDI_IMU_GNSS, 921600）
│   └── launch/ahrs_driver.launch.py ← 驱动启动文件
│
├── can_bridge/                      ← CAN总线通信桥梁
│   ├── src/can_bridge_node.cpp      ← 单节点：ROS2↔CAN双向转换
│   ├── include/can_bridge/controlcan.h ← ZLG VCI API C头文件
│   ├── lib/{arm64,x86_64}/          ← 预编译libcontrolcan.so
│   └── launch/can_bridge.launch.py  ← 条件启动（防重复）
│
├── lidar_ros2/                      ← 雷神激光雷达驱动
│   └── lslidar_ros/
│       ├── lslidar_msgs/            ← 自定义消息+服务定义
│       │   ├── msg/LslidarPacket.msg ← 原始数据包(1212字节)
│       │   ├── msg/LslidarScan.msg   ← 扫描数据
│       │   └── srv/*.srv            ← 8个控制服务
│       └── lslidar_driver/          ← 驱动核心
│           ├── src/lslidar_driver.cpp     ← 1698行主驱动（poll+解码+发布）
│           ├── src/input.cpp              ← UDP/PCAP输入抽象层
│           ├── params/lslidar_cx.yaml     ← 雷达网络配置
│           └── launch/lslidar_cx_launch.py
│
├── wtb_car_driver/                  ← 底盘驱动（系统"小脑"）
│   ├── src/wtb_car.cpp              ← 701行完整驱动（协议+里程计+控制）
│   ├── msg/CarMsg.msg               ← 自定义消息(speed+angle+battery)
│   ├── config/ekf_wtb_fdimu.yaml    ← EKF融合配置（odom+IMU）
│   ├── urdf/wtb_car.xacro           ← 机器人3D模型（TF树骨架）
│   └── launch/start_wtb_car_fdimu.launch.py ← 底层总启动文件
│
├── my_cartographer/                 ← Cartographer SLAM配置
│   ├── config/cartographer.lua      ← 建图参数（前端+后端+回环）
│   ├── config/localization_2d.lua   ← 纯定位配置
│   └── launch/cartographerAll.launch.py ← 建图总启动（含底层驱动）
│
└── my_navigation2/                  ← Nav2自主导航配置
    ├── param/wtb_nav2_params.yaml   ← 导航参数（AMCL+Planner+Controller）
    ├── maps/map_new.{pgm,yaml}      ← 预存地图
    ├── scripts/qt_nav2.py           ← PyQt5导航GUI
    ├── scripts/trafficLight_nav2.py ← 路径跟踪+交通灯控制
    └── launch/wtb_navigation2_fdimu.launch.py ← 导航总启动
```

## 2.2 7个包的依赖关系图

```
                          ┌─────────────────────────┐
                          │   [外部系统依赖]          │
                          │   robot_localization    │  ← EKF融合
                          │   nav2_bringup          │  ← 导航引擎
                          │   cartographer_ros      │  ← SLAM引擎
                          │   pointcloud_to_laserscan│ ← 3D→2D
                          │   joint/robot_state_publ.│ ← TF发布
                          │   rviz2                 │  ← 可视化
                          └──────────┬──────────────┘
                                     │ (运行时依赖)
                                     ▼
        ┌──────────────────────────────────────────────────────┐
        │                   [应用算法层]                        │
        │                                                      │
        │  my_navigation2 ──→ my_cartographer                  │
        │  (自主导航)          (SLAM建图)                       │
        │        │                  │                           │
        │        ├── 订阅 /ekf_odom, /scan                     │
        │        ├── 发布 /cmd_vel                             │
        │        └── 加载 .pgm地图                             │
        └────────┬──────────────┬──────────────────────────────┘
                 │              │
                 ▼              ▼
        ┌──────────────────────────────────────────────────────┐
        │                   [融合层]                            │
        │                                                      │
        │         EKF (robot_localization)                     │
        │         融合 /car_odom + /imu → /ekf_odom             │
        │         配置来自 wtb_car_driver/config/               │
        └────────────────────┬─────────────────────────────────┘
                             │
         ┌───────────────────┼───────────────────┐
         ▼                   ▼                   ▼
┌───────────────────────────────────────────────────────────────┐
│                        [驱动层]                                │
│                                                                │
│  wtb_car_driver          fdilink_ahrs          lslidar_driver │
│  (底盘驱动)              (IMU/GPS驱动)          (激光雷达驱动)  │
│  ├─发布 /car_odom        ├─发布 /imu           ├─发布 /point_  │
│  ├─发布 /wtb_car_message ├─发布 /euler_angles    cloud_raw    │
│  ├─发布 TF odom→base_    ├─发布 /mag_pose_2d   └─加载点云数据  │
│  │  footprint            ├─发布 /gps/fix         ↓              │
│  ├─订阅 /cmd_vel         ├─发布 /NED_odometry   pointcloud_    │
│  └─订阅 can_tx_2         └─发布 /magnetic       to_laserscan   │
│  ┌─发布 can_rx_2                                    │         │
│  └─订阅 can_tx_2                                    ▼         │
│                                                    /scan      │
└──────────────┬───────────────────┬────────────────┬───────────┘
               │                   │                 │
               ▼                   ▼                 │
        ┌──────────────────────────────────────────────┐
        │              [硬件抽象层]                      │
        │                                               │
        │  serial (纯C++库)      can_bridge              │
        │  ├─串口 open/read/write├─VCI_OpenDevice       │
        │  ├─115200-921600波特  ├─VCI_InitCAN          │
        │  ├─termios+pselect    ├─VCI_Transmit/Receive  │
        │  └─被fdilink_ahrs调用  ├─订阅 can_rx_2        │
        │                       ├─发布 can_tx_2         │
        │                       └─CAN通道0/1管理        │
        └───────────┬───────────────┬──────────────────┘
                    │               │
                    ▼               ▼
        ┌──────────────────────────────────────────────┐
        │              [物理硬件]                        │
        │                                               │
        │  /dev/FDI_IMU_GNSS    ZLG USB-CAN盒子         │
        │  (CP2102 USB-UART)    (USB 2.0)               │
        │       │                    │                   │
        │       ▼                    ▼                   │
        │  FDILink IMU模块      CAN总线 (500kbps)       │
        │  (9轴+GPS)           │                        │
        │                      ▼                        │
        │                 WTB底盘MCU                    │
        │                 (电机+转向舵机)                │
        │                                               │
        │  ┌─────────────────────────────┐              │
        │  │ 雷神C16 激光雷达             │              │
        │  │ 以太网 UDP 2368/2369        │              │
        │  └─────────────────────────────┘              │
        └──────────────────────────────────────────────┘
```

## 2.3 4层架构分层图

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                   │
│   📊 应用层 (Application Layer)                                   │
│   ┌─────────────────────┐  ┌─────────────────────────────────┐   │
│   │  my_cartographer    │  │  my_navigation2                  │   │
│   │  • cartographer.lua │  │  • Nav2 Bringup                  │   │
│   │  • SLAM建图启动      │  │  • AMCL定位 + 路径规划+ 行为树   │   │
│   │  • 发布 /map        │  │  • 订阅 /goal_pose → /cmd_vel   │   │
│   └─────────┬───────────┘  └───────────────┬──────────────────┘   │
│             │                               │                      │
│             └───────────┬───────────────────┘                      │
│                         │  /ekf_odom  /scan  /map                 │
│                         ▼                                          │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│   🔗 融合层 (Fusion Layer)                                        │
│   ┌──────────────────────────────────────────────────────────┐   │
│   │  EKF (robot_localization)                                 │   │
│   │  • 配置: ekf_wtb_fdimu.yaml                               │   │
│   │  • 输入: /car_odom (vx,vy,wz) + /imu (wz)                │   │
│   │  • 输出: /ekf_odom (融合里程计)                           │   │
│   │  • 输出: TF odom → base_footprint                         │   │
│   │  • 频率: 30Hz, 2D模式                                     │   │
│   └──────────────────────────────────────────────────────────┘   │
│                         ▲          ▲                               │
│                 /car_odom          /imu                            │
│                         │          │                               │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│   🔌 驱动层 (Driver Layer)                                        │
│                                                                   │
│   ┌──────────────┐  ┌───────────────┐  ┌─────────────────────┐   │
│   │ wtb_car_driver│  │ fdilink_ahrs   │  │ lslidar_driver      │   │
│   │              │  │               │  │                     │   │
│   │ 主要工作:    │  │ 主要工作:      │  │ 主要工作:           │   │
│   │ ①CAN协议编解 │  │ ①串口数据解析  │  │ ①UDP数据包接收      │   │
│   │  码(底盘)    │  │ ②CRC校验       │  │ ②点云解码+发布      │   │
│   │ ②阿克曼里程计 │  │ ③坐标变换      │  │ ③型号自动检测       │   │
│   │ ③底盘控制    │  │ ④发布IMU话题   │  │ ④服务控制接口       │   │
│   │ ④发布TF      │  │ ⑤发布GPS话题   │  │                     │   │
│   │              │  │               │  │                     │   │
│   │ 输出:        │  │ 输出:         │  │ 输出:               │   │
│   │ /car_odom    │  │ /imu          │  │ /point_cloud_raw    │   │
│   │ /wtb_car_msg │  │ /euler_angles │  │ /scan_raw           │   │
│   │ TF树         │  │ /gps/fix      │  │                     │   │
│   │              │  │ /NED_odometry │  │                     │   │
│   │              │  │ /mag_pose_2d  │  │                     │   │
│   │              │  │ /magnetic     │  │                     │   │
│   │              │  │ /system_speed │  │                     │   │
│   │              │  │               │  │                     │   │
│   │ 输入:        │  │ 输入:         │  │ 输入:               │   │
│   │ /cmd_vel     │  │ 串口数据流     │  │ UDP数据流           │   │
│   │ can_tx_2     │  │               │  │                     │   │
│   └──────┬───────┘  └───────┬───────┘  └─────────────────────┘   │
│          │                  │                                     │
│          ▼                  ▼                                     │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│   🖧 硬件抽象层 (HAL Layer)                                       │
│                                                                   │
│   ┌──────────────────────────┐  ┌────────────────────────────┐   │
│   │  can_bridge              │  │  serial (纯C++库)           │   │
│   │  • VCI_OpenDevice        │  │  • POSIX termios            │   │
│   │  • VCI_InitCAN (500kbps) │  │  • pselect() 阻塞IO        │   │
│   │  • VCI_StartCAN          │  │  • 波特率: 921600/8N1      │   │
│   │  • VCI_Transmit (发送)   │  │  • 超时控制                │   │
│   │  • VCI_Receive (接收)    │  │  • 端口枚举                │   │
│   │  • 设备掉线自动重连       │  │  • CRC校验(由上层调用)     │   │
│   │  • 双通道 (0/1)          │  │  • 线程安全 (pthread_mutex) │   │
│   │                          │  │                            │   │
│   │  链路: libcontrolcan.so  │  │  链路: libserial.so        │   │
│   │        ──USB──►          │  │        ──USB-UART──►       │   │
│   │        ZLG USB-CAN Box   │  │        CP2102芯片          │   │
│   │        ──CAN_H/CAN_L──►  │  │        ──UART──►           │   │
│   │        WTB底盘MCU        │  │        FDILink IMU模块     │   │
│   └──────────────────────────┘  └────────────────────────────┘   │
│                                                                   │
│   ┌──────────────────────────────────────────────────────────┐   │
│   │  lslidar_driver 内置的 Input 抽象层                        │   │
│   │  • InputSocket: UDP Socket (端口2368/2369)                │   │
│   │  • InputPCAP:   PCAP文件离线回放                          │   │
│   │  • 多播/单播支持                                           │   │
│   │  • poll()超时: 3000ms                                     │   │
│   │       ──以太网──► 雷神C16激光雷达                         │   │
│   └──────────────────────────────────────────────────────────┘   │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

## 2.4 系统数据流全景图

```
Rviz2 / 脚本
├─ 2D Pose Estimate（初始位姿）
├─ Nav2 Goal 设定（目标点/姿态）
├─ 地图显示 / 路径可视化 / 机器人实时位姿
│
▼ 发布 /goal_pose，提供 TF (map→odom)

my_navigation2 (导航层)
├─ Nav2 Bringup 核心组件
│  ├─ AMCL 粒子滤波
│  │  ├─ 输入：/scan, /map, /ekf_odom
│  │  └─ 输出：map→odom 的 TF 变换
│  ├─ BT Navigator（行为树引擎）
│  │  ├─ ① ComputePathToPos（全局规划器）
│  │  ├─ ② FollowPath（DWB 局部控制器）
│  │  └─ ③ Recovery（异常恢复行为）
│  └─ Costmap 2D（代价地图）
│     ├─ global_costmap：50m×50m 静态栅格
│     ├─ local_costmap：5m×5m 滚动窗口
│     ├─ 输入：/scan, /map, /ekf_odom
│     └─ 用于障碍物避障与路径规划
│
▼ 输出 /cmd_vel（线速度+角速度）及 /plan

传感器数据流层
├─ pointcloud_to_laserscan（3D点云转2D激光）
│  ├─ 输入：/point_cloud_raw（雷神 C16）
│  ├─ 高度过滤：[-0.75m, 0.5m]
│  ├─ 角度范围：[-π, π]
│  ├─ 测距范围：[0.5m, 50m]
│  └─ 输出：/scan（供 AMCL 和 Costmap 使用）
│
└─ EKF（robot_localization 扩展卡尔曼滤波）
   ├─ 输入1：/car_odom（里程计）→ vx, vy, wz
   ├─ 输入2：/imu（IMU 数据）→ wz（角速度）
   ├─ 输出：/ekf_odom（融合后的里程计）
   └─ 输出 TF：odom → base_footprint

▼ 驱动层接收 /cmd_vel，并发布原始传感器数据

驱动层（ROS2 节点）
├─ lslidar_driver（雷神 C16 激光雷达）
│  ├─ poll() 主线程：UDP 读取（192.168.1.200:2368/2369）
│  ├─ decodePacket()：极坐标→笛卡尔坐标
│  ├─ difopPoll() 线程：识别型号，提取垂直角表
│  ├─ 8 个控制服务（起停/转速/点云数据）
│  └─ 发布：/point_cloud_raw
│
├─ wtb_car_driver（WTB 阿克曼底盘驱动）
│  ├─ can_tx_callback()：接收底盘反馈（CAN Rx 0x18C4D2EF）
│  ├─ ParseCtrlFb()：解析档位、速度、转向角、电量
│  ├─ 发布：/car_odom（阿克曼里程计）
│  ├─ 发布：CarMsg（自定义底盘消息）
│  ├─ CmdVelCallback()：订阅 /cmd_vel
│  ├─ SendSpeedToAKM()：将速度/转向角编码为 CAN 帧
│  ├─ BuildCtrlCmdData()：构建控制帧（Tx 0x18C4D2D0）
│  └─ 发布：can_rx_2（供 can_bridge 发送）
│
└─ fdilink_ahrs（FDILink 惯导模块）
   ├─ processLoop()：串口帧同步（/dev/FDI_IMU_GNSS）
   ├─ CRC8/16 校验，坐标变换
   ├─ 输出：/imu（200Hz，四元数/欧拉角）
   └─ 输出：/gps（位置信息）

▼ 通过硬件抽象层与物理硬件交互

硬件抽象层
├─ 以太网口（UDP）
│  └─ 192.168.1.200:2368/2369 → lslidar_driver
│
├─ can_bridge（ROS2 CAN 桥接节点）
│  ├─ can_receive_loop() 线程（20Hz/50ms）
│  │  └─ VCI_Receive() 从 CAN 总线接收 → 发布 can_tx_2
│  ├─ can_rx_callback_2()（事件驱动，ROS2→CAN）
│  │  └─ VCI_Transmit() 发送控制命令
│  ├─ 设备管理：VCI_OpenDevice/InitCAN，通道1（底盘）500kbps
│  ├─ 通道0：空闲
│  └─ 特性：设备掉线自动重连
│
└─ serial（纯 C++ 串口库）
   ├─ serial::Serial 类：.open(), .read(), .write(), .close()
   ├─ 波特率：921600 bps，8N1
   └─ 对应 USB 转串口芯片：CP2102（VID:10c4 PID:ea60）

▼ 物理硬件层

物理硬件层
├─ 雷神 C16 激光雷达
│  ├─ 水平 FOV：360°，垂直 FOV：±15°
│  ├─ 测距精度：±3cm，范围 0.05m～200m
│  ├─ 转速：5～20Hz（可配置）
│  └─ 网络：UDP 包，含极坐标点云
│
├─ ZLG USB-CAN 盒子（USBCAN-2A，Dev=4）
│  ├─ CAN0：空闲
│  ├─ CAN1：连接 WTB 底盘 MCU，波特率 500kbps
│  ├─ CAN ID 定义
│  │  ├─ 控制帧 Tx：0x18C4D2D0
│  │  ├─ 反馈帧 Rx：0x18C4D2EF
│  │  └─ 电量/状态帧 Rx：0x18C4E2EF
│  └─ 通信方式：双路 CAN，支持同步收发
│
├─ FDILink AHRS/IMU/GPS 模块
│  ├─ 传感器：陀螺仪 + 加速度计 + 磁力计 + GPS + 气压计 + 温度
│  ├─ 输出频率：200Hz（IMU 数据）
│  ├─ 输出姿态：四元数（原生：前-右-下 → 转换后：东-北-天）
│  └─ 串口参数：921600 bps，8N1，设备节点 /dev/FDI_IMU_GNSS
│
└─ WTB 阿克曼底盘
   ├─ 轴距：0.82m
   ├─ 驱动方式：后轮差速（驱动） + 前轮转向（阿克曼几何）
   ├─ 最大速度：1.0 m/s（可配置）
   ├─ 最小有效速度：0.005 m/s（低于此值为噪声阈值）
   ├─ 转向范围：尚未标定（默认 ±30° 左右）
   ├─ 电池电量：位于 CAN ID 0x18C4E2EF 的 Byte0
   ├─ 档位：disable / P / R / N / D
   │
   ├─ CAN 控制帧（Tx 0x18C4D2D0）格式（8字节）
   │  ├─ Byte0：档位[0:3] + 速度低4位[3:0]
   │  ├─ Byte1：速度中8位[11:4]
   │  ├─ Byte2：速度高4位[15:12] + 转向角低4位[3:0]
   │  ├─ Byte3：转向角中8位[11:4]
   │  ├─ Byte4：转向角高4位[15:12]
   │  ├─ Byte5：保留
   │  ├─ Byte6：AliveCounter[0:4]（心跳计数）
   │  └─ Byte7：BCC 异或校验（所有字节异或）
   │
   └─ CAN 反馈帧（Rx 0x18C4D2EF）格式
      ├─ Byte0：档位[0:3] + 速度低4位[3:0]
      ├─ Byte1：速度中8位[11:4]
      ├─ Byte2：速度高4位[15:12] + 实际转向角低4位[3:0]
      ├─ Byte3：实际转向角中8位[11:4]
      ├─ Byte4：实际转向角高4位[15:12]
      └─ Byte5-7：保留
```

## 2.5 核心话题通信矩阵

```
┌──────────────────────────────────────────────────────────────────────┐
│                         ROS2 话题通信矩阵                             │
│                                                                       │
│  话题名              │  消息类型              │  发布者        → 订阅者│
│ ─────────────────────┼────────────────────────┼───────────────────────│
│  /cmd_vel            │  Twist                 │  Nav2 → wtb_car_driver│
│  /twist_cmd          │  TwistStamped          │  Nav2 → wtb_car_driver│
│  /can_rx_2           │  can_msgs/Frame        │  wtb_car → can_bridge │
│  /can_tx_2           │  can_msgs/Frame        │  can_bridge → wtb_car │
│  /car_odom           │  Odometry              │  wtb_car → EKF        │
│  /wtb_car_message    │  CarMsg                │  wtb_car → (监控)     │
│  /imu                │  Imu                   │  ahrs → EKF           │
│  /ekf_odom           │  Odometry              │  EKF → Nav2/Carto     │
│  /point_cloud_raw    │  PointCloud2           │  lslidar → pcl2laser  │
│  /scan               │  LaserScan             │  pcl2laser → Nav2/Carto│
│  /map                │  OccupancyGrid         │  Carto → Nav2/AMCL    │
│  /plan               │  Path                  │  Nav2 → Rviz          │
│  /goal_pose          │  PoseStamped           │  Rviz/GUI → Nav2      │
│  /amcl_pose          │  PoseWithCovStamped    │  AMCL → (监控)        │
│  /global_costmap     │  OccupancyGrid         │  Nav2 → Rviz          │
│  /local_costmap      │  OccupancyGrid         │  Nav2 → Rviz          │
│  /euler_angles       │  Vector3               │  ahrs → (监控)        │
│  /gps/fix            │  NavSatFix             │  ahrs → (监控)        │
│  /run_static         │  String                │  (手动) → wtb_car     │
│  /robot_description  │  (参数服务)             │  rsp → TF             │
│                                                                       │
│ ─────────────────────┼────────────────────────┼───────────────────────│
│                      TF 变换                  │                       │
│  odom → base_footprint│                       │  EKF → 全局           │
│  base_footprint → base_link│                  │  robot_state_publisher│
│  base_link → laser   │                        │  robot_state_publisher│
│  base_link → gyro_link│                       │  robot_state_publisher│
│  base_link → 左前/右前/左后/右后轮│              │  robot_state_publisher│
│  map → odom          │                        │  AMCL (导航模式)      │
│  map → odom          │                        │  Cartographer (建图)  │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

# 四、逐包深度分析

## 包1：`can_bridge` — CAN通信桥梁

**一句话定位**：硬件抽象层，把 USB-CAN 硬件的数据流转换为 ROS2 的话题消息。

**包结构**：

```
can_bridge/
├── src/can_bridge_node.cpp    ← 核心：单节点、单类，414行
├── include/can_bridge/controlcan.h  ← ZLG VCI API 的 C 头文件声明
├── lib/{arm64,x86_64}/        ← 预编译的 libcontrolcan.so/.a
├── launch/can_bridge.launch.py
├── env-hooks/                 ← 自动注入 LD_LIBRARY_PATH
└── CMakeLists.txt, package.xml
```

**核心代码详解 — 数据流**：

```
          ROS2 话题                        CAN 总线
     ┌─────────────────┐           ┌─────────────────┐
     │  can_rx_1 (订阅)  │────────→│ VCI_Transmit()   │──→ CAN通道1
     │  can_rx_2 (订阅)  │────────→│ VCI_Transmit()   │──→ CAN通道2
     │                  │           │                  │
     │  can_tx_1 (发布)  │←────────│ VCI_Receive()    │←── CAN通道1
     │  can_tx_2 (发布)  │←────────│ VCI_Receive()    │←── CAN通道2
     └─────────────────┘           └─────────────────┘
```

**关键代码1 — 初始化CAN设备**（can_bridge_node.cpp:73-126）：

```cpp
void Init_Can()
{
    // ① 打开设备：类型=4(USBCAN-2A)，索引=0
    dwRel = VCI_OpenDevice(DevType, DevIndex, 1);

    // ② 配置CAN参数
    VCI_INIT_CONFIG vic;
    vic.AccCode = 0x80000008;   // 接收所有帧的掩码
    vic.AccMask = 0xFFFFFFFF;
    vic.Filter = 1;             // 单滤波模式
    vic.Timing0 = 0x00;         // 波特率 500kbps
    vic.Timing1 = 0x1c;         //    (BS1=6, BS2=2, Prescaler=4)
    vic.Mode = 0;               // 正常模式

    // ③ 初始化两个CAN通道
    VCI_InitCAN(DevType, DevIndex, 0, &vic);  // CAN通道0
    VCI_StartCAN(DevType, DevIndex, 0);

    VCI_InitCAN(DevType, DevIndex, 1, &vic);  // CAN通道1
    VCI_StartCAN(DevType, DevIndex, 1);

    can_f = 1;  // 标记设备已就绪
}
```

**关键的 CAN 波特率计算**：`Timing0=0x00, Timing1=0x1c` 在 ZLG 驱动中对应：

- SJW=1, BRP=4 (即 36MHz / 4 = 9MHz)
- Tseg1=7, Tseg2=2 → 总共 10 个时间量子
- 波特率 = 9MHz / 10 = **900kbps**（实际应用中与底盘 500kbps 匹配需要微调）

**关键代码2 — 接收线程**（can_bridge_node.cpp:252-263）：

```cpp
void can_receive_loop()
{
    while (rclcpp::ok()) {
        std::this_thread::sleep_for(std::chrono::milliseconds(50)); // 20Hz
        can_receive_parse();  // 从两个CAN通道读取数据
    }
}
```

这个 20Hz 的接收频率意味着：**每 50ms 从 CAN 总线读取一批数据并发布到 ROS2**。对于底盘控制反馈（通常 50-100Hz），这个频率足够。

**关键代码3 — 数据双向转换**（can_bridge_node.cpp:204-215 和 can_bridge_node.cpp:266-352）：

ROS2消息 → CAN发送（回调）：

```cpp
void can_rx_callback_1(const can_msgs::msg::Frame::SharedPtr msg)
{
    VCI_CAN_OBJ vco;
    vco.ID = msg->id;                    // CAN ID
    vco.ExternFlag = msg->is_extended;   // 标准帧/扩展帧
    vco.RemoteFlag = msg->is_rtr;        // 数据帧/远程帧
    vco.DataLen = msg->dlc;              // 数据长度
    memcpy(vco.Data, msg->data.begin(), 8); // 8字节数据
    VCI_Transmit(DevType, DevIndex, CANIndex, &vco, 1);
}
```

CAN接收 → ROS2消息发布（轮询）：

```cpp
int reclen = VCI_Receive(DevType, DevIndex, CANIndex, rec, recvNum, 0);
for(int j = 0; j < reclen; j++) {
    auto msg = std::make_shared<can_msgs::msg::Frame>();
    msg->id = rec[j].ID;
    msg->dlc = rec[j].DataLen;
    std::copy(rec[j].Data, rec[j].Data + rec[j].DataLen, msg->data.begin());
    can_pub_1->publish(*msg);
}
```

### C++、Python实现方法总结对比表

|方面|C++ 实现|Python 实现|
|---|---|---|
|节点定义|`class CanBridgeNode : public rclcpp::Node`|`class CanBridgeNode(Node)`|
|发布者创建|`create_publisher<MsgType>("topic", 10)`|`create_publisher(MsgType, "topic", 10)`|
|订阅者创建|`create_subscription<MsgType>("topic", 10, bind(...))`|`create_subscription(MsgType, "topic", cb, 10)`|
|消息类型|模板参数，编译时确定|普通参数，运行时动态|
|消息分配|`std::make_shared<MsgType>()`|`MsgType()`|
|数据字段|`std::array<uint8_t, 8>` 等|`list` / `bytes`|
|多线程|手动创建 `std::thread`|多用 `create_timer` 或 `threading`|
|内存管理|智能指针 `SharedPtr`|自动垃圾回收|
|编译|需要编译|脚本直接运行|
|性能|高，适合实时处理|较低，适合快速开发和高层逻辑|

---

## 包2：`wtb_car_driver` — 底盘驱动和控制

**一句话定位**：这是系统的"小脑"，负责与底盘 CAN 协议通信、计算阿克曼里程计、发布坐标变换。

**包结构**：

```
wtb_car_driver/
├── src/wtb_car.cpp             ← 完整版驱动节点 (701行)
├── src/wtb_car_only.cpp        ← 简化版 (568行，无启停控制)
├── include/controlcan.h        ← ZLG CAN 驱动头文件
├── msg/CarMsg.msg              ← 自定义消息：speed + angle + battery
├── config/ekf_wtb_fdimu.yaml   ← EKF 融合配置
├── launch/start_wtb_car_fdimu.launch.py ← 总启动文件
├── urdf/wtb_car.xacro          ← 机器人3D模型
└── CMakeLists.txt, package.xml
```

### `wtb_car_driver`与`can_bridge`包的关系

```
┌──────────────────────────────────────────────────────────────────┐
│                         ROS2 应用层                               │
│                                                                   │
│   /cmd_vel (上层导航/遥操作发来的速度指令)                         │
│      │                                                            │
│      ▼                                                            │
│ ┌────────────────────────────────────────────────────────────────┐│
│ │               wtb_car_driver (底盘驱动包)                      ││
│ │                                                                ││
│ │   ┌────────────┐   ┌──────────────┐   ┌──────────────────┐   ││
│ │   │ 协议编解码  │   │ 阿克曼里程计  │   │ 启停状态控制     │   ││
│ │   │ BuildCmd/   │   │ Calculate&   │   │ run_static       │   ││
│ │   │ ParseFb     │   │ Published    │   │ callback         │   ││
│ │   └──────┬──────┘   └──────┬───────┘   └──────────────────┘   ││
│ │          │                 │                                    ││
│ │   发布到 can_rx_2    发布 /car_odom + /wtb_car_message         ││
│ └─────────┬──────────────────────────────────────────────────────┘│
│           │                                                       │
│           │  can_rx_2 (ROS2 Topic, 类型: can_msgs/msg/Frame)     │
│           │                                                       │
│           ▼                                                       │
│ ┌────────────────────────────────────────────────────────────────┐│
│ │               can_bridge (CAN通信桥梁包)                       ││
│ │                                                                ││
│ │   ┌──────────────────┐        ┌────────────────────┐         ││
│ │   │ can_rx_callback_2 │        │  can_receive_loop  │         ││
│ │   │ (ROS2→CAN 发送)   │        │  (CAN→ROS2 接收)   │         ││
│ │   └────────┬──────────┘        └────────┬───────────┘         ││
│ │            │                            │                      ││
│ │     VCI_Transmit()               VCI_Receive()                 ││
│ └────────────┬───────────────────────────┬──────────────────────┘│
│              │                           │                       │
│              ▼                           ▼                       │
│    ┌─────────────────────────────────────────────┐               │
│    │         libcontrolcan.so (ZLG 动态库)        │               │
│    └────────────────────┬────────────────────────┘               │
│                         │ USB                                     │
│                    ┌────▼────┐                                    │
│                    │ ZLG USB- │                                    │
│                    │  CAN盒子  │                                    │
│                    └────┬────┘                                    │
│                         │ CAN_H, CAN_L (500kbps)                   │
│                         │                                         │
│                    ┌────▼────┐                                    │
│                    │ WTB 底盘  │                                    │
│                    │ 电机+舵机 │                                    │
│                    └─────────┘                                    │
└──────────────────────────────────────────────────────────────────┘
```

**一句话说清楚关系**：`wtb_car_driver` 是"翻译官"，把 ROS2 的标准速度指令翻译成底盘 CAN 协议；`can_bridge` 是"邮递员"，负责把翻译好的 CAN 数据帧真正通过 USB-CAN 硬件发到物理总线上去。

### 数据流全过程：一次"前进0.5m/s"命令的完整旅程

```
时间线 ──────────────────────────────────────────────────────────→

步骤1: 上层发出命令
┌─────────────────────────────────────────────────────────────────┐
│ ros2 topic pub /cmd_vel "{linear: {x: 0.5}, angular: {z: 0.0}}"│
│                                                                  │
│ 或者 Nav2 的 DWB Controller 自动发布 /cmd_vel                   │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
                                   ▼   ROS2 DDS 网络
                                   │
步骤2: wtb_car_driver 接收并处理
┌──────────────────────────────────────────────────────────────────┐
│ CmdVelCallback()                    [wtb_car.cpp:380-413]        │
│   ├─ 检查 enable_run_ 是否为 true (启停状态)                     │
│   ├─ 限速: if (v > max_speed_) v = max_speed_                    │
│   └─ 调用 SendSpeedToAKM(0.5, 0.0)                               │
│                                                                   │
│ SendSpeedToAKM(0.5, 0.0)           [wtb_car.cpp:334-366]        │
│   ├─ 档位判断: speed>0 → "D"档                                   │
│   ├─ 调用 BuildCtrlCmdData("D", 0.5, 0.0)                       │
│   │   └─ 编码 CAN 帧: ID=0x18C4D2D0, 8字节数据                  │
│   └─ 发布到 ROS2 话题 can_rx_2 (3次!!)                           │
│                                                                   │
│ 此时 can_rx_2 话题上出现一条消息:                                │
│   can_msgs/msg/Frame {                                           │
│     id: 0x18C4D2D0                                               │
│     dlc: 8                                                       │
│     is_extended: true                                            │
│     data: [0x40, 0x1F, 0x40, 0x00, 0x00, 0x00, 0x00, 0x1F]     │
│   }                                                              │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
                                   ▼   ROS2 DDS 网络
                                   │
步骤3: can_bridge 接收并转发到物理CAN
┌──────────────────────────────────────────────────────────────────┐
│ can_rx_callback_2()               [can_bridge_node.cpp:227-239]  │
│   ├─ 从 ROS2 Frame 提取字段: ID, Data, DLC, ExternFlag...       │
│   ├─ 填充 VCI_CAN_OBJ 结构体                                     │
│   └─ 调用 VCI_Transmit(DevType=4, DevIndex=0, CANIndex=1, &vco) │
│                                                                   │
│ VCI_Transmit 通过 libcontrolcan.so 驱动 USB-CAN 盒子             │
│ USB-CAN 盒子在 CAN 总线上发出差分信号                            │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
                                   ▼   CAN 总线差分信号 (500kbps)
                                   │
步骤4: 底盘接收并执行
┌──────────────────────────────────────────────────────────────────┐
│ WTB 底盘 MCU 收到 CAN ID 0x18C4D2D0                              │
│   ├─ 解析: 档位=D, 速度=0.5m/s, 转向角=0°                       │
│   ├─ 验证: BCC异或校验通过                                        │
│   ├─ 验证: AliveCounter 连续变化 (说明上位机在线)                 │
│   └─ 执行: 驱动电机以 0.5m/s 前进                                 │
│                                                                   │
│ 同时，底盘通过 CAN ID 0x18C4D2EF 反馈实际状态                   │
│   速度=0.49m/s, 转向角=0.5°, 档位=D                             │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
                                   ▼   CAN 总线差分信号
                                   │
步骤5: can_bridge 接收反馈
┌──────────────────────────────────────────────────────────────────┐
│ can_receive_loop() 线程 (20Hz)   [can_bridge_node.cpp:252-263]  │
│   └─ can_receive_parse()        [can_bridge_node.cpp:266-352]   │
│       ├─ VCI_Receive(DevType, DevIndex, CANIndex2, rec, 2500)   │
│       ├─ 遍历收到的 CAN 帧                                       │
│       ├─ 逐帧转换为 can_msgs::msg::Frame                         │
│       └─ can_pub_2->publish(msg)  → 发布到 can_tx_2 话题        │
│                                                                   │
│ 此时 can_tx_2 话题上出现反馈消息:                                │
│   can_msgs/msg/Frame {                                           │
│     id: 0x18C4D2EF                                               │
│     dlc: 8                                                       │
│     data: [0x41, 0xEA, 0x00, 0x32, 0x00, ...]                  │
│   }                                                              │
└──────────────────────────────────┬──────────────────────────────┘
                                   │
                                   ▼   ROS2 DDS 网络
                                   │
步骤6: wtb_car_driver 解析反馈 + 计算里程计
┌──────────────────────────────────────────────────────────────────┐
│ can_tx_callback()                [wtb_car.cpp:180-215]          │
│   ├─ 判断 msg->id == 0x18C4D2EF (控制反馈)                     │
│   ├─ 调用 ParseCtrlFb(msg->data, gear, speed, angle)           │
│   │   └─ 解码出: gear="D", speed=0.49, angle=0.0087rad(≈0.5°) │
│   ├─ 缓存 latest_linear_velocity, latest_steering_angle        │
│   │                                                              │
│   └─ 调用 CalculateAndPublished()   [wtb_car.cpp:528-591]      │
│       ├─ dt = now - last_time                                    │
│       ├─ v = 0.49 * vel_scale_, delta = 0.0087 - steer_offset   │
│       ├─ ω = v * tan(delta) / WHEELBASE                         │
│       ├─ x += v*cos(θ)*dt, y += v*sin(θ)*dt, θ += ω*dt        │
│       └─ publishOdom(x, y, θ, v, ω) → 发布 /car_odom           │
│                                                                   │
│ 此时 /car_odom 话题上出现:                                       │
│   nav_msgs/Odometry {                                            │
│     pose.pose.position: {x: 0.5, y: 0.0, z: 0.0}               │
│     twist.twist.linear: {x: 0.49}                               │
│   }                                                              │
└──────────────────────────────────────────────────────────────────┘
```

### CAN协议编解码

底盘使用 CAN 扩展帧（29位ID）通信，协议定义了两个关键 ID：

|CAN ID|方向|作用|周期|
|---|---|---|---|
|`0x18C4D2D0`|PC→底盘|控制命令（档位+速度+转向角+心跳+BCC）|~50Hz|
|`0x18C4D2EF`|底盘→PC|控制反馈（实际档位+速度+转向角）|~100Hz|
|`0x18C4E2EF`|底盘→PC|电池电量|~1Hz|

**控制帧 8 字节编码**（wtb_car.cpp:230-267）：

```
控制命令帧 (ID: 0x18C4D2D0, 8字节)
┌──────────┬──────────┬──────────┬──────────┬──────────┬──────────┬──────────┬──────────┐
│ Byte 0   │ Byte 1   │ Byte 2   │ Byte 3   │ Byte 4   │ Byte 5   │ Byte 6   │ Byte 7   │
├──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│ 挡位[0:3]│ 速度[7:4]│ 速度     │ 角度[3:0]│ 角度     │ 保留     │RollCtr[0 │ BCC校验  │
│ +速度[3:0│ [11:8]   │ [19:12]  │ +角度    │ [15:8]   │          │ :4]      │ (XOR)    │
│ ][4:7]   │          │          │ [7:4]    │          │          │          │          │
└──────────┴──────────┴──────────┴──────────┴──────────┴──────────┴──────────┴──────────┘

编码规则:
  挡位: disable=0, P=1, R=2, N=3, D=4
  速度: unsigned 16bit, 精度 0.001 m/s/bit → 范围 0~65.535 m/s
  转向角: signed 16bit, 精度 0.01°/bit → 范围 -327.68° ~ +327.67°
  心跳计数器: unsigned 4bit, Rolling Counter 0-15
  BCC: Byte0 ^ Byte1 ^ ... ^ Byte6
```

对应的编码代码：

```cpp
// 档位 → Byte0 低4位
data[0] = GearValue & 0xF;

// 速度 int = speed * 1000 → 分布到 Byte0高4, Byte1, Byte2低4
unsigned int SpeedInt = static_cast<unsigned int>(TargetSpeed * 1000);
data[0] |= (SpeedInt & 0x0F) << 4;
data[1] = (SpeedInt & 0x0FF0) >> 4;
data[2] = (SpeedInt & 0xF000) >> 12;

// 转向角 int = angle / 0.01 → 分布到 Byte2高4, Byte3, Byte4低4
int SteerAngleInt = static_cast<int>(TargetSteerAngle / 0.01);
data[2] |= (SteerAngleInt & 0x0F) << 4;
data[3] = (SteerAngleInt & 0x0FF0) >> 4;
data[4] = (SteerAngleInt & 0xF000) >> 12;

// 心跳 + 校验
data[6] = (AliveCounter << 4) & 0xF0;
data[7] = data[0] ^ data[1] ^ data[2] ^ data[3] ^ data[4] ^ data[5] ^ data[6];
```

### 阿克曼里程计计算（wtb_car.cpp:528-591）

```
阿克曼转向几何 (自行车模型)

                 L (WHEELBASE=0.82m)
          ←──────────────────→
          ●──────────────────●
        前轮(转向轮)         后轮(驱动轮)
          ╱                    │
         ╱ δ(steering angle)   │ v (线速度)
        ╱                      │
       ╱                       │
      ╱────────────────────────
     ╱          R = L / tan(δ)
    ╱
   ● 瞬时转向中心

角速度 ω = v / R = v * tan(δ) / L

位置更新（欧拉积分）:
  x_new = x_old + v * cos(θ) * dt
  y_new = y_old + v * sin(θ) * dt
  θ_new = θ_old + ω * dt
```

对应代码：

```cpp
void CalculateAndPublished()
{
    rclcpp::Time current_time = this->get_clock()->now();
    double dt = (current_time - last_time_).seconds();

    // 校准 + 限制
    double v = latest_linear_velocity * vel_scale_;     // 速度校准系数
    double delta = latest_steering_angle - steer_offset_; // 转向角零偏

    if (fabs(v) < 0.005) { v = 0.0; }   // 噪声过滤

    // 阿克曼公式：角速度
    double omega = v * tan(delta) / WHEELBASE;

    // 位置积分
    x_ += v * cos(theta_) * dt;
    y_ += v * sin(theta_) * dt;
    theta_ += omega * dt;
    theta_ = normalizeAngle(theta_);     // 归一化到 [-π, π]
}
```

### 为什么发送3遍命令？（wtb_car.cpp:357-366）

```cpp
for(int i = 0; i < 3; i++)  // 连续发送3帧
{
    std::vector<unsigned char> data = BuildCtrlCmdData(TargetGear, lineSpeed, Angle);
    memcpy(msg.data.data(), data.data(), 8);
    can_pub->publish(msg);
}
```

因为 CAN 总线使用 `best_effort` QoS，不保证送达。发送 3 帧提高了命令抵达底盘的可靠性。每次调用 `BuildCtrlCmdData` 都会让心跳计数器 `AliveCounter` 自动递增，底盘可以据此判断控制器是否还在线。

### EKF 融合配置（ekf_wtb_fdimu.yaml）

```
        /car_odom                      /imu
     (轮式里程计)                   (陀螺仪)
          │                             │
          ▼                             ▼
    ┌─────────────────────────────────────────┐
    │            EKF (ekf_filter_node)         │
    │                                          │
    │  odom0: /car_odom                         │
    │  融合: vx, vy (线速度)  ← 差分模式        │
    │        wz (角速度)                        │
    │                                          │
    │  imu0: /imu                              │
    │  融合: wz (角速度)                        │
    │                                          │
    │  输出: /ekf_odom (融合里程计)             │
    │       odom→base_footprint TF             │
    │       频率: 30Hz                          │
    │       模式: 2D                            │
    └─────────────────────────────────────────┘
```

EKF 同时融合轮式里程计的线速度 + 角速度，以及 IMU 的角速度。**两个传感器互相补充**：里程计提供平移信息（IMU 无法提供），IMU 提供高频率、低延迟的角速度修正（里程计转向角可能有延迟）。

---

## 包3：`lidar_ros2` — 激光雷达驱动

**一句话定位**：激光雷达的"设备驱动程序"，将雷神 C 系列激光雷达的 UDP 数据包转换为 ROS2 点云。

这是整个系统中最复杂的包（核心驱动文件 1698 行），但逻辑清晰：

```
硬件层           驱动层                      输出层
┌─────────┐    ┌──────────────────┐    ┌──────────────────┐
│ 雷神C系列 │    │ LslidarDriver     │    │ /point_cloud_raw │
│ 激光雷达  │    │                  │    │ (sensor_msgs/    │
│         │───→│ ①difopPoll线程   │───→│  PointCloud2)    │
│ UDP 2368 │    │   (识别型号)      │    │                  │
│ (MSOP数据)│    │                  │    │ /scan_raw         │
│         │    │ ②poll线程         │───→│ (sensor_msgs/    │
│ UDP 2369 │    │   (解析数据包)    │    │  LaserScan)      │
│ (DIFOP)  │    │                  │    │                  │
└─────────┘    │ ③publishPointcloud│    └──────────────────┘
               │   (发布线程)       │
               └──────────────────┘
```

关键数据流：

```
UDP原始包 (1206字节/包)
  ↓
解码: azimuth(方位角) + distance(距离) + intensity(强度)
  每组12个Block, 每个Block 32个Firing, 每个Firing对应一个激光通道
  ↓
极坐标→直角坐标:
  x = distance * cos(vertical_angle) * sin(azimuth - 转换角)
  y = distance * cos(vertical_angle) * cos(azimuth - 转换角)
  z = distance * sin(vertical_angle)
  ↓
PCL PointCloud2 发布
```

---

## 包4：`my_cartographer` — SLAM 建图

**一句话定位**：Cartographer 的"配置文件包"，定义了建图时的参数和启动方式。没有源码，只有配置。

**核心配置文件** cartographer.lua 的架构：

```
                 ┌─────────────────────┐
                 │  激光雷达 /scan     │
                 │  里程计 /ekf_odom   │
                 └────────┬────────────┘
                          │
                          ▼
            ┌─────────────────────────┐
            │  Local SLAM (前端)       │
            │  • 运动滤波器 (0.3rad/0.2m) │
            │  • Ceres扫描匹配         │
            │  • 子地图 (30帧/子图)     │
            └────────┬────────────────┘
                     │
                     ▼
            ┌─────────────────────────┐
            │  Global SLAM (后端)      │
            │  • 回环检测 (min_score=0.6)│
            │  • 约束优化 (Huber=100)   │
            │  • 每20节点优化一次       │
            └────────┬────────────────┘
                     │
                     ▼
            ┌─────────────────────────┐
            │  /map 占据栅格地图       │
            │  (cartographer_occupancy │
            │   _grid_node发布)        │
            └─────────────────────────┘
```

Cartographer 核心架构：

```
┌─────────────────────────────────────────────────────────────┐
│                     Cartographer 核心                        │
│                                                              │
│  ┌──────────────────────┐      ┌───────────────────────┐    │
│  │   前端 Local SLAM     │      │   后端 Global SLAM      │    │
│  │   (实时, 与传感器同频)  │ ───→ │   (后台, 4线程池)       │    │
│  │                      │      │                        │    │
│  │  ① 运动滤波           │      │  ① 构建约束(回环检测)   │    │
│  │  ② Ceres 扫描匹配     │      │  ② Sparse Pose Graph   │    │
│  │  ③ 插入子地图         │      │  ③ 全局BA优化          │    │
│  └──────────────────────┘      └───────────────────────┘    │
│                                                              │
│  关键数据结构:                                                │
│  • Submap (子地图): 每30帧激光构成一个，用栅格概率表示        │
│  • Node (节点): 每帧激光在全局坐标系下的位姿                  │
│  • Constraint (约束): 节点之间的相对位姿关系                  │
│    - intra_submap: 前端扫描匹配产生的节点↔子图约束            │
│    - inter_submap: 回环检测产生的子图↔子图约束                │
└─────────────────────────────────────────────────────────────┘
```

**关键参数解读**：

|参数|值|意义|
|---|---|---|
|`motion_filter.max_distance_meters`|0.2|移动超过 20cm 才处理新帧，过滤静止抖动|
|`motion_filter.max_angle_radians`|0.3 (≈17°)|转向超过 17° 才处理新帧|
|`submaps.num_range_data`|30|每 30 帧激光构成一个子地图|
|`constraint_builder.max_constraint_distance`|40m|检测 40 米范围内的回环|
|`optimize_every_n_nodes`|20|每 20 个节点做一次全局 BA 优化|
|`num_background_threads`|4|全局优化用 4 个后台线程|

**核心参数调优指南**（按重要性排序）：

|排名|参数|当前值|调优方向|
|---|---|---|---|
|1|`optimize_every_n_nodes`|20|越小→优化越频繁，精度高但 CPU 高|
|2|`constraint_builder.min_score`|0.60|越小→回环越多但可能误匹配；越大→回环越可靠但可能漏检|
|3|`motion_filter.max_distance_meters`|0.2|越小→采样越密，建图越精细但计算量越大|
|4|`submaps.num_range_data`|30|越大→子图越大，局部建图更稳定但实时性下降|
|5|`ceres_scan_matcher.translation_weight`|80|越大→平移配准越"硬"，不容易被旋转干扰|
|6|`max_constraint_distance`|40m|大场景需要增大，小场景可减小避免错误回环|
|7|`MAP_BUILDER.num_background_threads`|4|根据 CPU 核心数调整|

---

## 包5：`my_navigation2` — 自主导航

**一句话定位**：Nav2 的"部署配置包"。导入地图、配置 AMCL 定位、配置路径规划器和控制器，以及 Python 编写的交互脚本。

**导航架构**：

```
                    ┌──────────────┐
                    │   用户指定    │
                    │  目标点/位姿  │
                    └──────┬───────┘
                           │ /goal_pose
                           ▼
              ┌────────────────────────┐
              │   BT Navigator Server   │
              │   行为树控制器          │
              ├────────────────────────┤
              │  ① ComputePathToPose   │  全局路径规划
              │     (Global Planner)   │  (A* / NavFn)
              ├────────────────────────┤
              │  ② FollowPath          │  局部路径跟踪
              │     (Controller)       │  (DWB / RPP)
              ├────────────────────────┤
              │  ③ Recovery Actions    │  异常恢复
              │     (Spin/Wait/BackUp) │
              └────────────────────────┘
                           │ /cmd_vel
                           ▼
              ┌────────────────────────┐
              │   wtb_car_driver       │
              │   底盘控制              │
              └────────────────────────┘

        并行运行:
              ┌────────────────────────┐
              │   AMCL 粒子滤波定位     │
              │   输入: /scan + /map   │
              │               + /ekf_odom │
              │   输出: map→odom TF    │
              │   粒子数: 5000          │
              └────────────────────────┘
```

---

# 五、实操指南

### 阶段一：启动机器人底层驱动

```bash
# 1. 确认 CAN 设备已连接
lsusb | grep -i "zlg\|usbcan"

# 2. 设置 USB 设备权限
sudo chmod 666 /dev/ttyUSB*

# 3. 启动完整底层驱动（CAN + IMU + LiDAR + 底盘 + EKF）
ros2 launch wtb_car_driver start_wtb_car_fdimu.launch.py
```

此时运行的话题：

```bash
# 查看所有活跃话题
ros2 topic list

# 关键话题验证
ros2 topic echo /car_odom       # 轮式里程计 (应该有数据)
ros2 topic echo /ekf_odom       # 融合里程计
ros2 topic echo /scan           # 2D激光扫描
ros2 topic echo /imu            # IMU数据
ros2 topic echo /wtb_car_message # 车辆状态 (speed, angle, battery)
```

### 阶段二：遥控测试

```bash
# 用键盘控制小车前后左右
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

数据流向：`键盘 → /cmd_vel → wtb_car_driver → CAN协议打包 → can_bridge → CAN总线 → 底盘`

### 阶段三：SLAM建图

```bash
# 启动 Cartographer 建图
ros2 launch my_cartographer cartographerAll.launch.py

# 遥控小车慢速在环境中走一圈
# 开着 Rviz2 观察地图构建过程
```

建图完成后保存地图：

```bash
# Cartographer 保存纯定位状态地图
rosservice call /finish_trajectory 0
rosservice call /write_state "{filename: '/home/robot/my_map.pbstream'}"

# 将 .pbstream 转换为 pgm+yaml (Nav2可用的格式)
ros2 run cartographer_ros cartographer_pbstream_to_ros_map \
  -pbstream_filename /home/robot/my_map.pbstream \
  -map_filestem /home/robot/my_map
```

### 阶段四：自主导航

```bash
# 1. 将生成的 my_map.pgm 和 my_map.yaml 放到 my_navigation2/maps/ 目录下

# 2. 启动导航
ros2 launch my_navigation2 wtb_navigation2_fdimu.launch.py

# 3. 在 Rviz2 中：
#    a) 点击 "2D Pose Estimate" 设置初始位置
#    b) 点击 "Nav2 Goal" 设定目标点
#    c) 观察规划路径和机器人自动行驶

# 或者用 Python 脚本
ros2 run my_navigation2 qt_nav2.py     # PyQt5 GUI 控制
ros2 run my_navigation2 trafficLight_nav2.py  # 交通灯模拟导航
```

### 启动依赖关系总结

```
启动顺序（由 launch 文件自动处理）:

层级1 (必须最先):  can_bridge        ← CAN硬件初始化
层级2 (并行启动):   ahrs_driver          ← IMU
                   lslidar_driver       ← 激光雷达
层级3 (依赖CAN):   wtb_car              ← 底盘驱动 (需要can_bridge)
层级4 (依赖传感器): joint_state_publisher ← TF树
                   robot_state_publisher  ← 从URDF发布静态TF
                   pointcloud_to_laserscan ← 3D→2D
层级5 (依赖里程计+IMU): ekf_node        ← EKF融合
层级6 (依赖融合里程计+scan): 
                   cartographer_node    ← SLAM建图
                   nav2_bringup         ← 自主导航 (二选一)
```

---

# 六、关键技术点总结

|技术点|使用位置|说明|
|---|---|---|
|**阿克曼运动模型**|`wtb_car.cpp:528-591`|`ω = v·tan(δ)/L`，将线速度+转向角转为位姿变化|
|**CAN协议位操作**|`wtb_car.cpp:230-267`|16位字段跨字节存储 (小端)，BCC异或校验|
|**差分里程计**|`ekf_wtb_fdimu.yaml:37`|`odom0_differential: true`，将绝对位姿转为速度量|
|**EKF多传感器融合**|`ekf_wtb_fdimu.yaml`|里程计(vx,vy,wz) + IMU(wz) → 互补融合|
|**运动滤波**|`cartographer.lua:48-49`|0.2m/0.3rad 阈值，过滤微小抖动，减轻计算压力|
|**回环检测**|`cartographer.lua:61-64`|40m范围，min_score=0.6|
|**Ceres 图优化**|`cartographer.lua:71-73`|Huber loss=100, 50次迭代|
|**AMCL 粒子滤波**|`wtb_nav2_params.yaml`|5000粒子, likelihood_field 模型|
|**心跳计数器**|`wtb_car.cpp:259-260`|0-15 循环，底盘检测上位机是否在线|
|**3帧重复发送**|`wtb_car.cpp:357-366`|提高 CAN best_effort 通信可靠性|

这套系统的设计精髓在于**分层解耦**：硬件层(can_bridge + lidar_ros2) → 驱动层(wtb_car_driver) → 融合层(EKF) → 算法层(cartographer/nav2)。每层只依赖下层提供的标准 ROS2 话题接口，使得任意一层都可以独立替换或调试。

---

# 七、Nav2 vs Autoware — 数据流对比分析

## 7.1 两套栈的总体架构

```
┌────────────────────────────────────────────────────────────────────────────┐
│                      Nav2 导航栈 (2层架构)                                   │
│                                                                             │
│   传感器 ──→ EKF融合 ──→ AMCL定位 ──→ 路径规划 ──→ 运动控制 ──→ 底盘        │
│   驱动        (1步)        (1步)       (1步)       (1步)      驱动          │
│                                                                             │
│   特点: 每一步都有单一明确的输入→输出，管道简洁                                │
└────────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────────┐
│                      Autoware 导航栈 (5层架构)                               │
│                                                                             │
│   传感器 ──→ 定位 ──→ 感知 ──→ 规划(3级) ──→ 控制 ──→ 车辆接口 ──→ 底盘     │
│   驱动      (NDT)    (检测+    (任务→行为     (轨迹    (适配器)     驱动     │
│                     跟踪+      →运动)        跟踪)                          │
│                     预测)                                                   │
│                                                                             │
│   特点: 每层有多个子模块，层间有严格的API接口和生命周期管理                      │
└────────────────────────────────────────────────────────────────────────────┘
```

## 7.2 Nav2完整数据流

```
                           ┌──────────┐
                           │  Rviz2    │
                           │ 用户交互  │
                           └─────┬─────┘
                                 │  /goal_pose (PoseStamped)
                                 │
┌────────────────────────────────┼─────────────────────────────────────────────┐
│                                ▼                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      Nav2 Bringup (nav2_bringup)                       │  │
│  │                                                                        │  │
│  │  ┌──────────┐    ┌──────────────────┐    ┌────────────────────────┐   │  │
│  │  │  AMCL     │    │  Planner Server   │    │  Controller Server     │   │  │
│  │  │ 粒子滤波  │    │  (Global Planner) │    │  (DWB Local Planner)   │   │  │
│  │  │          │    │                   │    │                        │   │  │
│  │  │ 输入:    │    │  输入: /goal_pose │    │  输入: /plan (Path)    │   │  │
│  │  │  /scan   │    │       /map        │    │       /scan            │   │  │
│  │  │  /map    │    │       global_cost │    │       local_costmap    │   │  │
│  │  │  /ekf_odom│   │                   │    │       /ekf_odom        │   │  │
│  │  │          │    │  输出: /plan      │    │                        │   │  │
│  │  │ 输出:    │    │       (全局路径)   │    │  输出: /cmd_vel       │   │  │
│  │  │ map→odom │    └──────────────────┘    │       (Twist)          │   │  │
│  │  │   TF     │                            └───────────┬────────────┘   │  │
│  │  └──────────┘                                        │                │  │
│  └──────────────────────────────────────────────────────┼────────────────┘  │
│                                                         │                   │
│  ┌──────────────────────────────────────────────────────┼────────────────┐  │
│  │                 Costmap 2D (代价地图)                 │                │  │
│  │                                                      ▼                │  │
│  │  global_costmap:  50m×50m 静态层(/map) + 障碍物层(/scan)              │  │
│  │  local_costmap:   5m×5m   滚动窗口(实时避障)                           │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│                                     /cmd_vel (Twist)                        │
└────────────────────────────────────────┼────────────────────────────────────┘
                                         │
                                         ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                           融合层                                              │
│                                                                             │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │                   EKF (robot_localization)                              │ │
│  │  融合: /car_odom (vx,vy,wz)  +  /imu (wz)                              │ │
│  │  输出: /ekf_odom  +  TF: odom→base_footprint (30Hz, 2D)                │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                             │
│               /car_odom ↑            ↑ /imu                                  │
└─────────────────────────┼────────────┼──────────────────────────────────────┘
                          │            │
┌─────────────────────────┼────────────┼──────────────────────────────────────┐
│                         │  驱动层      │                                      │
│                         │            │                                       │
│  ┌──────────────────────┴──┐  ┌──────┴─────────────┐  ┌───────────────────┐ │
│  │  wtb_car_driver         │  │  fdilink_ahrs      │  │  lslidar_driver   │ │
│  │  • /car_odom (里程计)   │  │  • /imu (陀螺+加)  │  │  • /point_cloud_  │ │
│  │  • /wtb_car_message     │  │  • /euler_angles   │  │    raw (点云)     │ │
│  │  • CAN协议 ↔ /cmd_vel   │  │  • /gps/fix (GPS)  │  └─────────┬─────────┘ │
│  └─────────────────────────┘  └────────────────────┘            │           │
│                                                                  │           │
│                                                ┌─────────────────▼─────────┐ │
│                                                │ pointcloud_to_laserscan   │ │
│                                                │ 3D→2D, height过滤        │ │
│                                                │ → /scan                   │ │
│                                                └───────────────────────────┘ │
└────────────────────────────────────────────────────────────────────────────┘
```

Nav2数据管道总结（6个步骤）：

```
步骤1: LiDAR → /point_cloud_raw → pointcloud_to_laserscan → /scan
步骤2: /car_odom + /imu → EKF → /ekf_odom + odom→base_footprint TF
步骤3: /scan + /map + /ekf_odom → AMCL → map→odom TF (定位)
步骤4: /scan + /map → costmaps (global + local 代价地图)
步骤5: /goal_pose + costmaps → Planner → /plan (全局路径)
步骤6: /plan + local_costmap + /ekf_odom → DWB Controller → /cmd_vel
─────────────────────────────────────────────────────────────
       /cmd_vel → wtb_car_driver → CAN → 底盘电机执行
```

## 7.3 Autoware完整数据流

```
                           ┌──────────┐
                           │  Rviz2    │
                           │ 用户交互  │
                           └─────┬─────┘
                                 │  /planning/mission_planning/checkpoint
                                 │  /planning/mission_planning/goal
                                 │
┌────────────────────────────────┼─────────────────────────────────────────────┐
│                                ▼                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                    Planning 规划层 (3级层次)                            │  │
│  │                                                                        │  │
│  │  ┌──────────────────────────────────────────────────────────────┐    │  │
│  │  │ ① Mission Planner (任务规划)                                   │    │  │
│  │  │   输入: 用户目标点 (checkpoint/goal)                            │    │  │
│  │  │        /map (高清地图 Lanelet2 格式)                           │    │  │
│  │  │   输出: 车道级路径 (Lanelet 序列)                              │    │  │
│  │  │   做什么: "从当前位置，走哪条车道到达目的地"                     │    │  │
│  │  └──────────────────────────┬───────────────────────────────────┘    │  │
│  │                             ▼                                         │  │
│  │  ┌──────────────────────────────────────────────────────────────┐    │  │
│  │  │ ② Behavior Planner (行为规划)                                  │    │  │
│  │  │   输入: 车道级路径 + 感知结果 (障碍物/红绿灯/停车线)           │    │  │
│  │  │        + 交通规则                                              │    │  │
│  │  │   输出: 行为决策 (LaneFollow / Stop / Avoid / Turn / ...)     │    │  │
│  │  │   做什么: "前方有车, 我该停车还是变道?"                         │    │  │
│  │  └──────────────────────────┬───────────────────────────────────┘    │  │
│  │                             ▼                                         │  │
│  │  ┌──────────────────────────────────────────────────────────────┐    │  │
│  │  │ ③ Motion Planner (运动规划)                                    │    │  │
│  │  │   输入: 行为决策 + 代价地图 + 车辆状态                          │    │  │
│  │  │   输出: /planning/scenario_planning/trajectory (Trajectory)   │    │  │
│  │  │   做什么: "生成一条平滑的、满足运动学约束的 X(t),Y(t),θ(t)"     │    │  │
│  │  └──────────────────────────┬───────────────────────────────────┘    │  │
│  └──────────────────────────────┼──────────────────────────────────────┘  │
│                                 │                                          │
│                                 ▼                                          │
│  ┌──────────────────────────────────────────────────────────────────────┐ │
│  │                     Control 控制层                                     │ │
│  │                                                                       │ │
│  │  ┌────────────────────────────────────────────────────────────┐      │ │
│  │  │ Trajectory Follower (轨迹跟踪器, 通常是MPC/PurePursuit)     │      │ │
│  │  │                                                             │      │ │
│  │  │  输入: /planning/.../trajectory (参考轨迹)                   │      │ │
│  │  │        /localization/.../kinematic_state (当前车辆状态)      │      │ │
│  │  │                                                             │      │ │
│  │  │  输出: /control/command/control_cmd                          │      │ │
│  │  │        (AckermannControlCommand)                             │      │ │
│  │  │        • longitudinal.velocity (目标速度 m/s)               │      │ │
│  │  │        • longitudinal.acceleration (目标加速度 m/s²)        │      │ │
│  │  │        • lateral.steering_tire_angle (目标转向角 rad)       │      │ │
│  │  │                                                             │      │ │
│  │  │  输出: /control/command/gear_cmd                             │      │ │
│  │  │        (GearCommand: DRIVE/REVERSE/PARK/NEUTRAL/LOW)       │      │ │
│  │  └────────────────────────────────────────────────────────────┘      │ │
│  └──────────────────────────────┬───────────────────────────────────────┘ │
│                                 │                                          │
│                /control/command/control_cmd  (AckermannControlCommand)     │
│                /control/command/gear_cmd     (GearCommand)                │
└─────────────────────────────────┼──────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────┼──────────────────────────────────────────┐
│                                 │  车辆接口层                                │
│  ┌──────────────────────────────┴──────────────────────────────────────┐  │
│  │              wvcsc_vehicle_interface (我们自己写的!)                  │  │
│  │                                                                      │  │
│  │  AckermannControlCommand → /cmd_vel (Twist)                         │  │
│  │  GearCommand             → /run_static (start/stop)                  │  │
│  │                                                                      │  │
│  │  /car_odom → VelocityReport + SteeringReport + ControlModeReport    │  │
│  └──────────────────────────────┬──────────────────────────────────────┘  │
│                                 │                                          │
│                         /cmd_vel + /run_static                             │
└─────────────────────────────────┼──────────────────────────────────────────┘
                                  │
                                  ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                         底盘驱动层 (同 Nav2)                                  │
│                                                                             │
│  ┌────────────────────────┐   ┌──────────────┐   ┌──────────────────────┐  │
│  │  wtb_car_driver        │   │  fdilink_ahrs│   │  lslidar_driver      │  │
│  │  /car_odom + CAN控制   │   │  /imu + /gps │   │  /point_cloud_raw    │  │
│  └────────────────────────┘   └──────────────┘   └──────────┬───────────┘  │
│                                                              │              │
│                                              ┌───────────────▼───────────┐  │
│                                              │ pointcloud_to_laserscan   │  │
│                                              │ → /scan                    │  │
│                                              └───────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                     Autoware 感知层 (Nav2 完全没有!)                          │
│                                                                             │
│  /point_cloud_raw                                                            │
│       │                                                                      │
│       ▼                                                                      │
│  ┌─────────────────────┐                                                     │
│  │ Ground Filter       │  地面分割：分离地面点和非地面点                      │
│  │ (ray_ground_filter) │  → /perception/obstacle_segmentation/pointcloud     │
│  └────────┬────────────┘                                                     │
│           ▼                                                                  │
│  ┌─────────────────────┐                                                     │
│  │ Clustering          │  聚类：把非地面点聚成一个个物体                      │
│  │ (euclidean_cluster) │  → /perception/obstacle_segmentation/pointcloud_    │
│  │                     │     clusters                                        │
│  └────────┬────────────┘                                                     │
│           ▼                                                                  │
│  ┌─────────────────────┐                                                     │
│  │ Shape Estimation    │  形状估计：给每个聚类拟合边界框 (BBox)               │
│  │ (shape_estimator)   │  → /perception/object_recognition/detection/objects │
│  └────────┬────────────┘                                                     │
│           ▼                                                                  │
│  ┌─────────────────────┐                                                     │
│  │ Tracking            │  多目标跟踪：给每个物体分配 ID, 估计速度             │
│  │ (multi_object_track)│  → /perception/object_recognition/tracking/objects  │
│  └────────┬────────────┘                                                     │
│           ▼                                                                  │
│  ┌─────────────────────┐                                                     │
│  │ Prediction          │  轨迹预测：预测每个物体的未来运动轨迹                │
│  │ (map_based_prediction)│ → /perception/object_recognition/objects          │
│  └────────┬────────────┘                                                     │
│           │                                                                  │
│           ▼                                                                  │
│  送入 Behavior Planner 做避障决策                                             │
└────────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                      Autoware 定位层 (替代AMCL)                               │
│                                                                             │
│  ┌──────────────────────────────┐   ┌────────────────────────────────────┐ │
│  │  NDT Scan Matcher            │   │  EKF Localizer                     │ │
│  │  (Normal Distributions       │   │                                     │ │
│  │   Transform)                 │   │  输入:                              │ │
│  │                              │   │  • NDT匹配位姿 (观测)               │ │
│  │  输入: /scan + 先验点云地图  │   │  • /imu (角速度)                   │ │
│  │  输出: 当前位姿 (pose)       │   │  • /vehicle/status/velocity_status │ │
│  └──────────────┬───────────────┘   │                                     │ │
│                 │                   │  输出:                               │ │
│                 ▼                   │  /localization/kinematic_state      │ │
│            NDT位姿观测              │  /localization/acceleration         │ │
│                 │                   │  TF: map→base_link                  │ │
│                 ▼                   │                                     │ │
│           ┌──────────┐              └────────────────────────────────────┘ │
│           │   EKF    │                                                      │
│           └──────────┘                                                      │
│                                                                             │
│  对比 AMCL:                                                                  │
│  • AMCL = 粒子滤波, 需要栅格地图, 计算量大, 需要初始位姿                          │
│  • NDT  = 点云匹配, 需要点云地图, 精度高, 不需要初始位姿                          │
└────────────────────────────────────────────────────────────────────────────┘
```

Autoware数据管道总结（行业标准5层分法）：

```
定位层   /scan + 点云地图 → NDT匹配 → EKF融合 → /localization/kinematic_state
感知层   /point_cloud_raw → ground_filter → cluster → track → predict
                            → /perception/objects (障碍物列表+预测轨迹)
规划层   目标点 + 高清地图 + 感知结果
            → Mission Planner (车道级 A→B)
            → Behavior Planner (避障/停车/变道决策)
            → Motion Planner (平滑曲线生成)
            → /planning/trajectory
控制层   /planning/trajectory + /localization/kinematic_state
            → Trajectory Follower (MPC/PurePursuit)
            → /control/command/control_cmd (AckermannControlCommand)
接口层   AckermannControlCommand → /cmd_vel → CAN → 底盘电机
```

## 7.4 逐层对比：相同点和不同点

```
┌──────────────────┬─────────────────────────────┬─────────────────────────────┐
│      对比维度     │         Nav2                │         Autoware            │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【传感器驱动层】   ✅ 完全相同                                                  │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  LiDAR            │ 雷神C16 → pointcloud → /scan│ 雷神C16 → pointcloud → /scan│
│  IMU              │ FDILink → /imu             │ FDILink → /imu              │
│  CAN/底盘          │ can_bridge + wtb_car       │ can_bridge + wtb_car        │
│  里程计            │ /car_odom                  │ /car_odom                   │
│  结论: 传感器驱动层完全共用, 无任何差异                                         │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【融合层】         ✅ 基本相同时                                                  │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  融合方式          │ EKF (robot_localization)   │ EKF (autoware_ekf_localizer)│
│  输入             │ /car_odom + /imu           │ NDT位姿 + /imu + 车速        │
│  输出             │ /ekf_odom                  │ /localization/kinematic_state│
│  TF               │ odom→base_footprint        │ map→base_link               │
│  频率             │ 30Hz                       │ 30-50Hz                     │
│  差异:                                                                        │
│  • Nav2 EKF融合的是轮式里程计(vx,vy,wz) + IMU(wz)                              │
│  • Autoware EKF融合的是 NDT匹配位姿 + IMU(wz) + 车速(线速度)                     │
│  • Nav2 EKF输出的是 odom→base_footprint (还需要AMCL再定位)                       │
│  • Autoware EKF输出的是 map→base_link (已经是全局定位, 不需要再定位)               │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【定位层】         ❌ 完全不同                                                   │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  算法             │ AMCL (粒子滤波)              │ NDT/GICP (扫描匹配)          │
│  输入             │ /scan + 栅格地图(.pgm)      │ /scan + 点云地图(.pcd)       │
│  定位精度         │ 5-15cm                     │ 2-5cm                       │
│  初始位姿          │ 必须手动指定 (2D Pose Estimate)│ 可自动初始化                │
│  重定位            │ 需要粒子重新分布, 慢          │ 可快速重定位                 │
│  计算开销          │ 5000粒子×激光点数, 中等       │ 体素+高斯分布, 较高           │
│  核心差异:                                                                     │
│  AMCL 是"我在栅格地图的哪个格子里?"                                               │
│  NDT  是"我观测到的点云和地图点云的哪个位置最吻合?"                                  │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【感知层】         ❌ Nav2 没有, Autoware 有                                     │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  Nav2             │ 无感知模块                   │                            │
│                   │ 只有 costmap 膨胀层          │                            │
│                   │ (把/scan的每个点当成障碍物)    │                            │
│  Autoware         │ 完整的感知管线:              │                             │
│                   │ 地面过滤→聚类→形状估计        │                             │
│                   │ →跟踪→预测                  │                             │
│  差异影响:         │                            │                             │
│  • Nav2无法区分"行人"和"墙壁"                     │                            │
│  • Autoware知道"那个行人正在向右移动, 预计1秒后到位置X"│                            │
│  • Nav2不能做预测性避障                            │                            │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【规划层】         ❌ 架构完全不同                                              │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  层级             │ 2层 (Global + Local)        │ 3层 (Mission+Behavior+Motion)│
│  Global Planner   │ A* / Dijkstra / Smac       │ Mission Planner              │
│  输入             │ /goal_pose + costmap       │ 目标点 + Lanelet2高清地图     │
│  输出             │ /plan (2D栅格路径)          │ 车道级路径(Lanelet序列)       │
│  Behavior         │ 无                         │ Behavior Planner             │
│                   │ (隐含在BT行为树中)            │ 停车线/红绿灯/避障/变道/转弯   │
│  Motion Planner   │ DWB (Dynamic Window)       │ Motion Planner (Optimization) │
│  输出             │ /cmd_vel (速度+角速度)       │ /planning/trajectory (轨迹)   │
│  消息类型          │ Twist (一次一个命令)         │ Trajectory (一条完整曲线)      │
│  关键区别:                                                                     │
│  Nav2 输出:  "下一步应该以 0.3m/s 直线走"                                         │
│  Autoware输出:"未来3秒的位置序列: (0,0)→(0.3,0)→(0.6,0.01)→..."                  │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【控制层】         ❌ 接口完全不同                                              │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  控制器            │ DWB Controller             │ Trajectory Follower (MPC)    │
│  输出话题          │ /cmd_vel                   │ /control/command/control_cmd│
│  消息类型          │ geometry_msgs/Twist        │ autoware_control_msgs/Control│
│  内容             │ linear.x + angular.z       │ 速度+加速度+转向角+转向速率    │
│  适配器            │ 不需要 (wtb_car直接接收)     │ wvcsc_vehicle_interface     │
│  关键区别:                                                                     │
│  • Nav2 /cmd_vel 只表达"当前这一刻"的速度和角速度                                   │
│  • Autoware Control 表达目标速度+加速度+转向角, 底盘可以做平滑插值                    │
│  • Autoware 还有 /control/command/gear_cmd (档位命令)                            │
├──────────────────┼─────────────────────────────┼─────────────────────────────┤
│  【底盘接口层】     ❌ 完全不同                                                 │
│  ─────────────────┼─────────────────────────────┼─────────────────────────────┤
│  /cmd_vel → 底盘   │ 直接: wtb_car订阅/cmd_vel     │ 间接: vehicle_interface    │
│                   │ SendSpeedToAKM(line, ang)   │ 转换 AckermannCmd → Twist  │
│  档位控制          │ 无 (隐含在速度的正负号)       │ 独立: GearCommand →        │
│                   │                            │   /run_static (stop/start)  │
│  状态反馈          │ /car_odom (里程计)          │ /vehicle/status/           │
│                   │                            │   velocity_status           │
│                   │                            │   steering_status           │
│                   │                            │   control_mode              │
│                   │                            │   gear_status               │
└──────────────────┴─────────────────────────────┴─────────────────────────────┘
```

## 7.5 核心架构差异的可视化对比

```
═╦══════════════════════════════════════════════════════════════════════╦═
 ║                        Nav2 — "扁平的管道"                           ║
═╩══════════════════════════════════════════════════════════════════════╩═

  /scan ──→ costmap ──┐
                       ├──→ Planner ──→ /plan ──→ DWB ──→ /cmd_vel
  /map ───→ costmap ──┘                                    (Twist)
                                                              │
  /ekf_odom ──────────────────────────→ AMCL ──→ map→odom TF  │
  /scan ──────────────────────────────→ AMCL                  │
                                                              ▼
  /cmd_vel ──→ wtb_car_driver ──→ CAN ──→ 底盘

  特点: 每个模块只做一件事, 模块间通过ROS2话题解耦, 但不强制标准化接口


═╦══════════════════════════════════════════════════════════════════════╦═
 ║                   Autoware — "严格分层的管道"                         ║
═╩══════════════════════════════════════════════════════════════════════╩═

  ┌────────── Localization ──────────┐
  │ NDT匹配 ← /scan                   │
  │    ↓                             │
  │ EKF ← NDT位姿 + /imu + 车速      │
  │    ↓                             │
  │ /localization/kinematic_state    │──→ [所有下游模块使用统一接口]
  └──────────────────────────────────┘

  ┌────────── Perception ────────────┐
  │ /point_cloud_raw                 │
  │    ↓ ground_filter              │
  │    ↓ clustering                 │
  │    ↓ shape_estimation           │
  │    ↓ tracking                   │
  │    ↓ prediction                 │
  │ /perception/objects             │──→ [交规 + 行为决策]
  └──────────────────────────────────┘

  ┌────────── Planning ──────────────┐
  │ Mission Planner (A→B 走哪条路)    │
  │    ↓                             │
  │ Behavior Planner (停车/变道/跟车) │
  │    ↓                             │
  │ Motion Planner (生成平滑轨迹)      │
  │    ↓                             │
  │ /planning/trajectory             │──→ [带有时间戳的完整轨迹]
  └──────────────────────────────────┘

  ┌────────── Control ───────────────┐
  │ Trajectory Follower (MPC/PurePur)│
  │    ↓                             │
  │ /control/command/control_cmd     │──→ [AckermannControlCommand]
  │ /control/command/gear_cmd        │
  └──────────────────────────────────┘

  ┌────────── Vehicle I/F ───────────┐
  │ wvcsc_vehicle_interface          │
  │ AckermannCmd → /cmd_vel          │
  │ GearCmd → /run_static            │
  └──────────────────────────────────┘
           │
           ▼
  wtb_car_driver → CAN → 底盘

  特点: 每层有标准化的输入/输出API, 层内模块可插拔替换, 架构支持多车/多传感器扩展
```

## 7.6 消息类型对比

**控制指令对比**：

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  Nav2 /cmd_vel                  Autoware /control/command/control_cmd    │
│  ─────────────                  ─────────────────────────────────────    │
│  geometry_msgs/Twist            autoware_control_msgs/Control            │
│                                                                          │
│  linear:                        longitudinal:                            │
│    x: 0.5    ← 前进0.5m/s         velocity: 0.5    ← 前进0.5m/s          │
│    y: 0.0                          acceleration: 0.3 ← 以0.3m/s²加速     │
│    z: 0.0                                                               │
│                                  lateral:                                │
│  angular:                          steering_tire_angle: 0.15 ← 转8.6°    │
│    x: 0.0                          steering_tire_rotation_rate: 0.1      │
│    y: 0.0                                                               │
│    z: 0.15  ← 转8.6° (当作转向角)                                        │
│                                                                          │
│  问题: angular.z 语义不明确        优势: 语义清晰, 专业字段                  │
│  (到底是转向角还是角速度?)           (有加速度/转向速率, 底盘可做平滑)        │
└─────────────────────────────────────────────────────────────────────────┘
```

**定位输出对比**：

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  Nav2                           Autoware                                 │
│  ─────                          ────────                                 │
│  /ekf_odom (nav_msgs/Odometry)  /localization/kinematic_state           │
│                                  (nav_msgs/Odometry)                     │
│                                                                          │
│  TF: odom→base_footprint        TF: map→base_link                       │
│       ↑ 局部里程计坐标系               ↑ 全局地图坐标系                    │
│                                                                          │
│  还需要 AMCL 再做一步            已经是最终定位结果                         │
│  map→odom→base_footprint        map→base_link (直接)                    │
└─────────────────────────────────────────────────────────────────────────┘
```

**路径/轨迹对比**：

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  Nav2 /plan                     Autoware /planning/.../trajectory        │
│  ────────                       ─────────────────────────────           │
│  nav_msgs/Path                  autoware_planning_msgs/Trajectory        │
│                                                                          │
│  包含:                          包含:                                     │
│  • 2D坐标序列 (x,y)              • 带时间戳的位姿序列                      │
│  • 无时间信息                    • 每个点的速度/加速度                     │
│  • 无速度信息                    • 曲率信息                              │
│                                                                          │
│  [(0,0), (0.5,0), (1,0.1)]     [(t=0,x=0,v=0), (t=1,x=0.5,v=0.5)...]  │
│                                                                          │
│  作用: "沿着这些点走"            作用: "按这个时间-速度-位姿表精确走"         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 7.7 核心差异根源 — 设计哲学

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│     Nav2 的设计哲学                 Autoware 的设计哲学                     │
│     ──────────────                  ──────────────────                    │
│                                                                          │
│  "做一件事并且做好"                "做一个完整的自动驾驶系统"                │
│                                                                          │
│  目标用户:                        目标用户:                                │
│   室内移动机器人                    L4级自动驾驶汽车                         │
│   仓储AGV/AMR                      园区接驳车/无人配送                       │
│   服务机器人                                                              │
│                                                                          │
│  不需要:                          需要:                                    │
│  ❌ 交通规则遵守                    ✅ 红绿灯/停车线/限速                   │
│  ❌ 动态障碍物分类                  ✅ 行人/车辆/自行车分类+跟踪              │
│  ❌ 障碍物轨迹预测                  ✅ 对未来N秒的预测                       │
│  ❌ 车道级导航                      ✅ Lanelet2高清地图                     │
│  ❌ 速度曲线规划                    ✅ 带时间/速度约束的轨迹优化               │
│                                                                          │
│  这解释了为什么:                                                           │
│  Nav2 = 传感器→融合→定位→规划→控制           (5个模块)                      │
│  AW   = 传感器→感知→定位→规划(3级)→控制→接口  (10+个模块)                    │
│                                                                          │
│  也解释了为什么 Nav2可以直接用/cmd_vel控制:                                   │
│    室内机器人只需"前进0.5m/s"就够了                                        │
│                                                                          │
│  而 Autoware需要完整轨迹+加速度+转向角:                                     │
│    自动驾驶汽车需要在路口先减速(加速度)、再转弯(转向角)、再加速               │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

## 7.8 总结表

```
┌─────────────┬────────────────────┬────────────────────┬──────────────┐
│   功能层     │      Nav2          │     Autoware        │   是否可共用  │
├─────────────┼────────────────────┼────────────────────┼──────────────┤
│ 传感器驱动   │ LiDAR+IMU+底盘     │ LiDAR+IMU+底盘      │  ✅ 完全共用  │
│ CAN通信      │ can_bridge         │ can_bridge          │  ✅ 完全共用  │
│ 底盘驱动     │ wtb_car_driver     │ wtb_car_driver      │  ✅ 完全共用  │
│ 3D→2D转换   │ pointcloud_to_laser │ pointcloud_to_laser │  ✅ 完全共用  │
│ TF树        │ URDF→robot_state_pub│ URDF→robot_state_pub│  ✅ 完全共用  │
├─────────────┼────────────────────┼────────────────────┼──────────────┤
│ 融合层       │ EKF (odom+IMU)     │ EKF (NDT+IMU+速度)  │  ⚠️ 输入不同  │
│ 定位层       │ AMCL (粒子滤波)    │ NDT (扫描匹配)      │  ❌ 完全不同  │
│ 感知层       │ (无) costmap膨胀   │ 检测+跟踪+预测      │  ❌ AW独有    │
│ 规划-全局    │ Planner (A*/Dijk)  │ Mission Planner     │  ❌ 完全不同  │
│ 规划-行为    │ BT行为树(简单)     │ Behavior Planner    │  ❌ AW更复杂  │
│ 规划-运动    │ DWB (局部规划)     │ Motion Planner      │  ❌ 完全不同  │
│ 控制         │ (内嵌于DWB)        │ Trajectory Follower │  ❌ 完全不同  │
│ 底盘接口     │ 直接 /cmd_vel      │ vehicle_interface   │  ⚠️ 需要适配  │
│ 档位控制     │ 无                 │ /control/gear_cmd   │  ❌ AW独有    │
│ 车辆状态反馈 │ /car_odom          │ 5个状态话题         │  ⚠️ 需要适配  │
│ 地图格式     │ .pgm + .yaml       │ .pcd + Lanelet2     │  ❌ 完全不同  │
└─────────────┴────────────────────┴────────────────────┴──────────────┘
```

**核心结论**：两套系统在硬件驱动层完全共用（can_bridge、wtb_car_driver、ahrs、lslidar），但在定位层以上完全不同。`wvcsc_vehicle_interface` 是连接 Autoware 标准接口和你已有底盘的唯一胶水代码。你可以在同一台车上切换使用 Nav2（室内/简单环境）或 Autoware（室外/复杂道路），它们共享硬件但不共享算法。