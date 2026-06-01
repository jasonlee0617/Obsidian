# 1.Git上传步骤（命令行）

## 1.1进入工作区src

```
cd ~/工作区根目录/src
```

## 1.2配置 Git 用户名和邮箱

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

## 1.3初始化 Git 仓库

在 `src` 目录下执行：
```
git init
```

然后把默认分支改成 `main`：
```
git branch -M main
```

## 1.4创建 `.gitignore` 文件

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

## 1.5查看当前 Git 状态

```
git status
```
应该会看到很多红色文件，表示它们还没有被 Git 跟踪

## 1.6添加所有文件到暂存区

```
git add .
```

再次查看状态：
```
git status
```
这时文件应该变成绿色，表示已经准备提交

## 1.7提交代码

```
git commit -m "Initial commit: add ROS2 Humble S622 robot arm source packages"
```

如果成功，会看到类似：
```
[main xxxxxxx] Initial commit: add ROS2 Humble S622 robot arm source packages
```

## 1.8添加 GitHub 远程仓库

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

## 1.9上传到 GitHub

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


## 1.10冲突文件解决



```
git pull --rebase origin main
```