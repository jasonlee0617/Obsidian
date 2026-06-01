# 1.初次Git上传步骤
## 1.1命令行

### 1.1.1进入工作区src

```
cd ~/工作区根目录/src
```

### 1.1.2配置 Git 用户名和邮箱

```
##git config --global user.name "你的GitHub用户名"
##git config --global user.email "你的GitHub邮箱"
git config --global user.name "jasonlee0617"  
git config --global user.email "676983492@qq.com"
```

检查是否配置成功：
```
git config --global user.name
git config --global user.email
```

正常输出应该是：
```
jasonlee0617
676983492@qq.com
```

### 1.1.3初始化 Git 仓库

在 `src` 目录下执行：
```
git init
```

然后把默认分支改成 `main`：
```
git branch -M main
```

### 1.1.4创建 `.gitignore` 文件(如果是ros2包)可选

在 `src` 目录下执行：
```
touch .gitignore
code .gitignore
```

然后把下面内容复制进去：
```
# ROS2 build artifacts
build/
install/
log/

# Python cache
__pycache__/
*.pyc
*.pyo
*.pyd

# C/C++ build files
*.o
*.so
*.a
*.out

# CMake
CMakeFiles/
CMakeCache.txt
cmake_install.cmake
Makefile

# Colcon
.colcon/
.metadata/

# VS Code personal settings
.vscode/ipch/
.vscode/.browse.c_cpp.db*
.vscode/c_cpp_properties.json

# System files
.DS_Store
Thumbs.db

# Logs
*.log
```

### 1.1.5查看当前 Git 状态

```
git status
```
应该会看到很多红色文件，表示它们还没有被 Git 跟踪

### 1.1.6添加所有文件到暂存区

```
git add .
```

再次查看状态：
```
git status
```
这时文件应该变成绿色，表示已经准备提交

### 1.1.7提交代码

```
git commit -m "Initial commit: add ROS2 Humble S622 robot arm source packages"
```

如果成功，会看到类似：
```
[main xxxxxxx] Initial commit: add ROS2 Humble S622 robot arm source packages
```

### 1.1.8添加 GitHub 远程仓库

```
##git remote add origin git@github.com:你的用户名/你的仓库名.git
git remote add origin https://github.com/jasonlee0617/S622_robotarm_src.git
```

然后检查：
```
git remote -v
```

应该看到：
```
origin  https://github.com/jasonlee0617/S622_robotarm_src.git (fetch)
origin  https://github.com/jasonlee0617/S622_robotarm_src.git (push)
```

### 1.1.9上传到 GitHub

```
git push -u origin main
```

上传失败，重新初始化git:
```
##删除本地 Git 记录
rm -rf .git
```

然后重新初始化：
```
git init
git branch -M main

```

重新设置远程仓库：
```
##git remote add origin git@github.com:你的用户名/你的仓库名.git
git remote add origin https://github.com/jasonlee0617/S622_robotarm_src.git
```

```
git status
git add .
git commit -m "Initial commit: add ROS2 Humble S622 robot arm source packages"
git push -u origin main
```


### 1.1.10冲突文件解决

![[Pasted image 20260601145816.png]]


```
git pull --rebase origin main
```

## 1.2可视化
### 1.2.1第一次上传前，先打开正确文件夹

在终端执行
```
#根据你的工作区操作
cd ~/S622_robotarm/src
code .
```

注意：不要打开到整个工作区根目录：
因为根目录下面可能有：
```
build/
install/
log/
```
这些不应该上传到 GitHub

### 1.2.2第一次上传前，先创建 `.gitignore`.

