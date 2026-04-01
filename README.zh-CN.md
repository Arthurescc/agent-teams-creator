# Agent Teams Creator

<p align="center">
  <img src="assets/icon-large.svg" alt="Agent Teams Creator icon" width="140" />
</p>

`agent-teams-creator` 是一个面向 Codex 的技能，专门用于分析、设计和实现结构清晰的 agent team runtime。

它最核心的价值，是把很多人说不清的 “multi-agent / swarm” 机制拆成真正可落地的三层：

- `task board = 状态`
- `mailbox = 传输`
- `coordinator / team lead = 控制平面`

## 它解决什么问题

很多多 agent 设计失败，不是因为模型不够强，而是因为把这些东西混在一起了：

- 谁拥有共享状态
- 谁负责消息传递
- 谁拥有协调和审批权

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

基于真实 Codex 工作流做对比测试时：

- 不使用 skill：结果更容易停留在“我看了哪些文件”或泛化 swarm 描述。
- 使用 `$agent-teams-creator`：结果会稳定讲清 coordination spine、协议消息类型、task board 模型、spawn modes、隔离策略和 verification 规则。

## 更直观的前后差异

### 不使用 skill

- 输出可能停留在阅读过程汇报
- “multi-agent” 很容易被讲成泛化 swarm
- task board、mailbox、coordinator 三者的边界容易模糊
- worktree 隔离也容易被说成默认行为

### 使用 `$agent-teams-creator`

- 输出会被强制拉回真正的 team runtime 机制
- task board 会被明确为共享状态
- mailbox 会被明确为传输和协议总线
- team lead / coordinator 会被明确为控制平面
- 默认共享工作区与可选 worktree 隔离会被分开说明
- verification 会被明确成独立阶段，而不是实现 worker 自我宣布完成

## 案例

### 场景：解释一个 agent teams 运行时，并设计一个类似系统

**baseline**
- 有的结果只停在“我读了这些文件”
- 有的结果虽然能回答，但仍然容易把关键协议细节讲得太泛

**使用 skill 后**
- 会稳定覆盖：
  - coordination spine
  - 结构化消息类型
  - spawn modes
  - teammate identity
  - task board 的 owner / blocker 模型
  - coordinator 角色
  - isolation stance
  - verification stance
  - 事实与推断边界

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

- `Use $agent-teams-creator to explain how a protocol-driven agent team runtime should work.`
- `Use $agent-teams-creator to design a coordinator-plus-workers system for this repo.`
- `Use $agent-teams-creator to add a task board and mailbox protocol to our multi-agent runtime.`
- `Use $agent-teams-creator to compare our current swarm design against a stronger team-runtime model.`

## 仓库内容

```text
SKILL.md
agents/openai.yaml
references/
assets/
```

## 开发说明

本项目为自研开发，面向实际 Codex 技能工作流使用。

重点在于让多 agent 运行时的设计更清晰、更协议化、更适合工程实现。

## 更多技能

总导航页见：[codex-skills-hub](https://github.com/Arthurescc/codex-skills-hub)

## License

MIT
