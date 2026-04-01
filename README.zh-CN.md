# Agent Teams Creator

<p align="center">
  <img src="assets/icon-large.svg" alt="Agent Teams Creator icon" width="140" />
</p>

`agent-teams-creator` 是一个面向 Codex 的技能，专门用于分析、设计和实现 Claude Code 风格的 agent teams 运行时。

它最核心的价值，是把很多人说不清的 “multi-agent / swarm” 机制拆成真正可落地的三层：

- `task board = 状态`
- `mailbox = 传输`
- `coordinator / team lead = 控制平面`

## 它解决什么问题

很多多 agent 设计失败，不是因为模型不够强，而是因为把这些东西混在一起了：

- 谁拥有共享状态
- 谁负责消息传递
- 谁拥有审批和协调权

这个技能会强制把分析或设计输出收束到：

- team bootstrap
- teammate identity
- spawn backends
- mailbox protocol
- shared task board
- coordinator role
- isolation stance
- verifier flow

## 压测后的实际效果

基于公开 Claude Code 重建源码做对比测试时：

- 不使用 skill：结果更容易停留在“我看了哪些文件”或泛泛 swarm 描述。
- 使用 `$agent-teams-creator`：结果会稳定讲清 coordination spine、协议消息类型、task board 模型、spawn modes、隔离策略和 verification 规则。

## 安装方式

### Windows

```powershell
git clone https://github.com/Arthurescc/agent-teams-creator `
  "$env:USERPROFILE\\.codex\\skills\\agent-teams-creator"
```

### macOS / Linux

```bash
git clone https://github.com/Arthurescc/agent-teams-creator \
  "${HOME}/.codex/skills/agent-teams-creator"
```

如果你使用 `CODEX_HOME`，请安装到：

```text
$CODEX_HOME/skills/agent-teams-creator
```

安装后重启 Codex，让技能列表刷新。

## 使用示例

- `Use $agent-teams-creator to explain Claude Code's agent teams runtime.`
- `Use $agent-teams-creator to design a coordinator-plus-workers system for this repo.`
- `Use $agent-teams-creator to compare our current swarm design against Claude Code-style team mechanics.`

## 仓库内容

```text
SKILL.md
agents/openai.yaml
references/
assets/
```

## 来源说明

本技能参考了公开的非官方分析项目：

- [oboard/claude-code-rev](https://github.com/oboard/claude-code-rev)
- [sanbuphy/claude-code-source-code](https://github.com/sanbuphy/claude-code-source-code)

与 Anthropic 无官方关联。

## License

MIT
