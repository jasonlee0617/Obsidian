# Codex Skills 安装、理解与使用入门

本文档用于整理本机 Codex Skills 的安装方式、概念区别、Cloudflare skills 的能力边界，以及后续维护方法。

目标不是只记录一次命令，而是把这套东西整理成一篇适合后续反复查阅的入门文档。

---

## 1. 什么是 Skill

`Skill` 可以理解为给 Codex 增加的一组“可复用任务说明书”。

它通常包含：

- `SKILL.md`：核心说明文件
- `scripts/`：可选脚本
- `references/`：可选参考资料
- 其他与该技能相关的资源文件

当用户的问题命中了 Skill 的描述范围时，Codex 会主动加载这个 Skill，或者也可以通过显式方式手动调用。

例如：

```text
$cloudflare 帮我写一个最小 Cloudflare Worker
```

简单理解：

- 普通对话：Codex 按通用能力回答
- 命中 skill：Codex 进入某一类任务的专用工作流

---

## 2. Skill、Plugin、MCP 的区别

### 2.1 Skill

- 本质是“说明 + 资源”
- 用来教 Codex 如何处理某类任务
- 适合本地快速接入

### 2.2 Plugin

- 是 Codex 的“分发单元”
- 可以把多个 skills、MCP 配置、App 集成打包在一起
- 适合分享给别人或长期维护

### 2.3 MCP

- 是 Model Context Protocol
- 作用是给 Codex 接入外部工具、文档、服务
- 例如远程文档查询、平台 API、浏览器控制等

### 2.4 本次采用的方式

本次只做：

- 安装 `cloudflare/skills` 到 Codex

本次不做：

- Plugin 化
- 接入 `cloudflare` 的 `.mcp.json`

注意：

`cloudflare/skills` 仓库中的 `.mcp.json` 不会因为复制了 skills 就自动在 Codex 中生效，MCP 需要单独配置到 Codex 的配置文件中。

---

## 3. Skills 到底能带来什么

安装 skill 之后，Codex 不只是“知道一个名字”，而是会得到一套更稳定的任务处理方式，包括：

- 遇到某类任务时优先读取哪类文档
- 哪些命令应该先检查
- 哪些配置项是必须的
- 哪些做法是推荐路径
- 哪些是常见坑和反模式
- 在复杂任务里应该先问什么、先验证什么

例如：

- 没装 `wrangler` skill 时，Codex 可能只会泛泛地说怎么部署 Worker
- 装了 `wrangler` skill 后，Codex 会优先按 Cloudflare 的推荐方式处理 `wrangler.jsonc`、`wrangler deploy`、`wrangler types`、bindings、环境切换等细节

所以 skill 的价值不只是“会不会”，而是“会不会以正确流程做”。

---

## 4. 本次实际环境

### 4.1 本地已克隆仓库位置

```bash
/home/robot/skills
```

补充说明：

- 该仓库最初放在 `/home/robot/S622_robotarm/src/skills`
- 后续迁移到了 `/home/robot/skills`
- 由于本次安装方式使用的是软链接，所以仓库路径变化后，需要同步修复 `~/.codex/skills` 下的链接目标

### 4.2 该仓库中真正的 Skill 目录

```bash
/home/robot/skills/skills
```

该目录下目前包含：

- `agents-sdk`
- `cloudflare`
- `cloudflare-email-service`
- `cloudflare-one`
- `cloudflare-one-migrations`
- `durable-objects`
- `sandbox-sdk`
- `turnstile-spin`
- `web-perf`
- `workers-best-practices`
- `wrangler`

### 4.3 本机 Codex 的用户 Skill 目录

本次实际安装位置是：

```bash
~/.codex/skills
```

说明：

- 当前这台机器上的 Codex 内置安装逻辑与实际环境都使用 `~/.codex/skills`
- 如果后续 Codex 版本对用户 skill 目录约定发生变化，应以当时的官方文档和本机实测结果为准

---

## 5. 推荐安装方式：软链接安装

### 5.1 为什么推荐软链接

推荐原因：

- 不需要重复复制仓库内容
- 后续更新 `skills` 仓库时，Codex 直接读取最新内容
- 方便删除和维护

适用前提：

- `cloudflare/skills` 仓库路径长期稳定

如果未来该仓库可能被移动、重命名或删除，则可以改用复制安装。

### 5.2 全局安装和仓库安装的区别

#### 全局安装

安装到：

```bash
~/.codex/skills
```

特点：

- 当前用户下的多个 Codex 工作区都能使用
- 适合个人长期使用
- 本次就是采用这种方式