然后填入下面内容：
```
# ROS2 / colcon build output
build/
install/
log/

# Python virtual environments
venv/
.venv/
env/
ENV/
*/venv/
*/.venv/
*/env/
*/ENV/
*/lib/python*/
*/lib64/python*/
*/site-packages/

# Python cache
__pycache__/
*.pyc
*.pyo
*.pyd

# Python package cache
*.egg-info/
.eggs/
dist/

# CMake / C++ temporary files
CMakeFiles/
CMakeCache.txt
cmake_install.cmake
Makefile
*.o
*.so
*.so.*
*.a
*.out

# AI / model files
*.pt
*.pth
*.onnx
*.engine
*.weights
*.tflite
*.h5

# ROS bag / database files
*.bag
*.db3
*.mcap

# Dataset / training output
datasets/
dataset/
runs/
wandb/
outputs/
checkpoints/

# Compressed files
*.zip
*.tar
*.tar.gz
*.tgz
*.rar
*.7z

# Media files
*.mp4
*.avi
*.mov
*.mkv

# VS Code cache
.vscode/ipch/
.vscode/.browse.c_cpp.db*
.vscode/c_cpp_properties.json

# System files
.DS_Store
Thumbs.db
```

### 1.2.3打开 VS Code 源代码管理界面

点击左侧这个图标：
```
源代码管理 / Source Control
```

快捷键是：
```
Ctrl + Shift + G
```

### 1.2.4如果还没有 Git 仓库，点击“初始化仓库”

如果 VS Code 看到你的 `src` 目录还不是 Git 仓库，它会显示类似按钮：
```
初始化仓库
Initialize Repository
```

点击它
这一步等价于命令：
```
git init
```

初始化完成后，VS Code 左侧会出现很多文件，通常会显示：
```
更改
```

### 1.2.5暂存文件：相当于 `git add`

暂存单个文件
鼠标放到某个文件上，点击右侧的：
```
+
```

这相当于：
```
更改 / Changes 右侧的 +
```

这相当于：
```
git add .
```

### 1.2.6填写第一次提交信息

在 Source Control 顶部输入框里写提交信息，例如：
```
Initial commit: add ROS2 Humble S622 robot arm source packages
```

然后点击：
```
提交 / Commit
```

这一步等价于：
```
git commit -m "Initial commit: add ROS2 Humble S622 robot arm source packages"
```

提交后，这些文件就进入了本地 Git 历史。
注意：**提交 commit 只是保存到本地，还没有上传到 GitHub**

### 1.2.7第一次上传到 GitHub：两种可视化方式

#### 情况 A：你还没有在 GitHub 网页创建仓库

这种最简单，直接用 VS Code 的：
```
Publish to GitHub
```

操作步骤：
```
1. 打开 Source Control
2. 点击 Publish to GitHub
3. VS Code 要求登录 GitHub 时，点击允许/登录
4. 输入仓库名，例如 S622_robotarm_src
5. 选择 Public 或 Private
6. 确认要上传的文件
7. 点击发布
```

如果是公开仓库，选择：
```
Public
```

如果只想自己看，选择：
```
Private
```
发布成功后，VS Code 会自动把本地仓库连接到 GitHub，并完成第一次推送

####  情况 B：你已经在 GitHub 网页创建好了仓库
##### 方法 1：VS Code 图形方式添加远程仓库
在 Source Control 面板中点击：
```
...
```

然后找：
```
远程 / Remote
→ 添加远程仓库 / Add Remote
```

如果菜单里没有，也可以用快捷命令：
```
Ctrl + Shift + P
```

输入：
```
Git: Add Remote
```

然后按提示输入远程仓库地址：
```
##根据你的仓库名创建
https://github.com/jasonlee0617/S622_robotarm_src.git
```

远程名称输入：
```
origin
```

##### 方法 2：用 VS Code 终端添加远程仓库
这个虽然是命令，但最稳定：
```
##根据你的仓库名字相应创建
cd ~/S622_robotarm/src
git remote add origin https://github.com/jasonlee0617/S622_robotarm_src.git
```
# 2.日常同步本地代码到 GitHub 的标准流程
## 2.1命令行同步

```
先确认状态 → 拉取远程更新 → 添加修改 → 提交 → 推送
```

```
cd ~/S622_robotarm/src
git status
git pull --rebase origin main
git add .
git commit -m "update S622 robot arm source code"
git push
git status
```


### 2.1.1进入你的 ROS2 src 仓库

```
cd ~/S622_robotarm/src
```

确认当前位置：
```
pwd
```

