- [[#1.1 通用准备|1.1 通用准备]]
	- [[#1.1 通用准备#1.1.1 安装基础工具|1.1.1 安装基础工具]]
	- [[#1.1 通用准备#1.1.2 下载 Autoware 源码|1.1.2 下载 Autoware 源码]]
	- [[#1.1 通用准备#1.1.3 导入 Autoware 子仓库|1.1.3 导入 Autoware 子仓库]]
- [[#1.2 正常电脑编译：已经安装 acados|1.2 正常电脑编译：已经安装 acados]]
	- [[#1.2 正常电脑编译：已经安装 acados#1.2.1 配置 acados 环境变量|1.2.1 配置 acados 环境变量]]
	- [[#1.2 正常电脑编译：已经安装 acados#1.2.2 正式编译|1.2.2 正式编译]]
- [[#1.3 正常电脑编译：未安装 acados|1.3 正常电脑编译：未安装 acados]]
	- [[#1.3 正常电脑编译：未安装 acados#1.3.1 安装 acados 基础依赖|1.3.1 安装 acados 基础依赖]]
	- [[#1.3 正常电脑编译：未安装 acados#1.3.2 下载并编译 acados|1.3.2 下载并编译 acados]]
	- [[#1.3 正常电脑编译：未安装 acados#1.3.3 配置 acados Python 环境|1.3.3 配置 acados Python 环境]]
	- [[#1.3 正常电脑编译：未安装 acados#1.3.4 写入环境变量|1.3.4 写入环境变量]]
- [[#1.4 工控机编译：清理旧环境并重新编译|1.4 工控机编译：清理旧环境并重新编译]]
	- [[#1.4 工控机编译：清理旧环境并重新编译#1.4.1 清理旧编译产物|1.4.1 清理旧编译产物]]
	- [[#1.4 工控机编译：清理旧环境并重新编译#1.4.2 检查 swap|1.4.2 检查 swap]]
	- [[#1.4 工控机编译：清理旧环境并重新编译#1.4.3 工控机正式编译命令|1.4.3 工控机正式编译命令]]
	- [[#1.4 工控机编译：清理旧环境并重新编译#1.4.4 编译完成验证|1.4.4 编译完成验证]]
- [[#1.5 常见编译错误快速处理|1.5 常见编译错误快速处理]]
	- [[#1.5 常见编译错误快速处理#1.5.1 `autoware_launch` 找不到|1.5.1 `autoware_launch` 找不到]]
	- [[#1.5 常见编译错误快速处理#1.5.2 缺少 `autoware_lidar_marker_localizer/package.sh`|1.5.2 缺少 `autoware_lidar_marker_localizer/package.sh`]]
	- [[#1.5 常见编译错误快速处理#1.5.3 `autoware_path_optimizer` 找不到 acados|1.5.3 `autoware_path_optimizer` 找不到 acados]]
	- [[#1.5 常见编译错误快速处理#1.5.4 Ninja 和 Unix Makefiles 冲突|1.5.4 Ninja 和 Unix Makefiles 冲突]]
	- [[#1.5 常见编译错误快速处理#1.5.5 `json.decoder.JSONDecodeError`|1.5.5 `json.decoder.JSONDecodeError`]]
	- [[#1.5 常见编译错误快速处理#1.5.6 `source install/setup.bash` 提示某包 `local_setup.bash` 不存在|1.5.6 `source install/setup.bash` 提示某包 `local_setup.bash` 不存在]]
- [[#1.6本机源码编译编acados的情况|1.6本机源码编译编acados的情况]]
	- [[#1.6本机源码编译编acados的情况#1.6.1acados环境路径写入|1.6.1acados环境路径写入]]
	- [[#1.6本机源码编译编acados的情况#1.6.2给 acados 创建 `.venv`|1.6.2给 acados 创建 `.venv`]]
	- [[#1.6本机源码编译编acados的情况#1.6.3安装 acados Python 接口|1.6.3安装 acados Python 接口]]
	- [[#1.6本机源码编译编acados的情况#1.6.4再次用 `.venv/bin/python3` 验证|1.6.4再次用 `.venv/bin/python3` 验证]]
	- [[#1.6本机源码编译编acados的情况#1.6.5清理编译|1.6.5清理编译]]
- [[#1. 安装系统基础依赖|1. 安装系统基础依赖]]
- [[#2.安装 Rust 环境|2.安装 Rust 环境]]
- [[#3. 安装 Node.js 与 pnpm|3. 安装 Node.js 与 pnpm]]
- [[#4. 克隆项目并安装前端依赖|4. 克隆项目并安装前端依赖]]
- [[#5. 解决编译报错（核心修正步骤）|5. 解决编译报错（核心修正步骤）]]
- [[#6. source环境出现警告错误|6. source环境出现警告错误]]
- [[#7.验证官方 Demo|7.验证官方 Demo]]
- [[#8.用本地模型替换 sample 模型|8.用本地模型替换 sample 模型]]

# 1.源码编译

本节把 Autoware 源码编译分成三种情况，避免把工控机低资源编译流程和普通开发电脑流程混在一起。

当前 WVCSC 实车部署推荐路径：

```text
Autoware 源码工作区：/home/eisa/autoware
WVCSC 实车工作区：/home/eisa/WVCSC_S2Z_UTB_ARM
acados 推荐路径：/home/eisa/.local/acados
ROS 版本：ROS 2 Humble
Ubuntu：22.04
```

## 1.1 通用准备

三种情况都先完成下面这些步骤。

### 1.1.1 安装基础工具

```bash
sudo apt update
sudo apt install -y git python3-vcstool python3-rosdep python3-pip
```

如果 `rosdep` 没初始化过：

```bash
sudo rosdep init
rosdep update
```

如果已经初始化过，只执行：

```bash
rosdep update
```

### 1.1.2 下载 Autoware 源码

如果本机还没有 `/home/eisa/autoware`：

```bash
cd /home/eisa
git clone https://github.com/autowarefoundation/autoware.git
cd /home/eisa/autoware
```

如果要使用固定稳定版本，例如 `1.8.0`：

```bash
cd /home/eisa/autoware
git checkout 1.8.0
```

安装 Ansible 依赖：

```bash
cd /home/eisa/autoware
bash ansible/scripts/install-ansible.sh
ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
```

安装 Autoware 开发环境：

```bash
# 有 NVIDIA GPU
ansible-playbook autoware.dev_env.install_dev_env

# 没有 NVIDIA GPU 时使用
# ansible-playbook autoware.dev_env.install_dev_env --skip-tags nvidia
```

如果遇到 GitHub API rate limit，例如：

```text
API rate limit exceeded
```

不是源码问题，是 GitHub 未认证请求次数用完。处理方式：

```bash
# 等待 x_ratelimit_reset 对应时间后重试
# 或者配置 GitHub token 后重试
```

### 1.1.3 导入 Autoware 子仓库

```bash
cd /home/eisa/autoware
mkdir -p src
vcs import src < repositories/autoware.repos
```

安装 ROS 依赖：

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro humble
```

## 1.2 正常电脑编译：已经安装 acados

适用于开发电脑性能较好，且 acados 已经安装完成的情况。

### 1.2.1 配置 acados 环境变量

假设 acados 安装在：

```text
/home/eisa/.local/acados
```

每次编译前执行：

```bash
export ACADOS_SOURCE_DIR=/home/eisa/.local/acados
export CMAKE_PREFIX_PATH="/home/eisa/.local/acados:${CMAKE_PREFIX_PATH}"
export LD_LIBRARY_PATH="/home/eisa/.local/acados/lib:${LD_LIBRARY_PATH}"
export PATH="/home/eisa/.local/acados/bin:${PATH}"
export PYTHONPATH="/home/eisa/.local/acados/interfaces/acados_template:${PYTHONPATH}"
```

验证 acados：

```bash
ls $ACADOS_SOURCE_DIR/lib/libacados.so
ls $ACADOS_SOURCE_DIR/bin/t_renderer
ldd $ACADOS_SOURCE_DIR/lib/libacados.so | grep "not found"
```

如果最后一条没有输出，说明动态库依赖基本正常。

### 1.2.2 正式编译

用户指定的标准编译命令如下：

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash

export ACADOS_SOURCE_DIR=/home/eisa/.local/acados
export CMAKE_PREFIX_PATH="/home/eisa/.local/acados:${CMAKE_PREFIX_PATH}"
export LD_LIBRARY_PATH="/home/eisa/.local/acados/lib:${LD_LIBRARY_PATH}"
export PATH="/home/eisa/.local/acados/bin:${PATH}"
export PYTHONPATH="/home/eisa/.local/acados/interfaces/acados_template:${PYTHONPATH}"

colcon build \
  --symlink-install \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

编译完成后：

```bash
source install/setup.bash
ros2 pkg prefix autoware_launch
ros2 launch autoware_launch planning_simulator.launch.xml --show-args
```

## 1.3 正常电脑编译：未安装 acados

适用于普通开发电脑还没有 acados 的情况。先安装 acados，再执行 `1.2` 的编译流程。

### 1.3.1 安装 acados 基础依赖

```bash
sudo apt update
sudo apt install -y \
  git cmake build-essential python3-venv python3-pip python3-dev \
  libblas-dev liblapack-dev
```

### 1.3.2 下载并编译 acados

```bash
mkdir -p /home/eisa/.local
cd /home/eisa/.local
git clone https://github.com/acados/acados.git
cd /home/eisa/.local/acados
git submodule update --recursive --init

mkdir -p build
cd build
cmake -DACADOS_WITH_QPOASES=ON -DCMAKE_POSITION_INDEPENDENT_CODE=ON ..
make install -j$(nproc)
```

### 1.3.3 配置 acados Python 环境

```bash
cd /home/eisa/.local/acados
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
python -m pip install "numpy<2" scipy matplotlib casadi
python -m pip install -e interfaces/acados_template
deactivate
```

验证：

```bash
/home/eisa/.local/acados/.venv/bin/python3 - <<'EOF'
import sys
import casadi
import numpy
import acados_template
print("Python:", sys.executable)
print("casadi:", casadi.__version__)
print("numpy:", numpy.__version__)
print("acados_template OK")
EOF
```

### 1.3.4 写入环境变量

```bash
grep -q "ACADOS_SOURCE_DIR=/home/eisa/.local/acados" ~/.bashrc || cat >> ~/.bashrc <<'EOF'
export ACADOS_SOURCE_DIR=/home/eisa/.local/acados
export CMAKE_PREFIX_PATH="/home/eisa/.local/acados:${CMAKE_PREFIX_PATH}"
export LD_LIBRARY_PATH="/home/eisa/.local/acados/lib:${LD_LIBRARY_PATH}"
export PATH="/home/eisa/.local/acados/bin:${PATH}"
export PYTHONPATH="/home/eisa/.local/acados/interfaces/acados_template:${PYTHONPATH}"
EOF
source ~/.bashrc
```

然后回到 `1.2.2` 执行 Autoware 编译。

## 1.4 工控机编译：清理旧环境并重新编译

适用于 WVCSC 工控机 `/home/eisa/autoware` 已经编译过，但中途出现过卡死、中断、CMake 缓存污染、包残缺安装等情况。

### 1.4.1 清理旧编译产物

只删除编译产物，不删除源码：

```bash
cd /home/eisa/autoware

# 确认没有正在编译的进程
ps -ef | grep -E "colcon|cmake|gmake|make|ninja|cc1plus" | grep -v grep

# 清理旧 build/install/log
rm -rf build install log
```

不要删除：

```text
/home/eisa/autoware/src
/home/eisa/autoware/repositories
/home/eisa/autoware_data
```

### 1.4.2 检查 swap

工控机编译 Autoware 容易因为内存压力导致桌面卡住或编译中断。建议至少保证 30G 以上总内存加 swap。

```bash
free -h
swapon --show
```

如果还没有额外 swapfile，可以追加 16G：

```bash
sudo fallocate -l 16G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
free -h
```

如果 `/swapfile` 已经存在，不要重复创建，只检查：

```bash
swapon --show
free -h
```

### 1.4.3 工控机正式编译命令

按当前要求，工控机也使用下面这个主编译命令：

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash

export ACADOS_SOURCE_DIR=/home/eisa/.local/acados
export CMAKE_PREFIX_PATH="/home/eisa/.local/acados:${CMAKE_PREFIX_PATH}"
export LD_LIBRARY_PATH="/home/eisa/.local/acados/lib:${LD_LIBRARY_PATH}"
export PATH="/home/eisa/.local/acados/bin:${PATH}"
export PYTHONPATH="/home/eisa/.local/acados/interfaces/acados_template:${PYTHONPATH}"

colcon build \
  --symlink-install \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

如果桌面卡住，不一定代表编译停止。另开 TTY 或 SSH 后检查：

```bash
ps -o pid,ppid,pcpu,pmem,etime,cmd -C colcon -C cmake -C gmake -C make -C ninja -C cc1plus -C c++
free -h
```

如果内存持续打满、系统无响应，再考虑中断后改用更保守命令救急：

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash

export ACADOS_SOURCE_DIR=/home/eisa/.local/acados
export CMAKE_PREFIX_PATH="/home/eisa/.local/acados:${CMAKE_PREFIX_PATH}"
export LD_LIBRARY_PATH="/home/eisa/.local/acados/lib:${LD_LIBRARY_PATH}"
export PATH="/home/eisa/.local/acados/bin:${PATH}"
export PYTHONPATH="/home/eisa/.local/acados/interfaces/acados_template:${PYTHONPATH}"

colcon build \
  --symlink-install \
  --parallel-workers 1 \
  --executor sequential \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

注意：上面 `parallel-workers 1` 是工控机救急方案；标准主流程仍以 `parallel-workers 2` 为准。

### 1.4.4 编译完成验证

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash
source install/setup.bash

ros2 pkg prefix autoware_launch
ros2 pkg prefix tier4_localization_launch
ros2 pkg prefix tier4_perception_launch
ros2 pkg prefix autoware_path_optimizer
```

官方 planning simulator 参数验证：

```bash
ros2 launch autoware_launch planning_simulator.launch.xml --show-args
```

官方示例运行：

```bash
ros2 launch autoware_launch planning_simulator.launch.xml \
  map_path:=$HOME/autoware_data/maps/sample-map-planning \
  vehicle_model:=sample_vehicle \
  sensor_model:=sample_sensor_kit
```

WVCSC 工作区运行前 source 顺序：

```bash
cd /home/eisa/WVCSC_S2Z_UTB_ARM
source /opt/ros/humble/setup.bash
source /home/eisa/autoware/install/setup.bash
source install/setup.bash
```

然后再启动 WVCSC 实车链，例如：

```bash
ros2 launch wvcsc_autoware_bringup hybrid_real_vehicle.launch.xml \
  map_path:=/home/eisa/autoware_data/maps/wvcsc_map \
  rviz:=true
```

## 1.5 常见编译错误快速处理

### 1.5.1 `autoware_launch` 找不到

说明 `autoware_launch` 或它的上游依赖没有完整编译安装。

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash

colcon build \
  --packages-up-to autoware_launch \
  --symlink-install \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### 1.5.2 缺少 `autoware_lidar_marker_localizer/package.sh`

错误类似：

```text
Failed to find:
/home/eisa/autoware/install/autoware_lidar_marker_localizer/share/autoware_lidar_marker_localizer/package.sh
```

先编译缺失包及其依赖：

```bash
colcon build \
  --packages-up-to autoware_lidar_marker_localizer \
  --symlink-install \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

然后再编译目标 launch 包。

### 1.5.3 `autoware_path_optimizer` 找不到 acados

先确认 acados 环境变量：

```bash
export ACADOS_SOURCE_DIR=/home/eisa/.local/acados
export CMAKE_PREFIX_PATH="/home/eisa/.local/acados:${CMAKE_PREFIX_PATH}"
export LD_LIBRARY_PATH="/home/eisa/.local/acados/lib:${LD_LIBRARY_PATH}"
```

清理该包 CMake 缓存后重编：

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash

colcon build \
  --packages-select autoware_path_optimizer \
  --symlink-install \
  --cmake-clean-cache \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

如果仍失败：

```bash
rm -rf build/autoware_path_optimizer install/autoware_path_optimizer

colcon build \
  --packages-select autoware_path_optimizer \
  --symlink-install \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### 1.5.4 Ninja 和 Unix Makefiles 冲突

错误类似：

```text
generator : Ninja does not match the generator used previously: Unix Makefiles
```

这是旧 CMake 缓存污染。处理：

```bash
cd /home/eisa/autoware
rm -rf build/<出错包名>
```

或者全清：

```bash
rm -rf build install log
```

然后重新编译。

### 1.5.5 `json.decoder.JSONDecodeError`

通常是上一次编译中断导致 CMake reply JSON 空文件或损坏。

```bash
cd /home/eisa/autoware
rm -rf build/<出错包名>

colcon build \
  --packages-select <出错包名> \
  --symlink-install \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### 1.5.6 `source install/setup.bash` 提示某包 `local_setup.bash` 不存在

说明该包 install 目录残缺。不要只手工创建空文件作为长期方案，应重新编译对应包：

```bash
cd /home/eisa/autoware
source /opt/ros/humble/setup.bash

colcon build \
  --packages-select <缺失包名> \
  --symlink-install \
  --cmake-clean-cache \
  --parallel-workers 2 \
  --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## 1.6本机源码编译编acados的情况

**acados 已经存在，问题不是没安装，而是 CMake 查找路径没有配置正确**

解决办法：
### 1.6.1acados环境路径写入
```
#在bashrc中写入相关路径，根据你下载安装编译的acados路径而定
gedit ~/.bashrc
```

文件末尾空白处写入：
```
export ACADOS_INSTALL_DIR="/home/robot/桌面/acados_toolkit/acados"  
export ACADOS_SOURCE_DIR="$ACADOS_INSTALL_DIR"  
export acados_DIR="$ACADOS_INSTALL_DIR/cmake"  
export CMAKE_PREFIX_PATH="$ACADOS_INSTALL_DIR:$CMAKE_PREFIX_PATH"  
export LD_LIBRARY_PATH="$ACADOS_INSTALL_DIR/lib:$LD_LIBRARY_PATH"
```

### 1.6.2给 acados 创建 `.venv`

```
#执行
cd /home/robot/桌面/acados_toolkit/acados
#安装 Python 虚拟环境工具：
sudo apt update
sudo apt install -y python3-venv python3-pip python3-dev
#创建 `.venv`：
python3 -m venv .venv
#检查：
ls /home/robot/桌面/acados_toolkit/acados/.venv/bin/python3
#现在应该能看到：
/home/robot/桌面/acados_toolkit/acados/.venv/bin/python3
```

### 1.6.3安装 acados Python 接口

```
#进入 acados 目录：
cd /home/robot/桌面/acados_toolkit/acados
#激活虚拟环境,安装依赖
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
python -m pip install "numpy<2" scipy matplotlib casadi==3.7.2
python -m pip install -e interfaces/acados_template
deactivate
```
### 1.6.4再次用 `.venv/bin/python3` 验证

```
/home/robot/桌面/acados_toolkit/acados/.venv/bin/python3 - <<'EOF'
import sys
import casadi
import numpy
import acados_template

print("Python:", sys.executable)
print("casadi:", casadi.__version__)
print("numpy:", numpy.__version__)
print("acados_template OK")
EOF
```

正确结果应该类似：
```
Python: /home/robot/桌面/acados_toolkit/acados/.venv/bin/python3
casadi: 3.7.2
numpy: 1.xx.x
acados_template OK
```

### 1.6.5清理编译

```
rm -rf src/*/*/autoware_path_optimizer/src/acados_mpc/c_generated_code
cd /home/robot/autoware

source /opt/ros/humble/setup.bash
source ~/.bashrc

export ACADOS_INSTALL_DIR="/home/robot/桌面/acados_toolkit/acados"
export ACADOS_SOURCE_DIR="$ACADOS_INSTALL_DIR"
export acados_DIR="$ACADOS_INSTALL_DIR/cmake"
export CMAKE_PREFIX_PATH="$ACADOS_INSTALL_DIR:$CMAKE_PREFIX_PATH"
export LD_LIBRARY_PATH="$ACADOS_INSTALL_DIR/lib:$LD_LIBRARY_PATH"

colcon build \
  --symlink-install \
  --packages-select autoware_path_optimizer \
  --cmake-args \
    -DCMAKE_BUILD_TYPE=Release \
    -Dacados_DIR="$ACADOS_SOURCE_DIR/cmake"
```


# 2.Autoware Build GUI可视化界面 编译安装全流程
## 1. 安装系统基础依赖
```
sudo apt install libwebkit2gtk-4.1-0 libjavascriptcoregtk-4.1-0 libsoup-3.0-0 libsoup-3.0-common
```

## 2.安装 Rust 环境
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs/ | sh
```

安装完成后，终端无法直接识别 `rustc` 命令。需要手动刷新当前 Shell 的环境变量：
```
source "$HOME/.cargo/env"
```

## 3. 安装 Node.js 与 pnpm
```
sudo apt update
sudo apt install nodejs
npm install -g pnpm
```

## 4. 克隆项目并安装前端依赖

拉取 Autoware Build GUI 的源码并进入目录：
```
git clone https://github.com/leo-drive/autoware-build-gui.git cd autoware-build-gui pnpm i
```

## 5. 解决编译报错（核心修正步骤）

**修正操作**：安装缺失的开发依赖库：
```
sudo apt install libatk1.0-dev libgdk-pixbuf2.0-dev libgtk-3-dev libwebkit2gtk-4.1-dev
```
_如果在后续编译中提示找不到 `libclang`，
可额外执行
```
sudo apt install libclang-dev
```

补全依赖后，强制重新安装 pnpm 依赖以确保环境干净，最后启动开发版应用：
```
pnpm install --force 
pnpm tauri dev
```

## 6. source环境出现警告错误

问题：
```
robot@eisa:~/autoware$ source install/setup.bash not found: "/home/robot/autoware/install/autoware_tensorrt_common/share/autoware_tensorrt_common/local_setup.bash" not found: "/home/robot/autoware/install/autoware_tensorrt_classifier/share/autoware_tensorrt_classifier/local_setup.bash" not found: "/home/robot/autoware/install/autoware_tensorrt_plugins/share/autoware_tensorrt_plugins/local_setup.bash" not found: "/home/robot/autoware/install/bevdet_vendor/share/bevdet_vendor/local_setup.bash"
```

解决办法：
```
cd /home/robot/autoware

for pkg in \
  autoware_tensorrt_common \
  autoware_tensorrt_classifier \
  autoware_tensorrt_plugins \
  bevdet_vendor
do
  mkdir -p install/$pkg/share/$pkg
  touch install/$pkg/share/$pkg/local_setup.bash
  touch install/$pkg/share/$pkg/local_setup.sh
  touch install/$pkg/share/$pkg/local_setup.zsh
done
```

然后重新 source：
```
source /opt/ros/humble/setup.bash
source /home/robot/autoware/install/setup.bash
```
## 7.验证官方 Demo

在容器里执行：
```
ros2 launch autoware_launch planning_simulator.launch.xml \
  map_path:=/home/robot/autoware_data/maps/sample-map-planning \
  vehicle_model:=sample_vehicle \
  sensor_model:=sample_sensor_kit
```

## 8.用本地模型替换 sample 模型

```
ros2 launch autoware_launch planning_simulator.launch.xml \
  map_path:=/home/robot/autoware_data/maps/sample-map-planning \
  vehicle_model:=wvcsc_vehicle \
  sensor_model:=wvcsc_sensor_kit
  
```