#### 仓库安装

常见做法是把 skill 放到仓库内的：

```bash
.agents/skills
```

特点：

- 只对当前仓库或当前项目生效
- 适合团队共享、跟仓库一起版本管理
- 适合某个项目专用的开发流程

简单理解：

- 想给“自己所有项目”都用：装到用户目录
- 想给“某一个仓库”专用：装到仓库目录

---

## 6. 安装步骤

### 6.1 创建 Codex 的用户 Skill 目录

```bash
mkdir -p ~/.codex/skills
```

### 6.2 批量把本地 Cloudflare skills 软链接到 Codex

```bash
for d in /home/robot/skills/skills/*; do
  name=$(basename "$d")
  target="$HOME/.codex/skills/$name"
  if [ -e "$target" ]; then
    echo "skip $name: $target already exists"
  else
    ln -s "$d" "$target"
  fi
done
```

### 6.3 本次实际安装结果

已成功链接：

- `agents-sdk`
- `cloudflare`
- `cloudflare-email-service`
- `cloudflare-one`
- `cloudflare-one-migrations`
- `durable-objects`
- `sandbox-sdk`
- `turnstile-spin`
- `web-perf`
- `workers-best-practices`
- `wrangler`

### 6.4 核心最小集合

如果只想先装最常用的几个，可以只保留：

- `cloudflare`
- `wrangler`
- `durable-objects`
- `agents-sdk`

---

## 7. 这些新安装的 skills 都能做什么

下面的介绍不是单纯翻译名称，而是从“你实际遇到什么任务时该用它”来说明。

### 7.1 `cloudflare`

定位：

- Cloudflare 平台总入口 skill

适合任务：

- Workers
- Pages
- KV / D1 / R2 / Queues / Vectorize
- Workers AI
- Tunnel / 网络能力
- 安全类服务

它的作用：

- 先判断你应该用 Cloudflare 的哪个产品
- 再把任务导向更细的 skill 或对应工作流

适合示例：

```text
$cloudflare 帮我做一个 Cloudflare Worker + D1 的最小项目
$cloudflare 我该用 KV 还是 R2 存这类数据
```

### 7.2 `wrangler`

定位：

- Cloudflare Workers CLI 专项 skill

适合任务：

- 初始化 Worker 项目
- 本地开发 `wrangler dev`
- 部署 `wrangler deploy`
- 编写和检查 `wrangler.jsonc`
- 添加各类 bindings
- 配置环境与类型生成

它的作用：

- 优先按 Cloudflare 官方推荐方式组织 CLI 命令和配置
- 避免手写错误的配置字段

适合示例：

```text
$wrangler 帮我初始化一个 Worker 项目并配置 D1
$wrangler 检查这个 wrangler.jsonc 有没有问题
```

### 7.3 `durable-objects`

定位：

- Cloudflare 强状态协调组件 skill

适合任务：

- 聊天室
- 实时协作
- 游戏房间
- 库存 / 预订 / 锁资源
- WebSocket 持久连接
- alarms 定时调度

它的作用：

- 帮你正确设计 DO 粒度
- 提醒迁移、存储、并发和初始化模式

适合示例：

```text
$durable-objects 帮我设计一个聊天室 Durable Object
$durable-objects review 一下这段 DO 代码
```

### 7.4 `agents-sdk`

定位：

- Cloudflare 上做 AI agents 的核心 skill

适合任务：

- 有状态 agent
- WebSocket 实时 agent
- callable RPC
- schedule / cron
- durable workflow
- MCP server / MCP client
- 聊天 agent
- 邮件型 agent

它的作用：

- 指导如何在 Workers 上构建带状态、可调度、可扩展的 agent 系统

适合示例：

```text
$agents-sdk 帮我做一个有持久状态的聊天 agent
$agents-sdk 帮我写一个支持 scheduleEvery 的巡检 agent
```

### 7.5 `sandbox-sdk`

定位：

- Cloudflare 安全沙箱执行环境 skill

适合任务：

- 在线代码解释器
- 执行不可信代码
- AI code interpreter
- 隔离的脚本执行环境
- 带文件系统的临时运行容器

它的作用：

- 指导如何用 Cloudflare Sandbox SDK 执行命令、写文件、开放端口、维护会话上下文

适合示例：

```text
$sandbox-sdk 帮我做一个 Python 代码执行沙箱
$sandbox-sdk 帮我把这个 AI code interpreter 跑起来
```

### 7.6 `cloudflare-email-service`

定位：

- Cloudflare 邮件发送与接收专项 skill

适合任务：