应该显示：
```
/home/robot/S622_robotarm/src
```

然后确认远程仓库是不是你的 GitHub 仓库：
```
git remote -v
```

正常应该显示：
```
origin  https://github.com/jasonlee0617/S622_robotarm_src.git (fetch)
origin  https://github.com/jasonlee0617/S622_robotarm_src.git (push)
```

### 2.1.2查看当前代码状态

```
git status
```

#### 情况 A：没有任何修改

如果显示：
```
无文件要提交，干净的工作区
```
说明本地没有新修改，不需要上传

#### 情况 B：有文件被修改

例如：
```
修改： fairino_planning_core/config/lk_params.yaml修改： yolov8_obb/scripts/yolov8_obb_publisher.py
```
说明这些文件改过，但还没有提交。
#### 情况 C：有新文件

例如：
```
未跟踪的文件：    new_launch_file.launch.py
```
说明这个文件还没有被 Git 管理，需要
```
git add
```

### 2.1.3先拉取 GitHub 最新代码

这是很重要的一步。  
因为如果 GitHub 上有你本地没有的提交，你直接 `git push` 会失败

推荐执行：
```
git pull --rebase origin main
```

如果成功，会看到类似：
```
Successfully rebased and updated refs/heads/main.
```

或者：
```
Already up to date.
```

### 2.1.4添加你本地修改的代码

如果你想上传所有修改：
```
git add .
```

如果你只想上传某个文件，例如：
```
git add fairino_planning_core/config/lk_params.yaml
```

或者：
```
git add yolov8_obb/scripts/yolov8_obb_publisher.py
```

添加后再看状态：
```
git status
```

### 2.1.5. 提交代码

提交就是给当前修改保存一个版本
```
git commit -m "update S622 robot arm source code"
```

### 2.1.6推送到 GitHub

提交完成后执行：
```
git push
```

### 2.1.7. 最后确认是否同步成功

```
git status
```

如果显示：
```
位于分支 main
您的分支与上游分支 'origin/main' 一致。

无文件要提交，干净的工作区
```


## 2.2可视化同步

### 2.2.1暂存修改

鼠标放到文件上，点击右侧的：
```
+
```

等价于：
```
git add 文件名
```

暂存所有文件
点击：
```
更改 / Changes 右侧的 +
```

等价于：
```
git add .
```

### 2.2.2写提交信息

在 Source Control 顶部输入框里写：
```
更新的详细备注
```

然后点击：
```
提交 / Commit
```

等价于：
```
git commit -m "fix fairino planning config"
```

### 2.2.3同步到 GitHub

提交之后，点击：
```
推送 / Push
```

或者点击蓝色按钮：
```
同步更改 ↑1
```

### 2.2.4VS Code 里 `↓1 ↑2` 是什么意思？

```
同步更改 ↓1 ↑2
```

意思是：
```
↓1：GitHub 上有 1 个提交需要拉取
↑2：你本地有 2 个提交需要推送
```

这种情况不能直接只点“推送”
你应该先：
```
拉取
```

再：
```
推送
```

或者用终端：
```
git pull --rebase origin main
git push
```

# 3.直接把当前 src 重新连接到远程仓库

先进入当前 `src`：
```
cd ~/S622_robotarm/src
```

初始化 Git：
```
git init
git branch -M main
```

添加远程仓库：
```
git remote add origin https://github.com/jasonlee0617/S622_robotarm_src.git
```

拉取远程信息：
```
git fetch origin
```

把当前仓库指向远程 `main` 历史：
```
git reset --mixed origin/main
```

然后检查状态：
```
git status
git branch -vv
git remote -v
```

如果你的当前文件和 GitHub 上完全一致，应该显示：
```
无文件要提交，干净的工作区
位于分支 main  
您的分支与上游分支 'origin/main' 一致。  
  
无文件要提交，干净的工作区
```

以及：
```
* main xxxxxx [origin/main] ...
origin  https://github.com/jasonlee0617/S622_robotarm_src.git (fetch)
origin  https://github.com/jasonlee0617/S622_robotarm_src.git (push)
```

