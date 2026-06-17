
# 1.linux系统
Codex 官方文档说明：Skill 是一个包含 `SKILL.md` 的目录，`SKILL.md` 需要有 `name` 和 `description`；Codex IDE Extension 也支持 Skills；用户级 Skills 位置是 `$HOME/.agents/skills`，项目级位置是 `.agents/skills`

创建~/.agents/skills
这是当前 Codex 用来扫描个人 Skills 的标准目录：
```
~/.agents/skills
```

## 1.1andrej-karpathy-skills项目安装

### 1. 克隆仓库

在 Linux 终端执行：
```
cd ~/my-skills
git clone https://github.com/multica-ai/andrej-karpathy-skills.git
```

### 2. 创建 Codex 用户级 Skills 目录

```
mkdir -p ~/.agents/skills
```

### 3. 复制这个 Skill 到 Codex

```
cp -r ~/my-skills/andrej-karpathy-skills/skills/karpathy-guidelines ~/.agents/skills/
```

最终目录应该是：
```
~/.agents/skills/karpathy-guidelines/SKILL.md
```

检查一下：
```
ls ~/.agents/skills/karpathy-guidelines
head -20 ~/.agents/skills/karpathy-guidelines/SKILL.md
```

### 4. 重启 VS Code / Codex 会话

安装后，建议：
```
# 完全关闭 VS Code 后重新打开
code .
```

### 5.确保源 skill 目录确实存在

建立个人的skills仓库，统一管理各种skills，

## 1.2 M4cs/compressoor（自动省上下文-Codex skill + plugin）

```
cd ~/my-skills
git clone https://github.com/M4cs/compressoor.git
cd compressoor
python3 skills/compressoor/scripts/install_codex_compressoor.py --force
```

### 1.3 Agent-Skills-for-Context-Engineering(上下文压缩方法论 skill)

适合你让 Codex 在长任务前后做这种事：
```
$context-compression

请把当前会话压缩成可继续工作的 handoff，总结：
1. 当前目标
2. 已修改文件
3. 已验证命令和结果
4. 未解决问题
5. 下一步最小操作
```

安装到 Codex 可以这样：
```
cd ~/my-skills
git clone https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering.git context-engineering-skills

##已经创建可以不执行
mkdir -p ~/.agents/skills
##已经创建可以不执行

ln -sfnT \
  ~/my-skills/context-engineering-skills/skills/context-compression \
  ~/.agents/skills/context-compression

head -20 ~/.agents/skills/context-compression/SKILL.md
```