- Worker 发事务邮件
- Email Routing
- 收邮件并处理
- 通过 REST API 发邮件
- agent 处理邮件
- 可投递性、SPF、DKIM、DMARC

它的作用：

- 把邮件功能接入过程中那些容易漏掉的前置配置补齐

适合示例：

```text
$cloudflare-email-service 给我的 Worker 加验证码邮件
$cloudflare-email-service 帮我接收用户回复邮件并交给 agent 处理
```

### 7.7 `turnstile-spin`

定位：

- Cloudflare Turnstile 一条龙接入 skill

适合任务：

- 注册表单防机器人
- 登录表单防滥用
- 联系表单防垃圾提交
- 从 reCAPTCHA / hCaptcha 迁移到 Turnstile

它的作用：

- 扫描代码库
- 创建 Turnstile widget
- 部署 siteverify Worker
- 改前端提交逻辑
- 做完整验证

适合示例：

```text
$turnstile-spin 给这个表单接入 Turnstile
$turnstile-spin 把项目里的 reCAPTCHA 换成 Turnstile
```

### 7.8 `workers-best-practices`

定位：

- Cloudflare Worker 代码评审与最佳实践 skill

适合任务：

- review Worker 项目
- 检查配置是否符合现代写法
- 检查绑定、类型、secret、streaming、waitUntil 等问题
- 识别常见反模式

它的作用：

- 不只是说“能跑”，而是帮你识别哪些写法以后会埋坑

适合示例：

```text
$workers-best-practices review 一下这个 Worker 仓库
$workers-best-practices 看看这段代码有没有反模式
```

### 7.9 `cloudflare-one`

定位：

- Cloudflare Zero Trust / SASE / 企业接入 skill

适合任务：

- Access
- Gateway
- WARP
- Tunnel
- DLP
- CASB
- device posture
- Cloudflare WAN

它的作用：

- 用于设计、配置、排障和审查企业级 Zero Trust 架构

适合示例：

```text
$cloudflare-one 帮我设计一个私有应用通过 Access 发布的方案
$cloudflare-one 帮我排查 WARP + Tunnel 的访问问题
```

### 7.10 `cloudflare-one-migrations`

定位：

- Cloudflare One 迁移专项 skill

适合任务：

- 从 Zscaler ZIA / ZPA 迁移
- 从 Palo Alto / Prisma 迁移
- 从传统 VPN / SWG / SASE 迁移到 Cloudflare One

它的作用：

- 帮你先做规则映射、资产梳理、风险识别和迁移计划，而不是直接草率改配置

适合示例：

```text
$cloudflare-one-migrations 帮我规划从 ZPA 迁移到 Cloudflare One
```

### 7.11 `web-perf`

定位：

- Web 性能审计 skill

适合任务：

- Core Web Vitals 分析
- LCP / INP / CLS 问题排查
- 网络请求链路分析
- 页面性能优化建议

它的作用：

- 借助 Chrome DevTools MCP 做性能审计

注意：

- 该 skill 依赖 `chrome-devtools` MCP
- 如果没有配置对应 MCP，它就不能完整发挥作用

适合示例：

```text
$web-perf 帮我分析这个网页为什么 LCP 很差
```

---

## 8. Cloudflare skills 速查表

这一节适合在“我想做什么，但不知道该叫哪个 skill”时快速查。

| 我想做什么 | 应该优先用哪个 skill |
|-----------|----------------------|
| 做一个最小 Cloudflare Worker | `cloudflare` 或 `wrangler` |
| 初始化 Worker 项目、部署、改 wrangler 配置 | `wrangler` |
| 做聊天室、房间状态、库存锁、实时协作 | `durable-objects` |
| 做 AI agent、带状态聊天、可调度 agent | `agents-sdk` |
| 做安全代码执行沙箱 / code interpreter | `sandbox-sdk` |
| 给 Worker 加事务邮件或收邮件 | `cloudflare-email-service` |
| 给表单加 Turnstile / 迁移验证码 | `turnstile-spin` |
| review Worker 项目质量和反模式 | `workers-best-practices` |
| 做 Zero Trust / Access / WARP / Tunnel | `cloudflare-one` |
| 做 Zscaler / Palo Alto 到 Cloudflare One 的迁移规划 | `cloudflare-one-migrations` |
| 做网页性能审计 | `web-perf` |

更口语一点的对应关系：

