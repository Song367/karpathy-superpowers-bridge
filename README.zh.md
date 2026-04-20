# karpathy-superpowers-bridge

一个仅面向 **Codex** 的 skill，用来把 **Superpowers** 的流程纪律和 **Karpathy 风格** 的克制原则结合起来。

这个仓库目前只发布一个 Codex bridge skill，适合这样的使用方式：

- 用 Superpowers 保留强流程和硬门禁
- 用 Karpathy 风格约束计划、评审和实现不要过重
- 用一个自包含 skill 取代额外安装独立 Karpathy skill 的复杂度

English version: [README.md](./README.md)

## 这个 Skill 是做什么的

`karpathy-superpowers-bridge` 不是用来替代 Superpowers 的。

它做的是一层协调：

- **Superpowers** 继续负责工作流
  - 先设计，再实现
  - 先找到根因，再修 bug
  - 先有失败测试，再写生产代码
  - 先有新鲜验证证据，再宣称完成
- **Karpathy** 继续负责克制和收敛
  - 明确说出假设
  - 优先最简单方案
  - 保持外科手术式 diff
  - 用可验证目标定义成功

这个 bridge 的目标，是让这两套系统协同工作，而不是把所有规则粗暴拼成一份臃肿的提示词。

## 设计目标

这个 skill 基于四个核心决定：

1. 用户指令和项目指令仍然优先。
2. 只要 Superpowers 的硬门禁明确命中，就保留它。
3. Karpathy 原则用来约束 Superpowers 在计划、review 和 subagent 执行中的“过重倾向”。
4. 这个 bridge 是自包含的。核心行为不需要再单独安装 Karpathy skill。

## 前置依赖

请先安装 **Superpowers**。

这个 bridge 假设 Superpowers 已经安装并且能被 Codex 发现。没有 Superpowers 时，这个 skill 仍然可以阅读，但它最重要的路由能力会失去价值。

Superpowers 的官方 Codex 文档：

- [Superpowers for Codex](https://github.com/obra/superpowers/blob/main/docs/README.codex.md)
- [官方 Codex 安装说明](https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md)

截至 **2026 年 4 月 20 日**，官方给 Codex 的快速安装入口是：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md
```

如果以后 Superpowers 改了安装方式，请以它的官方文档为准。

## 安装

### 第一步：先安装 Superpowers

先按官方 Superpowers Codex 文档完成安装。

### 第二步：安装这个 Bridge Skill

根据你的 Codex 环境选择合适的 skill 安装目录。

#### 选项 A：安装到 `~/.agents/skills`（推荐给 Codex CLI）

```bash
mkdir -p ~/.agents/skills/karpathy-superpowers-bridge
curl -o ~/.agents/skills/karpathy-superpowers-bridge/SKILL.md \
  https://raw.githubusercontent.com/Song367/karpathy-superpowers-bridge/main/skills/karpathy-superpowers-bridge/SKILL.md
```

#### 选项 B：安装到 `$CODEX_HOME/skills` 或 `~/.codex/skills`（适合通过该路径发现 skills 的 Codex 环境）

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/karpathy-superpowers-bridge"
curl -o "${CODEX_HOME:-$HOME/.codex}/skills/karpathy-superpowers-bridge/SKILL.md" \
  https://raw.githubusercontent.com/Song367/karpathy-superpowers-bridge/main/skills/karpathy-superpowers-bridge/SKILL.md
```

这个路径已经在一套 Codex 桌面环境里实际验证可用，使用的是：

```text
~/.codex/skills/karpathy-superpowers-bridge/
```

### 第三步：重启 Codex

安装完成后，请启动一个新的 Codex 会话，让 skill 被重新发现。

## 另一种安装方式

如果你更喜欢先 clone 仓库，再手动 copy 或 symlink：

```bash
git clone https://github.com/Song367/karpathy-superpowers-bridge.git
```

然后把下面这个目录：

- `skills/karpathy-superpowers-bridge/`

复制或链接到以下任一位置：

- `~/.agents/skills/`
- `${CODEX_HOME:-$HOME/.codex}/skills/`

## 它是如何结合 Superpowers 和 Karpathy 的

这个 bridge 使用下面的优先级模型：

1. **用户和项目指令**
2. **命中的 Superpowers 硬门禁**
3. **当前工作流内部的 Karpathy 约束**
4. 默认助手行为

实际含义是：

- Karpathy 可以让 Superpowers 变得更克制、更轻量。
- Karpathy **不会** 默认取消 Superpowers 已经明确命中的硬门禁。
- 如果某个刚性的 Superpowers skill 明确适用，仍然应进入它的工作流。
- 进入工作流之后，再用 Karpathy 原则去压住过度设计、过度 review 和 scope creep。

## 这个 Bridge 主要补的是什么

这个 bridge 主要约束的是 Superpowers 最容易变重的几个接缝：

- **计划过滤**
  - 不为了未来而做架构
  - 不因为“也许以后需要”就加抽象
  - 没有现实需要时，不拆更多层、不加更多文件
- **Review 过滤**
  - 缺少 extensibility 不自动算缺陷
  - 缺少 configurability 不自动算缺陷
  - scope creep 被当作 bug，而不是优点
- **Subagent 过滤**
  - 不要猜，要提问
  - 只做被指定的任务
  - diff 要保持外科手术式
  - 有疑虑要上报，不要装作确定

## 这个仓库目前不包含什么

当前版本 **只提供 Codex 版本**。

暂时还没有：

- Claude Code 打包版本
- Cursor rules
- 独立的 `CLAUDE.md`
- 单独的 Karpathy plugin

## 仓库结构

```text
README.md
README.zh.md
skills/
  karpathy-superpowers-bridge/
    SKILL.md
```

## 使用方式

安装后，Codex 可以在任务和 skill 描述匹配时自动发现它。

你也可以显式提到它，例如：

```text
Use karpathy-superpowers-bridge for this task.
```

## License

目前仓库里还没有加入 license 文件。如果你准备更广泛公开分发，建议再补一个许可证文件，把复用方式写清楚。
