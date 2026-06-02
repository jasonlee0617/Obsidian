- [[#1.安装 Docker 官方源|1.安装 Docker 官方源]]
	- [[#1.安装 Docker 官方源#1.清理可能冲突的包|1.清理可能冲突的包]]
	- [[#1.安装 Docker 官方源#2.安装基础依赖|2.安装基础依赖]]
	- [[#1.安装 Docker 官方源#3.创建 keyrings 目录|3.创建 keyrings 目录]]
	- [[#1.安装 Docker 官方源#4.使用清华源下载 Docker GPG key|4.使用清华源下载 Docker GPG key]]
	- [[#1.安装 Docker 官方源#5.更新 APT|5.更新 APT]]
	- [[#1.安装 Docker 官方源#6.检查 Docker 包是否可用|6.检查 Docker 包是否可用]]
	- [[#1.安装 Docker 官方源#7.安装 Docker|7.安装 Docker]]
	- [[#1.安装 Docker 官方源#8.启动 Docker 服务|8.启动 Docker 服务]]
	- [[#1.安装 Docker 官方源#9.让普通用户不用 sudo 运行 Docker|9.让普通用户不用 sudo 运行 Docker]]
	- [[#1.安装 Docker 官方源#10.解决 Docker Hub 拉取超时|10.解决 Docker Hub 拉取超时]]
	- [[#1.安装 Docker 官方源#11.检查能不能访问 GHCR|11.检查能不能访问 GHCR]]
	- [[#1.安装 Docker 官方源#12.检查系统和硬件|12.检查系统和硬件]]
	- [[#1.安装 Docker 官方源#13.检查 NVIDIA GPU|13.检查 NVIDIA GPU]]
- [[#2.准备 Autoware 数据目录|2.准备 Autoware 数据目录]]
- [[#3.选择我们先用哪个 Autoware Docker 镜像|3.选择我们先用哪个 Autoware Docker 镜像]]
- [[#4:不行则配置 Docker daemon 代理(成功则跳过)|4:不行则配置 Docker daemon 代理(成功则跳过)]]
- [[#5.启动 Autoware 容器|5.启动 Autoware 容器]]
	- [[#5.启动 Autoware 容器#5.1开放 X11 图形权限|5.1开放 X11 图形权限]]
	- [[#5.启动 Autoware 容器#5.2启动 Autoware Core 容器|5.2启动 Autoware Core 容器]]
	- [[#5.启动 Autoware 容器#5.3做一个固定启动脚本|5.3做一个固定启动脚本]]
	- [[#5.启动 Autoware 容器#5.4重新进入 core 容器并探索|5.4重新进入 core 容器并探索]]
	- [[#5.启动 Autoware 容器#5.5检查容器里的 ROS 和 Autoware|5.5检查容器里的 ROS 和 Autoware]]
	- [[#5.启动 Autoware 容器#5.6为什么 core-humble 不是最终 Demo 镜像|5.6为什么 core-humble 不是最终 Demo 镜像]]
	- [[#5.启动 Autoware 容器#5.7先检查 core 容器能否直接运行 planning launch|5.7先检查 core 容器能否直接运行 planning launch]]
- [[#6. 拉取 universe-humble 镜像|6. 拉取 universe-humble 镜像]]
- [[#7.准备 Planning Simulator Demo 的地图|7.准备 Planning Simulator Demo 的地图]]
	- [[#7.准备 Planning Simulator Demo 的地图#7.1 克隆 Autoware 主仓库，用它的 Ansible 脚本下载 Demo 数据|7.1 克隆 Autoware 主仓库，用它的 Ansible 脚本下载 Demo 数据]]
	- [[#7.准备 Planning Simulator Demo 的地图#7.2下载 Planning Simulator 需要的 sample map|7.2下载 Planning Simulator 需要的 sample map]]
	- [[#7.准备 Planning Simulator Demo 的地图#7.3容器里启动 Planning Simulator|7.3容器里启动 Planning Simulator]]
	- [[#7.准备 Planning Simulator Demo 的地图#7.4简单运行自动驾驶|7.4简单运行自动驾驶]]
		- [[#7.4简单运行自动驾驶#第一步：用 `2D Pose Estimate` 设置初始位姿|第一步：用 `2D Pose Estimate` 设置初始位姿]]
		- [[#7.4简单运行自动驾驶#第二步：用 `2D Goal Pose` 设置终点位姿|第二步：用 `2D Goal Pose` 设置终点位姿]]
		- [[#7.4简单运行自动驾驶#第三步：点击rviz左侧界面的“Auto”按钮执行自动驾驶|第三步：点击rviz左侧界面的“Auto”按钮执行自动驾驶]]
		- [[#7.4简单运行自动驾驶#第四步：第二个容器终端里依次执行这些命令|第四步：第二个容器终端里依次执行这些命令]]
- [[#1.Autoware Planning 分层|1.Autoware Planning 分层]]
	- [[#1.Autoware Planning 分层#1.1Mission Planning：生成 route|1.1Mission Planning：生成 route]]
	- [[#1.Autoware Planning 分层#1.2Behavior Planning：生成行为级 path 和可行驶区域|1.2Behavior Planning：生成行为级 path 和可行驶区域]]
	- [[#1.Autoware Planning 分层#1.3Motion Planning：生成更平滑、更可执行的轨迹|1.3Motion Planning：生成更平滑、更可执行的轨迹]]
	- [[#1.Autoware Planning 分层#1.4Control 的系统级理解|1.4Control 的系统级理解]]
	- [[#1.Autoware Planning 分层#1.5Trajectory Follower：跟踪轨迹|1.5Trajectory Follower：跟踪轨迹]]
	- [[#1.Autoware Planning 分层#1.6Vehicle Cmd Gate：最终安全门|1.6Vehicle Cmd Gate：最终安全门]]
	- [[#1.Autoware Planning 分层#1.6Auto 按钮到底做了什么|1.6Auto 按钮到底做了什么]]
- [[#2.真实系统数据流|2.真实系统数据流]]
	- [[#2.真实系统数据流#2.1总图：真实系统数据流|2.1总图：真实系统数据流]]
	- [[#2.真实系统数据流#2.2第一层：Mission Planner|2.2第一层：Mission Planner]]
	- [[#2.真实系统数据流#2.3Behavior Path Planner|2.3Behavior Path Planner]]
	- [[#2.真实系统数据流#2.4第三层：Behavior Velocity Planner|2.4第三层：Behavior Velocity Planner]]
	- [[#2.真实系统数据流#2.5第四层：Motion Velocity Planner|2.5第四层：Motion Velocity Planner]]
	- [[#2.真实系统数据流#2.6# 第五层：Velocity Smoother|2.6# 第五层：Velocity Smoother]]
	- [[#2.真实系统数据流#2.7第六层：Trajectory Follower|2.7第六层：Trajectory Follower]]
	- [[#2.真实系统数据流#2.8第七层：Vehicle Cmd Gate|2.8第七层：Vehicle Cmd Gate]]
	- [[#2.真实系统数据流#2.9整理成模块表|2.9整理成模块表]]
	- [[#2.真实系统数据流#2.10必须掌握的调试口诀|2.10必须掌握的调试口诀]]
- [[#3.理解 Autoware 如何把“路线”加工成“可控制的轨迹”|3.理解 Autoware 如何把“路线”加工成“可控制的轨迹”]]

# 关键词

```
传感器输入
高精定位
感知
预测
行为规划
运动规划
车辆控制
车辆接口
仿真与评估
```

# 能力

```
多传感器融合、激光/相机/雷达感知、交通灯识别、动态目标跟踪、NDT 与 GNSS/IMU 融合定位、路口和变道行为规划、动态避障、轨迹跟踪、标准化车辆接口、数字孪生仿真和 rosbag 回放
```

# 阶段 1：理解自动驾驶系统框架

Autoware 典型链路是：
```
Sensors
  ↓
Sensing / Preprocessing
  ↓
Localization
  ↓
Perception
  ↓
Prediction
  ↓
Planning
  ↓
Control
  ↓
Vehicle Interface
  ↓
Vehicle / Simulator

```

展开后是：
```
LiDAR / Camera / Radar / GNSS / IMU / CAN
    ↓
点云滤波、图像处理、时间同步、坐标转换
    ↓
NDT 定位 / GNSS-IMU 融合 / 地图匹配
    ↓
目标检测 / 目标跟踪 / 交通灯识别 / 车道理解
    ↓
预测动态障碍物未来轨迹
    ↓
行为规划：跟车、变道、避障、路口、停车
    ↓
运动规划：生成带速度的轨迹
    ↓
控制：转向、油门、刹车
    ↓
车辆接口：线控底盘 / 仿真车辆

```

# 阶段2：安装和运行 Autoware Demo

## 1.安装 Docker 官方源

### 1.清理可能冲突的包

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
  sudo apt-get remove -y $pkg
done

```
如果提示某些包没有安装，这是正常的

### 2.安装基础依赖

```
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
```

### 3.创建 keyrings 目录

```
sudo install -m 0755 -d /etc/apt/keyrings

```

### 4.使用清华源下载 Docker GPG key

```
curl -fsSL https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

然后设置权限：
```
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

检查文件是否存在：
```
ls -l /etc/apt/keyrings/docker.gpg
```

应该能看到类似：
```
-rw-r--r-- 1 root root ... /etc/apt/keyrings/docker.gpg
```

添加清华 Docker CE 软件源
```
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu $(. /etc/os-release && echo ${UBUNTU_CODENAME:-$VERSION_CODENAME}) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

检查源文件：
```
cat /etc/apt/sources.list.d/docker.list
```

系统是 Ubuntu 22.04，所以应该看到类似：
```
deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu jammy stable
```

### 5.更新 APT

```
sudo apt update
```

这一步不能再出现：
```
	NO_PUBKEY
```

也不能再出现：
```
仓库没有数字签名
```

### 6.检查 Docker 包是否可用

```
apt-cache policy docker-ce
apt-cache policy docker-ce-cli
apt-cache policy containerd.io
```

### 7.安装 Docker

```
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 8.启动 Docker 服务

安装完成后执行：
```
sudo systemctl enable --now docker
```

检查 Docker 服务：
```
systemctl status docker --no-pager
```

希望看到：
```
active (running)
```

再检查版本：
```
sudo docker version
```

如果能看到 Client 和 Server 两部分，说明 Docker Engine 已经正常运行。

### 9.让普通用户不用 sudo 运行 Docker

现在你还在用：
```
sudo docker ...
```

建议把当前用户加入 docker 组：
```
sudo usermod -aG docker $USER
```

然后执行：
```
newgrp docker
```

测试：
```
docker version
```
如果不用 `sudo` 也能输出 Client 和 Server，说明权限配置好了。

### 10.解决 Docker Hub 拉取超时

执行：
```
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json > /dev/null <<'EOF'
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://docker.1ms.run"
  ]
}
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker
```

检查是否生效：
```
docker info | grep -A 10 "Registry Mirrors"
```

然后再测试：
```
docker run --rm hello-world
```

### 11.检查能不能访问 GHCR

Autoware 镜像在 GitHub Container Registry，所以先测试网络：
```
curl -I https://ghcr.io/v2/
```

正常情况下你可能看到类似：
```
HTTP/2 401
```

再测试 DNS：
```
ping -c 4 ghcr.io
```

### 12.检查系统和硬件

执行：
```
lsb_release -a
uname -m
free -h
df -h ~
```

你希望看到：
```
Ubuntu 22.04 / jammy
x86_64 或 amd64
内存最好 16GB 以上，32GB 更舒服
磁盘最好剩余 100GB 以上
```

### 13.检查 NVIDIA GPU

执行：
```
nvidia-smi
```

如果能看到类似：
```
NVIDIA-SMI ...
Driver Version ...
CUDA Version ...
```
说明主机 NVIDIA 驱动正常

接着安装 NVIDIA Container Toolkit：
```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
  | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg

distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list \
  | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
  | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt update
sudo apt install -y nvidia-container-toolkit

sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

## 2.准备 Autoware 数据目录

Autoware demo 会用到地图、模型、数据等。先创建目录：
```
mkdir -p ~/autoware_data
mkdir -p ~/autoware_data/maps
mkdir -p ~/autoware_data/rosbags
mkdir -p ~/autoware_data/models
```

检查：
```
tree -L 2 ~/autoware_data
```

如果没有 `tree`：
```
sudo apt install -y tree
```

## 3.选择我们先用哪个 Autoware Docker 镜像

初学者，建议第一阶段先用 **Autoware Core runtime 镜像**，原因是：
```
体积相对小
更适合先跑通基本流程
先理解 launch、RViz、topic、TF
不用马上陷入完整 Universe 的复杂依赖
```

官方 Core Docker 文档给出的镜像类型包括：
```
ghcr.io/autowarefoundation/autoware:core-jazzy
ghcr.io/autowarefoundation/autoware:core-devel-jazzy
```

并说明如果使用 ROS 2 Humble，可以把 `jazzy` 替换成 `humble`
所以我们先尝试：
```
docker pull ghcr.io/autowarefoundation/autoware:core-humble
```

若出现以下情况，是因为网络不稳定的原因
![[Pasted image 20260512140139.png]]

继续：
```
docker pull ghcr.io/autowarefoundation/autoware:core-humble
```

确认镜像存在：
```
docker images | grep autoware
```

应该看到类似：
```
ghcr.io/autowarefoundation/autoware   core-humble   ...
```

## 4:不行则配置 Docker daemon 代理(成功则跳过)
创建目录：
```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

写入代理配置：
```
sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf > /dev/null <<'EOF'
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1,::1"
EOF
```

重启 Docker：
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

检查是否生效:
```
systemctl show --property=Environment docker --no-pager
```

正常应看到：
```
HTTP_PROXY=http://127.0.0.1:7890
HTTPS_PROXY=http://127.0.0.1:7890
```

降低 Docker 并发下载数
Autoware 镜像比较大，建议降低并发：
```
sudo tee /etc/docker/daemon.json > /dev/null <<'EOF'
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://docker.1ms.run"
  ],
  "max-concurrent-downloads": 1,
  "max-concurrent-uploads": 1
}
EOF

```

重启：
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

检查代理访问 GHCR
```
curl -x http://127.0.0.1:7890 -I https://ghcr.io/v2/ --max-time 20
```

可接受结果：
```
HTTP/1.1 200 Connection established
HTTP/2 401

```

或者：
```
HTTP/1.1 200 Connection established
HTTP/2 405
```

这说明：
```
代理能访问 GHCR
```

如果出现：
```
SSL_connect reset
connection reset by peer
timeout
```

说明代理节点或代理模式不稳定，需要：
```
切换 Global 模式
更换代理节点
确认 7890 是 HTTP 代理端口
确认 ghcr.io、githubusercontent.com 走代理
```

## 5.启动 Autoware 容器

### 5.1开放 X11 图形权限

在宿主机执行：
```
echo $DISPLAY
```

通常会输出：
```
:0
```

然后执行：
```
xhost +local:docker
xhost +local:root
```

如果没有 `xhost`：
```
sudo apt install -y x11-xserver-utils
```

### 5.2启动 Autoware Core 容器

先用 CPU / 基础图形方式进入容器：
```
docker run -it --rm \
  --privileged \
  --net=host \
  --ipc=host \
  -e DISPLAY=$DISPLAY \
  -e QT_X11_NO_MITSHM=1 \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -v $HOME/autoware_data:/home/autoware/autoware_data \
  ghcr.io/autowarefoundation/autoware:core-humble \
  bash
```

这条命令的作用是：
```
启动 Autoware Core Humble 容器
进入容器的 bash 终端
让容器共享宿主机网络
让容器能访问宿主机图形界面
把宿主机 ~/autoware_data 挂载到容器 /home/autoware/autoware_data
```

进入后先执行：
```
whoami
pwd
ls
printenv | grep ROS
ros2 --version
```

重点看：
```
printenv | grep ROS
```

应该能看到类似：
```
ROS_DISTRO=humble
```

测试 ROS 2 是否可用:
```
ros2 topic list
```

测试 RViz 图形界面:
```
rviz2
```

### 5.3做一个固定启动脚本

每次启动容器都手敲这么长一串：
```
docker run -it --rm \
  --privileged \
  --net=host \
  --ipc=host \
  -e DISPLAY=$DISPLAY \
  -e QT_X11_NO_MITSHM=1 \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -v $HOME/autoware_data:/home/autoware/autoware_data \
  ghcr.io/autowarefoundation/autoware:core-humble \
  bash
```

不方便，也容易输错,我们现在创建一个脚本
先退出当前容器：
```
exit
```

然后创建脚本目录：
```
mkdir -p ~/autoware_scripts
```

创建脚本：
```
gedit ~/autoware_scripts/run_autoware_core.sh
```

写入下面内容：
```
#!/bin/bash

set -e

mkdir -p $HOME/autoware_data/maps
mkdir -p $HOME/autoware_data/rosbags
mkdir -p $HOME/autoware_data/models
mkdir -p $HOME/autoware_data/ml_models

xhost +local:docker
xhost +local:root

docker run -it --rm \
  --privileged \
  --net=host \
  --ipc=host \
  -e DISPLAY=$DISPLAY \
  -e QT_X11_NO_MITSHM=1 \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -v $HOME/autoware_data:/home/aw/autoware_data \
  ghcr.io/autowarefoundation/autoware:core-humble \
  bash

```

保存后给执行权限：
```
chmod +x ~/autoware_scripts/run_autoware_core.sh

```

以后启动 core 容器只需要：
```
~/autoware_scripts/run_autoware_core.sh
```

### 5.4重新进入 core 容器并探索

执行：
```
~/autoware_scripts/run_autoware_core.sh
```

进入容器后，先执行：
```
whoami
pwd
echo $HOME
```

应该看到类似：
```
aw
/home/aw
/home/aw
```

然后检查挂载目录：
```
maps
models
rosbags
ml_models
```

这说明宿主机的：
```
~/autoware_data
```

已经挂载到了容器里的：
```
/home/aw/autoware_data
```

### 5.5检查容器里的 ROS 和 Autoware

在容器里执行：
```
printenv | grep ROS
```

应该看到类似：
```
ROS_VERSION=2
ROS_DISTRO=humble
```

然后执行：
```
ros2 pkg list | grep autoware | head -50
```

再执行：
```
ros2 pkg list | grep tier4 | head -50
```

接着检查 Autoware 关键包：
```
ros2 pkg prefix autoware_launch
```

如果输出类似：
```
/opt/autoware
```

或者某个 Autoware 安装路径，说明容器中能找到 Autoware launch 包。
如果提示找不到：
```
Package not found
```

也不要慌，这说明 `core-humble` 镜像可能不是完整 demo 镜像，下一步我们就切到 `universe-humble`

### 5.6为什么 core-humble 不是最终 Demo 镜像

现在用的是：
```
ghcr.io/autowarefoundation/autoware:core-humble
```

它适合：
```
验证 Docker
验证 ROS 2
验证 RViz
理解 Autoware Core 环境
```

但官方 Planning Simulator 示例使用的是：
```
universe-jazzy
```

所以我们下一步真正跑 Demo 时，建议使用：
```
ghcr.io/autowarefoundation/autoware:universe-humble
```

原因是：
```
Planning Simulator 需要 autoware_launch
需要 sample_vehicle
需要 sample_sensor_kit
需要 planning simulator 相关 launch 和参数
这些通常在 universe 镜像里更完整
```

### 5.7先检查 core 容器能否直接运行 planning launch

在 core 容器里执行：
```
ros2 pkg prefix autoware_launch
```

如果能找到，再执行：
```
ros2 launch autoware_launch planning_simulator.launch.xml --show-args
```

如果报：
```
Package 'autoware_launch' not found
```

说明 core 镜像不适合跑这个 demo，我们就直接使用 universe 镜像

## 6. 拉取 universe-humble 镜像

先退出 core 容器：
```
exit
```

回到宿主机后执行：
```
docker pull ghcr.io/autowarefoundation/autoware:universe-humble
```

进入docker-universe-humble的普通命令：
```
docker run -it --rm ghcr.io/autowarefoundation/autoware:universe-humble
```

退出docker命令：
```
exit
```

创建 universe 容器启动脚本:
```
gedit ~/autoware_scripts/run_autoware_universe.sh
```

写入：
```
#!/bin/bash

set -e

mkdir -p $HOME/autoware_data/maps
mkdir -p $HOME/autoware_data/rosbags
mkdir -p $HOME/autoware_data/models
mkdir -p $HOME/autoware_data/ml_models

xhost +local:docker
xhost +local:root

docker run -it --rm \
  --privileged \
  --net=host \
  --ipc=host \
  -e DISPLAY=$DISPLAY \
  -e QT_X11_NO_MITSHM=1 \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -v $HOME/autoware_data:/home/aw/autoware_data \
  ghcr.io/autowarefoundation/autoware:universe-humble \
  bash
```

给权限：
```
chmod +x ~/autoware_scripts/run_autoware_universe.sh
```

启动：
```
~/autoware_scripts/run_autoware_universe.sh
```

进入容器后检查：
```
ros2 pkg prefix autoware_launch
ros2 launch autoware_launch planning_simulator.launch.xml --show-args
```
如果能显示 launch 参数，说明下一步可以准备地图运行 Demo

## 7.准备 Planning Simulator Demo 的地图


###  7.1 克隆 Autoware 主仓库，用它的 Ansible 脚本下载 Demo 数据

这一步骤在自己电脑上操作，不要在容器内进行操作
```
cd ~
git clone https://github.com/autowarefoundation/autoware.git
```

然后进入仓库：
```
cd ~/autoware
```

安装 Ansible：
```
sudo apt update
sudo apt install -y ansible
```

安装 `pipx`：
```
sudo apt-get update
sudo apt-get install -y pipx python3-venv
```

让 `pipx` 路径生效：
```
python3 -m pipx ensurepath
```

当前终端立即生效：
```
export PATH="$HOME/.local/bin:$PATH"
```

安装 Autoware 官方推荐的 Ansible 版本：
```
pipx install --include-deps --force "ansible==10.*"
```

安装完成后检查：
```
which ansible
which ansible-galaxy

ansible --version
ansible-galaxy --version
```

然后执行：
```
ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
```

### 7.2下载 Planning Simulator 需要的 sample map

执行：
```
ansible-playbook autoware.dev_env.install_dev_env \
  --tags demo_artifacts \
  --ask-become-pass
```

这一步会使用 `demo_artifacts` Ansible role，把 sample map 下载并解压到：
```
~/autoware_data/maps/sample-map-planning/
```

### 7.3容器里启动 Planning Simulator

进入容器：
```
~/autoware_scripts/run_autoware_universe.sh
```

在容器里执行：
```
ros2 launch autoware_launch planning_simulator.launch.xml \
  map_path:=/home/aw/autoware_data/maps/sample-map-planning \
  vehicle_model:=sample_vehicle \
  sensor_model:=sample_sensor_kit
```

这个命令的含义是：
```
map_path:=/home/aw/autoware_data/maps/sample-map-planning
    使用官方示例地图

vehicle_model:=sample_vehicle
    使用官方示例车辆模型

sensor_model:=sample_sensor_kit
    使用官方示例传感器套件

```

### 7.4简单运行自动驾驶
#### 第一步：用 `2D Pose Estimate` 设置初始位姿
点击顶部工具栏：
```
2D Pose Estimate
```
然后在地图上的**车道线上**点击并拖动方向

重点要求：
```
车必须放在车道上
车头方向必须顺着车道箭头方向
不要放到路边、停车位外、建筑物上、灰色区域、非车道区域
```

进入rviz显示界面
![[Pasted image 20260515140819.png]]


#### 第二步：用 `2D Goal Pose` 设置终点位姿

点击：
```
2D Goal Pose
```

重点要求：
```
目标点也必须在车道上
目标方向也要顺着车道方向
不要选太远
不要跨到不连通车道
不要放在停车位、路外、建筑物区域
```
同样要在**同一条或连通车道上**点击并拖动方向

![[Pasted image 20260515141241.png]]

#### 第三步：点击rviz左侧界面的“Auto”按钮执行自动驾驶

![[Pasted image 20260515141425.png]]

数据链路：
```
RViz 2D Goal Pose
  ↓
/planning/mission_planning/goal
  ↓
mission_planner
  ↓
/planning/mission_planning/route
  ↓
behavior_path_planner
  ↓
behavior_velocity_planner
  ↓
path_optimizer
  ↓
motion_velocity_planner
  ↓
velocity_smoother
  ↓
/planning/scenario_planning/trajectory
  ↓
trajectory_follower
  ↓
/control/trajectory_follower/control_cmd
  ↓
vehicle_cmd_gate
  ↓
/control/command/control_cmd
  ↓
simple_planning_simulator
  ↓
/localization/kinematic_state
```

#### 第四步：第二个容器终端里依次执行这些命令

1.节点概览
```
ros2 node list | grep -E "planning|control|vehicle|simulator|operation|routing"
```

2.路线和轨迹话题
```
ros2 topic list | grep -E "route|trajectory|goal"
```

3.查看 route
```
ros2 topic echo /planning/mission_planning/route --once
```

4.查看 trajectory
```
ros2 topic echo /planning/scenario_planning/trajectory --once
```

5.查看控制命令
```
ros2 topic echo /control/command/control_cmd --once
```

6.查看车辆状态
```
ros2 topic echo /localization/kinematic_state --once
```

7.查看 operation mode 服务
```
ros2 service list | grep operation_mode
```

# 阶段三 系统架构讲解

## 1.Autoware Planning 分层

```
Mission Planning
    ↓
Behavior Planning
    ↓
Motion Planning
    ↓
Velocity Smoothing / Validation

```

### 1.1Mission Planning：生成 route

典型节点：
```
/planning/mission_planning/mission_planner
/planning/mission_planning/route_selector
```

主要输入：
```
车辆当前位置
目标点
Lanelet2 地图
```

主要输出：
```
/planning/mission_planning/route
/planning/route
```

### 1.2Behavior Planning：生成行为级 path 和可行驶区域

典型节点：
```
/planning/scenario_planning/lane_driving/behavior_planning/behavior_path_planner
/planning/scenario_planning/lane_driving/behavior_planning/behavior_velocity_planner
```

它解决的问题是
```
沿 route 具体应该走车道中间吗？
是否需要换道？
是否需要避开静态障碍？
哪些区域是可行驶区域？
是否要提前打转向灯？
是否要在停止线、路口、交通灯前减速或停车？
```

### 1.3Motion Planning：生成更平滑、更可执行的轨迹

典型节点：
```
/planning/scenario_planning/lane_driving/motion_planning/path_optimizer
/planning/scenario_planning/lane_driving/motion_planning/motion_velocity_planner
```

它解决的问题是：
```
行为层给出大致 path 后，车辆实际该怎么平滑地走？
速度如何变化？
遇到障碍物是否要 stop / slow down / cruise？
轨迹是否符合车辆运动学约束？
```

### 1.4Control 的系统级理解

Control 主要分两级：
```
Trajectory Follower
    ↓
Vehicle Cmd Gate
```

所以控制链路是：
```
/planning/scenario_planning/trajectory
        ↓
/control/trajectory_follower/controller_node_exe
        ↓
/control/trajectory_follower/control_cmd
        ↓
/control/vehicle_cmd_gate
        ↓
/control/command/control_cmd
        ↓
/simulation/simple_planning_simulator

```
### 1.5Trajectory Follower：跟踪轨迹

看到的节点：
```
/control/trajectory_follower/controller_node_exe
```

它主要输入：
```
/planning/scenario_planning/trajectory
/localization/kinematic_state
/vehicle/status/steering_status
/vehicle/status/velocity_status
```

它主要输出：
```
/control/trajectory_follower/control_cmd
```

它解决的问题是：
```
当前车辆位置和轨迹之间有偏差
应该给多少速度？
应该给多少加速度？
应该打多少方向？
```

### 1.6Vehicle Cmd Gate：最终安全门

看到的节点：
```
/control/vehicle_cmd_gate
```

它接收：
```
/control/trajectory_follower/control_cmd
/system/emergency/control_cmd
/external/control_cmd
```

最后输出：
```
/control/command/control_cmd
```

它解决的问题是：
```
当前是否允许自动驾驶控制？
是否处于 emergency？
是否有外部控制命令？
控制命令是否超过安全限制？
是否需要强制 stop？
```

### 1.6Auto 按钮到底做了什么

看到服务：
```
/api/operation_mode/change_to_autonomous
/api/operation_mode/change_to_stop
/api/operation_mode/change_to_local
/api/operation_mode/change_to_remote
```

Autoware 的 Operation Mode 里，Autoware control mode 包含 Stop、Autonomous、Local、Remote 四种模式：Stop 保持车辆停止，Autonomous 由系统自主控制车辆，Local 用近端设备人工控制，Remote 用远程应用人工控制

也就是说，点击 RViz 里的 `Auto` 本质上是：
```
请求 Autoware 从 Stop / Local 等模式切到 Autonomous
```

对应命令是：
```
ros2 service call /api/operation_mode/change_to_autonomous \
  autoware_adapi_v1_msgs/srv/ChangeOperationMode {}
```

只有当 operation mode 允许自动控制时，`vehicle_cmd_gate` 才会把自动驾驶控制命令真正放行到车辆或 simulator
```
有 route，不一定会动
有 trajectory，不一定会动
有 trajectory_follower/control_cmd，也不一定会动
必须 Auto / Autonomous 后，最终 control_cmd 才会真正用于车辆运动
```

## 2.真实系统数据流

### 2.1总图：真实系统数据流
真实链路可以整理成：
```
/localization/kinematic_state
/map/vector_map
/perception/object_recognition/objects
/perception/occupancy_grid_map/map
/perception/traffic_light_recognition/traffic_signals
/system/operation_mode/state
        │
        ▼
[mission_planner]
        │
        ▼
/planning/mission_planning/route
        │
        ▼
[behavior_path_planner]
        │
        ▼
/planning/scenario_planning/lane_driving/behavior_planning/path_with_lane_id
        │
        ▼
[behavior_velocity_planner]
        │
        ▼
/planning/scenario_planning/lane_driving/behavior_planning/path
        │
        ▼
[path_optimizer]
        │
        ▼
/planning/scenario_planning/lane_driving/motion_planning/path_optimizer/trajectory
        │
        ▼
[motion_velocity_planner]
        │
        ▼
/planning/scenario_planning/lane_driving/trajectory
        │
        ▼
[scenario_selector]
        │
        ▼
/planning/scenario_planning/scenario_selector/trajectory
        │
        ▼
[velocity_smoother]
        │
        ▼
/planning/scenario_planning/velocity_smoother/trajectory
        │
        ▼
/planning/scenario_planning/trajectory
        │
        ▼
/planning/trajectory
        │
        ▼
[trajectory_follower/controller_node_exe]
        │
        ▼
/control/trajectory_follower/control_cmd
        │
        ▼
[vehicle_cmd_gate]
        │
        ▼
/control/command/control_cmd
        │
        ▼
[simple_planning_simulator]
        │
        ▼
/localization/kinematic_state

```

### 2.2第一层：Mission Planner

输出的是：
```
/planning/mission_planning/mission_planner
```

它订阅：
```
/localization/kinematic_state
/map/vector_map
/planning/scenario_planning/lane_driving/behavior_planning/behavior_path_planner/output/is_reroute_available
/planning/scenario_planning/modified_goal
/system/operation_mode/state
```

它发布：
```
/planning/mission_planning/route
/planning/mission_planning/route_marker
/planning/mission_planning/state
```

Mission Planner 的职责非常明确：
```
输入：
    当前车辆位置
    Lanelet2 矢量地图
    修改后的目标点
    当前 operation mode
    是否允许重新规划路线

输出：
    车道级 route
    route 可视化 marker
    route 状态

```

所以 Mission Planner 可以理解为：
```
汽车导航软件中的“路线规划器”
```

它不负责车辆怎么精细转弯，不负责速度，不负责转向控制。它只回答：
```
从当前车道到目标车道，应该经过哪些 lanelet？
```

### 2.3Behavior Path Planner

`behavior_path_planner` 订阅了很多东西：
```
/localization/kinematic_state
/map/vector_map
/perception/object_recognition/objects
/perception/occupancy_grid_map/map
/perception/traffic_light_recognition/traffic_signals
/planning/mission_planning/route
/planning/scenario_planning/max_velocity
/planning/scenario_planning/scenario
/system/operation_mode/state

```

它发布的核心输出是：
```
/planning/scenario_planning/lane_driving/behavior_planning/path_with_lane_id
/planning/scenario_planning/modified_goal
/planning/turn_indicators_cmd
/planning/behavior_path_planner/hazard_lights_cmd
/planning/scenario_planning/lane_driving/behavior_planning/behavior_path_planner/output/is_reroute_available
```

还有大量 debug 输出：
```
/planning/path_candidate/lane_change_left
/planning/path_candidate/lane_change_right
/planning/path_candidate/static_obstacle_avoidance
/planning/path_reference/...
/planning/planning_factors/...
/planning/cooperate_status/...
/planning/auto_mode_status/...

```

这说明 Behavior Path Planner 不是简单地“沿车道中心线走”。它会考虑：
```
当前车辆状态
地图
route
感知到的动态物体
占据栅格地图
交通灯状态
是否需要重新规划
是否需要变道
是否需要避障
是否需要修正目标点
是否需要转向灯或双闪
```

它的核心输出：
```
path_with_lane_id
```

可以理解为：
```
带有 lane 信息的行为级 path
```

### 2.4第三层：Behavior Velocity Planner

`behavior_velocity_planner` 订阅：
```
/localization/kinematic_state
/map/vector_map
/perception/object_recognition/objects
/perception/obstacle_segmentation/pointcloud
/perception/occupancy_grid_map/map
/perception/traffic_light_recognition/traffic_signals
/planning/scenario_planning/lane_driving/behavior_planning/path_with_lane_id
/planning/scenario_planning/max_velocity_default
```

发布的核心输出是：
```
/planning/scenario_planning/lane_driving/behavior_planning/path
```

以及大量规则相关 debug 输出：
```
/planning/planning_factors/blind_spot
/planning/planning_factors/crosswalk
/planning/planning_factors/detection_area
/planning/planning_factors/intersection
/planning/planning_factors/stop_line
/planning/planning_factors/traffic_light
/planning/planning_factors/virtual_traffic_light
/planning/planning_factors/walkway
```

说明 Behavior Velocity Planner 的职责是：
```
在 behavior path 的基础上，根据交通规则调整速度。
```

它重点处理：
```
红绿灯
停止线
路口
斑马线
盲区
检测区域
禁止停车区域
人行道
虚拟交通灯
```

所以可以这样区分：
```
behavior_path_planner:
    主要回答“走哪里？”

behavior_velocity_planner:
    主要回答“哪里要停？哪里要慢？哪里能走？”
```

### 2.5第四层：Motion Velocity Planner

`motion_velocity_planner` 订阅：
```
/planning/mission_planning/route
/planning/scenario_planning/lane_driving/motion_planning/path_optimizer/trajectory
/perception/object_recognition/objects
/perception/obstacle_segmentation/pointcloud
/perception/occupancy_grid_map/map
/perception/traffic_light_recognition/traffic_signals
/control/command/control_cmd
/control/trajectory_follower/lateral/predicted_trajectory
/localization/kinematic_state
/vehicle/status/steering_status
```

发布的核心输出是：
```
/planning/scenario_planning/lane_driving/trajectory
```

还有大量障碍物和安全相关 debug 轨迹：
```
/debug/obstacle_stop/trajectory
/debug/obstacle_slow_down/trajectory
/debug/obstacle_cruise/trajectory
/debug/dynamic_obstacle_stop/trajectory
/debug/out_of_lane/trajectory
/debug/run_out/trajectory
/debug/road_user_stop/trajectory
/debug/boundary_departure_prevention/trajectory
```

这说明 Motion Velocity Planner 更偏向：
```
基于障碍物、车辆状态和已有轨迹，进一步修改速度。
```

它比 Behavior Velocity Planner 更接近“动态障碍物和运动层”的速度处理

可以这样理解：
```
Behavior Velocity Planner:
    更偏交通规则
    例如红绿灯、停止线、路口、斑马线

Motion Velocity Planner:
    更偏运动安全和障碍物
    例如障碍物停车、障碍物减速、防止出车道、动态障碍物停车
```

### 2.6# 第五层：Velocity Smoother

`velocity_smoother` 订阅：
```
/localization/kinematic_state
/localization/acceleration
/planning/scenario_planning/max_velocity
/planning/scenario_planning/scenario_selector/trajectory
/system/operation_mode/state
```

它发布：
```
/planning/scenario_planning/velocity_smoother/trajectory
```

还有很多 debug 轨迹：
```
/debug/trajectory_raw
/debug/trajectory_time_resampled
/debug/trajectory_lateral_acc_filtered
/debug/trajectory_steering_rate_limited
/debug/forward_filtered_trajectory
/debug/backward_filtered_trajectory
/debug/merged_filtered_trajectory
```

这说明它的作用是：
```
对输入轨迹做速度平滑和约束处理。
```

它会考虑：
```
当前速度
当前加速度
最大速度限制
operation mode
横向加速度限制
转向速率限制
时间采样
前向/后向速度过滤
```

可以把它理解为：
```
让 trajectory 从“可行”变成“更舒适、更连续、更适合控制器跟踪”。
```

让 trajectory 从“可行”变成“更舒适、更连续、更适合控制器跟踪”。

自动驾驶车辆不能突然从 0 跳到 3 m/s，也不能在目标点突然速度断崖式变成 0，所以要做速度平滑

### 2.7第六层：Trajectory Follower

控制器节点是：
```
/control/trajectory_follower/controller_node_exe
```

它订阅：
```
/planning/trajectory
/localization/kinematic_state
/localization/acceleration
/system/operation_mode/state
/vehicle/status/steering_status
/vehicle/calibration/steer_offset_estimator/steering_offset_update
```

它发布：
```
/control/trajectory_follower/control_cmd
/control/trajectory_follower/lateral/predicted_trajectory
/control/trajectory_follower/controller_node_exe/debug/resampled_reference_trajectory
/control/trajectory_follower/controller_node_exe/debug/predicted_trajectory_in_frenet_coordinate
/control/trajectory_follower/lateral/diagnostic
/control/trajectory_follower/longitudinal/diagnostic
```

### 2.8第七层：Vehicle Cmd Gate

`vehicle_cmd_gate` 订阅了很多控制来源：
```
/control/trajectory_follower/control_cmd
/external/selected/control_cmd
/system/emergency/control_cmd
/system/operation_mode/state
/autoware/engage
/control/gate_mode_cmd
/localization/kinematic_state
/vehicle/status/steering_status
```

它发布最终控制命令：
```
/control/command/control_cmd
/control/command/gear_cmd
/control/command/hazard_lights_cmd
/control/command/turn_indicators_cmd
/control/current_gate_mode
/control/vehicle_cmd_gate/operation_mode
/control/vehicle_cmd_gate/is_paused
/control/vehicle_cmd_gate/is_stopped
/control/vehicle_cmd_gate/is_filter_activated
```

这说明它不是普通转发节点，而是最终安全门

它需要决定：
```
现在应该使用自动驾驶命令？
还是外部控制命令？
还是紧急停车命令？
是否已经 Engage？
是否是 Autonomous 模式？
是否需要 Pause？
是否需要 Stop？
控制命令是否要限幅？
```

所以主控制链是：
```
trajectory_follower/control_cmd
        ↓
vehicle_cmd_gate
        ↓
control/command/control_cmd
        ↓
simple_planning_simulator 或真实车辆接口
```

### 2.9整理成模块表

![[Pasted image 20260515161408.png]]
### 2.10必须掌握的调试口诀

```
1. 有没有 /localization/kinematic_state？
   没有：定位/仿真车辆状态有问题。

2. 有没有 /map/vector_map？
   没有：地图没加载。

3. 有没有 /planning/mission_planning/route？
   没有：目标点或 route 规划失败。

4. 有没有 /planning/scenario_planning/lane_driving/behavior_planning/path_with_lane_id？
   没有：behavior path planner 失败。

5. 有没有 /planning/scenario_planning/lane_driving/trajectory？
   没有：motion planning 失败。

6. 有没有 /planning/trajectory？
   没有：最终 planning 输出没出来。

7. 有没有 /control/trajectory_follower/control_cmd？
   没有：控制器没有工作。

8. 有没有 /control/command/control_cmd？
   没有：vehicle_cmd_gate 没有输出。

9. operation mode 是否是 Autonomous？
   不是：车辆不会真正执行自动驾驶。

10. /localization/kinematic_state 是否在变化？
    不变化：车辆或 simulator 没有执行控制命令。
```

## 3.理解 Autoware 如何把“路线”加工成“可控制的轨迹”

```
Route
  /planning/mission_planning/route
  由 mission_planner 生成
  含义：走哪些车道

PathWithLaneId
  /planning/.../path_with_lane_id
  由 behavior_path_planner 生成
  含义：带车道语义的路径

Path
  /planning/.../behavior_planning/path
  由 behavior_velocity_planner 生成
  含义：经过交通规则速度处理的行为路径

Trajectory
  /planning/.../path_optimizer/trajectory
  由 path_optimizer 生成
  含义：更平滑、更适合车辆运动的轨迹

Trajectory
  /planning/.../lane_driving/trajectory
  由 motion_velocity_planner 生成
  含义：经过障碍物和运动安全处理的轨迹

Trajectory
  /planning/.../velocity_smoother/trajectory
  由 velocity_smoother 生成
  含义：速度、加速度、转向变化更平滑的轨迹

Final Trajectory
  /planning/trajectory
  给控制器使用

ControlCmd
  /control/trajectory_follower/control_cmd
  控制器原始输出

Final ControlCmd
  /control/command/control_cmd
  最终发给车辆或仿真器

```

```
velocity_smoother 生成最终平滑轨迹
        ↓
/planning/trajectory 成为 Planning 和 Control 的正式接口
        ↓
trajectory_follower 根据轨迹生成原始控制命令
        ↓
vehicle_cmd_gate 对控制命令进行安全处理和限幅
        ↓
/control/command/control_cmd 才是最终发给车辆/仿真器的命令

```

Planning 和 Control 接结构图
```
/planning/trajectory
    Type: autoware_planning_msgs/msg/Trajectory
    含义：Planning 给 Control 的最终轨迹
        ↓
[trajectory_follower/controller_node_exe]
    输入：
        /planning/trajectory
        /localization/kinematic_state
        /localization/acceleration
        /vehicle/status/steering_status
        /system/operation_mode/state
    输出：
        /control/trajectory_follower/control_cmd
        ↓
[vehicle_cmd_gate]
    输入：
        /control/trajectory_follower/control_cmd
        /system/emergency/control_cmd
        /external/selected/control_cmd
        /system/operation_mode/state
        /autoware/engage
        /localization/kinematic_state
        /vehicle/status/steering_status
    输出：
        /control/command/control_cmd
        ↓
[simple_planning_simulator]
    输入：
        /control/command/control_cmd
    输出：
        /localization/kinematic_state
```

```
Planning:
    生成 /planning/trajectory

Control:
    读取 /planning/trajectory
    读取 /localization/kinematic_state
    生成 /control/trajectory_follower/control_cmd

Gate:
    读取 control_cmd
    根据 operation mode / emergency / limits 处理
    生成 /control/command/control_cmd

Simulator:
    根据最终 control_cmd 推动车辆
    更新 /localization/kinematic_state

```