- “我想做 Cloudflare 相关，但还不知道具体产品” -> `cloudflare`
- “我已经确定是 Worker 项目，现在要真正落地” -> `wrangler`
- “我要做强状态和协调逻辑” -> `durable-objects`
- “我要做 agent，而不是普通 Worker” -> `agents-sdk`
- “我要做代码执行环境” -> `sandbox-sdk`
- “我要做邮件” -> `cloudflare-email-service`
- “我要防机器人” -> `turnstile-spin`
- “我要做代码质量 review” -> `workers-best-practices`

---

## 9. 安装后如何验证

### 9.1 先在终端检查链接是否存在

```bash
ls -l ~/.codex/skills
```

重点检查以下几个文件是否存在：

```bash
~/.codex/skills/cloudflare/SKILL.md
~/.codex/skills/wrangler/SKILL.md
~/.codex/skills/durable-objects/SKILL.md
~/.codex/skills/agents-sdk/SKILL.md
```

也可以逐个检查：

```bash
test -f ~/.codex/skills/cloudflare/SKILL.md && echo ok
test -f ~/.codex/skills/wrangler/SKILL.md && echo ok
test -f ~/.codex/skills/durable-objects/SKILL.md && echo ok
test -f ~/.codex/skills/agents-sdk/SKILL.md && echo ok
```

### 9.2 重启 Codex

Skill 安装完成后，建议：

- 重启 Codex
- 或至少新开一个线程

原因：

- 有些技能列表是在会话启动时扫描的
- 不重启时，当前线程未必立即刷新

### 9.3 在 Codex 中验证

方法 1：查看技能列表

```text
/skills
```

应该可以看到：

- `cloudflare`
- `wrangler`
- `durable-objects`
- `agents-sdk`

方法 2：显式调用

```text
$cloudflare 帮我写一个最小 Cloudflare Worker
```

方法 3：隐式触发

直接输入下面这类问题，观察 Codex 是否自动匹配相关 skill：

- 帮我写一个 `wrangler.jsonc`
- 给我一个 Durable Object 示例
- 帮我搭一个 Cloudflare Worker 项目

---

## 10. 日常使用建议

对于 Cloudflare 相关任务，推荐按下面的思路使用：

### 10.1 不确定产品怎么选

优先叫：

```text
$cloudflare
```

### 10.2 已经知道自己要做 Worker

优先叫：

```text
$wrangler
```

### 10.3 已经知道要做 DO、Agent、邮件、验证码

直接精确调用对应 skill：

- `$durable-objects`
- `$agents-sdk`
- `$cloudflare-email-service`
- `$turnstile-spin`

### 10.4 想让 Codex 帮你做代码评审

优先叫：

```text
$workers-best-practices review 一下这个仓库
```

这样 Codex 会更容易进入正确的审查模式。

---

## 11. 更新方式

因为本次采用的是软链接，所以更新非常简单。

### 11.1 更新 skills 仓库

进入仓库后拉最新内容：

```bash
cd /home/robot/skills
git pull
```

### 11.2 重新打开 Codex

```text
重启 Codex 或新开线程
```

说明：

- 由于 `~/.codex/skills/<skill-name>` 指向的是原仓库目录
- 仓库内容更新后，Codex 看到的就是新版本

---

## 12. 仓库迁移后的影响与修复

### 12.1 仓库迁移会不会影响 Skill 使用

会，前提是你使用的是软链接安装，并且你做的是“移动”而不是“复制”。

原因：

- `~/.codex/skills/<skill-name>` 本质上是一个软链接
- 它会直接指向 skills 仓库中的真实目录
- 如果真实目录从旧路径移动到新路径，原软链接就会失效

例如：

```bash
~/.codex/skills/cloudflare -> /旧路径/skills/skills/cloudflare
```

当旧路径不存在时，Codex 就无法继续读取该 skill。

### 12.2 什么情况不会受影响

以下情况通常不会影响：

- 你只是复制了一份 skills 仓库到新位置
- 旧路径还保留着
- `~/.codex/skills` 仍然能指向原目录

### 12.3 如何检查链接是否失效

```bash
for p in ~/.codex/skills/*; do
  [ -L "$p" ] || continue
  if [ -e "$p" ]; then
    echo "ok $(basename "$p")"
  else
    echo "broken $(basename "$p") -> $(readlink "$p")"
  fi
done
```

### 12.4 本次实际修复方法

本次 `skills` 仓库迁移到：

```bash
/home/robot/skills
```

因此需要把 `~/.codex/skills` 下的 Cloudflare skills 全部重建为新路径：

```bash
for d in /home/robot/skills/skills/*; do
  name=$(basename "$d")
  target="$HOME/.codex/skills/$name"
  rm -f "$target"
  ln -s "$d" "$target"
done
```

