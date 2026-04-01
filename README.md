# Agent Teams Creator

<p align="center">
  <img src="assets/icon-large.svg" alt="Agent Teams Creator icon" width="140" />
</p>

<p align="center">
  Turn multi-agent "swarm" ideas into an explicit team runtime with task state, mailbox protocols, and verification boundaries.
</p>

## What It Is

`agent-teams-creator` is a Codex skill for analyzing, designing, and implementing Claude Code-style agent team runtimes.

It helps with requests like:

- "How do Claude Code teams actually work?"
- "Design a coordinator + workers system for this project."
- "Add a shared task board and structured inter-agent messaging."
- "Make our multi-agent runtime safer and easier to reason about."

## Why It Matters

Most agent-team designs go wrong because they confuse three different things:

- shared state
- transport
- role authority

This skill forces those apart.

Its central model is:

- `task board = state`
- `mailbox = transport`
- `team lead / coordinator = control plane`

That distinction makes the runtime much easier to build, test, and debug.

## What You Get

- A source-backed explanation of Claude Code's agent teams mechanism.
- A clean model of team bootstrap, spawn modes, and teammate identity.
- A protocol view of inter-agent messaging.
- A task-board-first coordination model.
- Clear guidance on default isolation vs optional worktree isolation.
- Strong verification rules so implementation workers do not self-certify success.

## Key Capabilities

- Team file and deterministic identity design.
- Shared task board architecture.
- Structured mailbox message catalog.
- Spawn backend model: in-process, pane-based, and fallback execution.
- Coordinator/lead behavior model.
- Isolation and cleanup guidance.
- Independent verifier path.

## Validated Value

This skill was pressure-tested against public Claude Code reconstruction sources.

Observed difference:

| Scenario | Without the skill | With the skill |
| --- | --- | --- |
| Explain and rebuild Claude Code-style agent teams | The agent often stopped at "I inspected these files" or broad swarm commentary. | The agent consistently described the coordination spine, protocol message types, task-board model, spawn backends, isolation stance, and verification rules. |

That means the skill materially improves answer shape, not just wording.

## Before / After

### Without `agent-teams-creator`

- answers can stall at file-reading summaries
- "multi-agent" gets described too loosely
- the difference between task board, mailbox, and coordinator is often blurred
- worktree isolation is easy to overstate or misstate

### With `agent-teams-creator`

- the answer is forced toward the real team runtime
- the task board is identified as shared state
- the mailbox is identified as transport and protocol bus
- the lead is identified as control-plane authority
- default shared-workspace collaboration is separated from optional worktree isolation
- verification is treated as an independent phase, not worker self-certification

## Case Study

### Case: "Explain Claude Code's agent teams runtime and how to build one"

**Baseline behavior**
- one baseline run stopped at "I inspected these files"
- another broad answer still risked collapsing protocol details into generic swarm language

**With the skill**
- the answer explicitly covered:
  - coordination spine
  - structured message types
  - spawn modes
  - teammate identity
  - task-board ownership/blockers
  - coordinator role
  - isolation stance
  - verification stance
  - source-backed vs inferred behavior

### Why that matters

That moves the output from "interesting analysis" to "something you can actually build from."

## Quick Start

Clone or copy this repository into your Codex skills directory:

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

If you use `CODEX_HOME`, install it under:

```text
$CODEX_HOME/skills/agent-teams-creator
```

Restart Codex after installation so the skill list refreshes.

## Example Prompts

- `Use $agent-teams-creator to explain Claude Code's agent teams runtime.`
- `Use $agent-teams-creator to design a coordinator-plus-workers system for this repo.`
- `Use $agent-teams-creator to add a task board and mailbox protocol to our multi-agent runtime.`
- `Use $agent-teams-creator to compare our current swarm design against Claude Code-style team mechanics.`

## What It Covers

- team bootstrap
- teammate identity
- spawn modes
- mailbox protocol
- task board model
- coordinator role
- isolation stance
- verifier flow
- facts vs inferred/gated behavior

## Repository Layout

```text
SKILL.md
agents/openai.yaml
references/
  agent-teams-mechanism.md
  agent-teams-workflow.md
assets/
  icon-small.svg
  icon-large.svg
```

## Source Basis

This skill is informed by public, non-official analysis of:

- [oboard/claude-code-rev](https://github.com/oboard/claude-code-rev)
- [sanbuphy/claude-code-source-code](https://github.com/sanbuphy/claude-code-source-code)

It is not affiliated with Anthropic.

## Who This Is For

- agent-platform engineers
- developer tooling teams
- researchers building multi-agent coding systems
- teams adding verifier or reviewer workers
- anyone who wants a protocol-driven alternative to vague swarm prompting

## Chinese Documentation

See [README.zh-CN.md](README.zh-CN.md).

## More Skills

Browse the collection page: [codex-skills-hub](https://github.com/Arthurescc/codex-skills-hub)

## License

MIT
