# Agent Teams Creator

<p align="center">
  <img src="assets/icon-large.svg" alt="Agent Teams Creator icon" width="140" />
</p>

<p align="center">
  Turn multi-agent swarm ideas into an explicit team runtime with task state, mailbox protocols, and verification boundaries.
</p>

## What It Is

`agent-teams-creator` is an open `SKILL.md`-based agent skill for analyzing, designing, and implementing structured agent team runtimes.

It is meant to be reusable across Codex, OpenClaw, Claude Code, Hermes Agent, and other coding agents that can ingest skill-style Markdown instructions. This is not a Codex-only repo.

It helps with requests like:

- "How should an agent team runtime actually work?"
- "Design a coordinator + workers system for this project."
- "Add a shared task board and structured inter-agent messaging."
- "Make our multi-agent runtime safer and easier to reason about."

## Compatibility

This repository uses the portable `SKILL.md` pattern that now shows up across multiple agent ecosystems.

- Works well in: Codex, OpenClaw, Claude Code, Hermes Agent
- Usually portable to: agents that support custom skills, instruction packs, or Markdown playbooks
- Best fit: coordinator-worker systems, protocol-driven swarms, verifier-based teams, shared task boards, and mailbox-driven runtimes

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

- A clean model of team bootstrap, spawn modes, and teammate identity.
- A protocol view of inter-agent messaging.
- A task-board-first coordination model.
- Clear guidance on default isolation vs optional worktree isolation.
- Strong verification rules so implementation workers do not self-certify success.

## What People Usually Need Clarified

Multi-agent repos often sound impressive but stay blurry on the mechanism. This skill is for the moment when you need the README or design answer to become buildable:

- what is shared state vs message transport
- who has authority to assign, approve, or block work
- when isolation is required and when shared workspace is acceptable
- how verifier flows differ from worker flows
- how to describe all this without collapsing it into vague "swarm" language

## Key Capabilities

- Team file and deterministic identity design.
- Shared task board architecture.
- Structured mailbox message catalog.
- Spawn backend model: in-process, pane-based, and fallback execution.
- Coordinator/lead behavior model.
- Isolation and cleanup guidance.
- Independent verifier path.

## Validated Value

This skill was pressure-tested in practical Codex workflow scenarios.

Observed difference:

| Scenario | Without the skill | With the skill |
| --- | --- | --- |
| Explain and rebuild a protocol-driven agent team runtime | The agent often stopped at "I inspected these files" or broad swarm commentary. | The agent consistently described the coordination spine, protocol message types, task-board model, spawn backends, isolation stance, and verification rules. |

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

### Case: "Explain an agent team runtime and how to build one"

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
  - facts vs inferred behavior

### Why that matters

That moves the output from "interesting analysis" to "something you can actually build from."

## Quick Start

Clone or copy this repository into your agent skills directory.

### Common skill locations

```text
Codex:       ~/.codex/skills/agent-teams-creator
Claude Code: ~/.claude/skills/agent-teams-creator
OpenClaw:    ~/.openclaw/skills/agent-teams-creator
Hermes:      ~/.hermes/skills/agent-teams-creator
```

If your tool prefers project-local skills, place the repo there and keep `SKILL.md` at the skill root.

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

Then copy or symlink the same folder into any additional agent's skills directory if you use multiple tools.

If you use `CODEX_HOME`, install it under:

```text
$CODEX_HOME/skills/agent-teams-creator
```

Restart or refresh your agent after installation so the skill list refreshes.

### Claude Code example

```bash
git clone https://github.com/Arthurescc/agent-teams-creator \
  "${HOME}/.claude/skills/agent-teams-creator"
```

## Example Prompts

- `Use $agent-teams-creator to explain how a protocol-driven agent team runtime should work.`
- `Use $agent-teams-creator to design a coordinator-plus-workers system for this repo.`
- `Use $agent-teams-creator to add a task board and mailbox protocol to our multi-agent runtime.`
- `Use $agent-teams-creator to compare our current swarm design against a stronger team-runtime model.`

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

## What Changes After You Use It

Instead of vague multi-agent commentary, you should get:

- a coordination spine you can diagram
- a protocol you can implement
- role and permission boundaries you can test
- a verification path that does not depend on workers grading themselves

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

## Development

This project is independently designed and developed for practical agent-skill workflows across multiple coding agents.

It is focused on making multi-agent runtime design more explicit, protocol-driven, and implementation-ready.

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

## FAQ

### Is this only for Codex?

No. It is written in the open `SKILL.md` style and is intended to be reused across Codex, OpenClaw, Claude Code, Hermes Agent, and similar tools.

### Is this only for greenfield multi-agent systems?

No. It is also useful when refactoring an existing swarm, coordinator-worker runtime, reviewer loop, or shared-task orchestration system.

### Why does the README talk so much about task board vs mailbox vs coordinator?

Because that separation is usually the missing piece. Once state, transport, and authority are explicit, the system becomes much easier to explain, implement, and verify.

## License

MIT