### 12.5 修复后的验证

```bash
test -f ~/.codex/skills/cloudflare/SKILL.md && echo ok
test -f ~/.codex/skills/wrangler/SKILL.md && echo ok
test -f ~/.codex/skills/durable-objects/SKILL.md && echo ok
test -f ~/.codex/skills/agents-sdk/SKILL.md && echo ok
```

之后再：

- 重启 Codex
- 或新开一个线程

---

## 13. 卸载方式

### 13.1 删除单个 Skill

例如删除 `cloudflare`：

```bash
rm ~/.codex/skills/cloudflare
```

### 13.2 删除全部本次安装的 Cloudflare Skills

```bash
rm ~/.codex/skills/agents-sdk
rm ~/.codex/skills/cloudflare
rm ~/.codex/skills/cloudflare-email-service
rm ~/.codex/skills/cloudflare-one
rm ~/.codex/skills/cloudflare-one-migrations
rm ~/.codex/skills/durable-objects
rm ~/.codex/skills/sandbox-sdk
rm ~/.codex/skills/turnstile-spin
rm ~/.codex/skills/web-perf
rm ~/.codex/skills/workers-best-practices
rm ~/.codex/skills/wrangler
```

说明：

- 删除的是软链接
- 不会删除原始仓库 `/home/robot/skills`

---

## 14. 如果不想用软链接，也可以复制安装

适合场景：

- 原仓库路径可能会变
- 想让 Codex 独立持有一份 skill 文件

示例：

```bash
cp -R /home/robot/skills/skills/cloudflare ~/.codex/skills/
```

批量复制：

```bash
cp -R /home/robot/skills/skills/* ~/.codex/skills/
```

缺点：

- 后续仓库更新后不会自动同步
- 需要手工重新复制

---

## 15. 常见问题

### 15.1 安装后 `/skills` 看不到新技能

排查顺序：

1. 先检查 `~/.codex/skills/<skill-name>/SKILL.md` 是否存在
2. 确认是否重启了 Codex 或新开了线程
3. 确认软链接目标目录没有被移动
4. 确认 `SKILL.md` 文件名和目录结构正确

### 15.2 skills 仓库迁移后突然失效

大概率是软链接仍然指向旧路径。

处理方式：

1. 先检查 `~/.codex/skills` 下的链接是否 broken
2. 确认新的仓库目录
3. 批量删除旧链接并重新建立到新路径
4. 重启 Codex 或新开线程

### 15.3 我已经安装了 skill，为什么还不能使用 Cloudflare 的在线文档或 API

原因：

- skill 只提供“说明”和“工作流”
- 不会自动启用仓库里的 MCP 配置

如果要让 Codex 直接连接 Cloudflare 的 MCP 服务，还需要单独配置：

- `cloudflare-api`
- `cloudflare-docs`
- `cloudflare-bindings`
- `cloudflare-builds`
- `cloudflare-observability`

### 15.4 Skill 和 Plugin 到底是不是一回事

不是一回事。

简单理解：

- Skill：任务说明书
- Plugin：打包发布容器

当前 `cloudflare/skills` 仓库本身更接近“通用 skills 仓库”，不是一个可直接被 Codex 当作完整本地插件安装的现成 `.codex-plugin` 项目。

### 15.5 什么时候应该做成 Plugin

适合以下情况：

- 要分享给别人用
- 想把 skills、MCP、命令、资源打包在一起
- 想通过 marketplace 安装

如果只是自己本地使用，直接装 skills 往往就够了。

---

## 16. 本次操作总结

本次已经完成的是：

- 找到本地 `cloudflare/skills` 仓库
- 确认真正的技能目录当前在 `/home/robot/skills/skills`
- 将全部 skills 软链接安装到 `~/.codex/skills`
- 在仓库迁移后，重新修复了 `~/.codex/skills` 的软链接目标
- 校验核心 skill 的 `SKILL.md` 可正常访问

本次没有完成的是：

- 没有把该仓库做成 Codex plugin
- 没有把 `.mcp.json` 写入 Codex 的 MCP 配置

---

## 17. 后续可继续扩展的方向

如果后面要继续完善，可以做下面两件事：

### 17.1 接入 Cloudflare MCP

把仓库中的 `.mcp.json` 内容转成 Codex 可识别的 MCP 配置，写入：

```bash
~/.codex/config.toml
```

### 17.2 做成本地 Codex Plugin

补齐：

- `.codex-plugin/plugin.json`
- 本地 marketplace 配置

这样就可以通过 Codex 的插件目录管理，而不是手动维护 skill 目录。
