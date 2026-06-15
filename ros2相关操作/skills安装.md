# Codex Skills 安装与使用说明

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

## 3. 本次实际环境

### 3.1 本地已克隆仓库位置

```bash
/home/robot/S622_robotarm/src/skills
```

### 3.2 该仓库中真正的 Skill 目录

```bash
/home/robot/S622_robotarm/src/skills/skills
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

### 3.3 本机 Codex 的用户 Skill 目录

本次实际安装位置是：

```bash
~/.codex/skills
```

说明：

- 当前这台机器上的 Codex 内置安装逻辑与实际环境都使用 `~/.codex/skills`
- 如果后续 Codex 版本对用户 skill 目录约定发生变化，应以当时的官方文档和本机实测结果为准

---

## 4. 推荐安装方式：软链接安装

### 4.1 为什么推荐软链接

推荐原因：

- 不需要重复复制仓库内容
- 后续更新 `skills` 仓库时，Codex 直接读取最新内容
- 方便删除和维护

适用前提：

- `cloudflare/skills` 仓库路径长期稳定

如果未来该仓库可能被移动、重命名或删除，则可以改用复制安装。

### 4.2 全局安装和仓库安装的区别

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

## 5. 安装步骤

### 5.1 创建 Codex 的用户 Skill 目录

```bash
mkdir -p ~/.codex/skills
```

### 5.2 批量把本地 Cloudflare skills 软链接到 Codex

```bash
for d in /home/robot/S622_robotarm/src/skills/skills/*; do
  name=$(basename "$d")
  target="$HOME/.codex/skills/$name"
  if [ -e "$target" ]; then
    echo "skip $name: $target already exists"
  else
    ln -s "$d" "$target"
  fi
done
```

### 5.3 本次实际安装结果

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

### 5.4 核心最小集合

如果只想先装最常用的几个，可以只保留：

- `cloudflare`
- `wrangler`
- `durable-objects`
- `agents-sdk`

---

## 6. 安装后验证

### 6.1 先在终端检查链接是否存在

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

### 6.2 重启 Codex

Skill 安装完成后，建议：

- 重启 Codex
- 或至少新开一个线程

原因：

- 有些技能列表是在会话启动时扫描的
- 不重启时，当前线程未必立即刷新

### 6.3 在 Codex 中验证

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

- 帮我写一个 `wrangler.toml`
- 给我一个 Durable Object 示例
- 帮我搭一个 Cloudflare Worker 项目

---

## 7. 更新方式

因为本次采用的是软链接，所以更新非常简单。

### 7.1 更新 skills 仓库

进入仓库后拉最新内容：

```bash
cd /home/robot/S622_robotarm/src/skills
git pull
```

### 7.2 重新打开 Codex

```text
重启 Codex 或新开线程
```

说明：

- 由于 `~/.codex/skills/<skill-name>` 指向的是原仓库目录
- 仓库内容更新后，Codex 看到的就是新版本

---

## 8. 卸载方式

### 8.1 删除单个 Skill

例如删除 `cloudflare`：

```bash
rm ~/.codex/skills/cloudflare
```

### 8.2 删除全部本次安装的 Cloudflare Skills

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
- 不会删除原始仓库 `/home/robot/S622_robotarm/src/skills`

---

## 9. 如果不想用软链接，也可以复制安装

适合场景：

- 原仓库路径可能会变
- 想让 Codex 独立持有一份 skill 文件

示例：

```bash
cp -R /home/robot/S622_robotarm/src/skills/skills/cloudflare ~/.codex/skills/
```

批量复制：

```bash
cp -R /home/robot/S622_robotarm/src/skills/skills/* ~/.codex/skills/
```

缺点：

- 后续仓库更新后不会自动同步
- 需要手工重新复制

---

## 10. 常见问题

### 10.1 安装后 `/skills` 看不到新技能

排查顺序：

1. 先检查 `~/.codex/skills/<skill-name>/SKILL.md` 是否存在
2. 确认是否重启了 Codex 或新开了线程
3. 确认软链接目标目录没有被移动
4. 确认 `SKILL.md` 文件名和目录结构正确

### 10.2 我已经安装了 skill，为什么还不能使用 Cloudflare 的在线文档或 API

原因：

- skill 只提供“说明”和“工作流”
- 不会自动启用仓库里的 MCP 配置

如果要让 Codex 直接连接 Cloudflare 的 MCP 服务，还需要单独配置：

- `cloudflare-api`
- `cloudflare-docs`
- `cloudflare-bindings`
- `cloudflare-builds`
- `cloudflare-observability`

### 10.3 Skill 和 Plugin 到底是不是一回事

不是一回事。

简单理解：

- Skill：任务说明书
- Plugin：打包发布容器

当前 `cloudflare/skills` 仓库本身更接近“通用 skills 仓库”，不是一个可直接被 Codex 当作完整本地插件安装的现成 `.codex-plugin` 项目。

### 10.4 什么时候应该做成 Plugin

适合以下情况：

- 要分享给别人用
- 想把 skills、MCP、命令、资源打包在一起
- 想通过 marketplace 安装

如果只是自己本地使用，直接装 skills 往往就够了。

---

## 11. 本次操作总结

本次已经完成的是：

- 找到本地 `cloudflare/skills` 仓库
- 确认真正的技能目录在 `/home/robot/S622_robotarm/src/skills/skills`
- 将全部 skills 软链接安装到 `~/.codex/skills`
- 校验核心 skill 的 `SKILL.md` 可正常访问

本次没有完成的是：

- 没有把该仓库做成 Codex plugin
- 没有把 `.mcp.json` 写入 Codex 的 MCP 配置

---

## 12. 后续可继续扩展的方向

如果后面要继续完善，可以做下面两件事：

### 12.1 接入 Cloudflare MCP

把仓库中的 `.mcp.json` 内容转成 Codex 可识别的 MCP 配置，写入：

```bash
~/.codex/config.toml
```

### 12.2 做成本地 Codex Plugin

补齐：

- `.codex-plugin/plugin.json`
- 本地 marketplace 配置

这样就可以通过 Codex 的插件目录管理，而不是手动维护 skill 目录。
