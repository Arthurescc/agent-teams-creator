---
name: agent-teams-creator
description: Analyze, design, or implement protocol-driven agent team runtimes. Portable across Codex, OpenClaw, Claude Code, Hermes Agent, and similar SKILL.md-compatible coding agents. Use when the user wants a coordinator-plus-workers system with shared tasks, mailbox messaging, plan approval, teammate permissions, worktree isolation, or verifier-style team workflows.
---

# Agent Teams Creator

## Overview

Treat agent teams as a runtime protocol, not just "spawn more agents." Build or analyze the system around five linked primitives: team bootstrap, worker spawn, shared task board, structured mailbox messaging, and isolated execution with verification.

Read `references/agent-teams-mechanism.md` for the source-backed Claude Code model. Read `references/agent-teams-workflow.md` when you need the implementation order, module boundaries, or forward-test scenarios.

## Workflow

1. Classify the request.
   - Determine whether the user wants:
     - source analysis of an existing team runtime
     - a new Claude Code-style team harness
     - a refactor of an existing multi-agent system
     - a narrower subsystem such as mailbox protocol, task board, or verifier flow

2. Anchor the design around the real coordination spine.
   - Shared truth: team-scoped task board.
   - Delivery channel: mailbox / structured agent messages.
   - Execution units: isolated teammates with explicit identity.
   - Control plane: coordinator / team lead prompt and policy layer.
   - Safety boundary: worktree or equivalent isolated write scope.

3. Analyze or build in this order.
   - Team bootstrap: team file, deterministic lead identity, team-scoped task list.
   - Spawn layer: in-process vs pane/tmux/iTerm vs fallback mode.
   - Worker runtime: teammate context, inbox polling, idle/abort lifecycle.
   - Protocol layer: task assignment, permission request/response, plan approval, shutdown, mode sync.
   - Coordination layer: lead prompt, claim/update flow, task ownership, blocker handling.
   - Isolation layer: worktrees, bounded allowed tools, verifier or review worker.

4. Keep the protocol explicit.
   - Every teammate needs a visible identity, role, objective, and write scope.
   - Every state transition should flow through tasks or structured messages.
   - Do not rely on "shared understanding" between workers.

5. Distinguish validated behavior from inference.
   - Mark what is implemented in source.
   - Mark what is feature-gated or only described in comments/README.
   - Do not present inferred internal behavior as proven fact.

6. Force the analysis to reach the actual mechanism.
   - Do not stop at "I inspected these files."
   - Convert repo reading into a team-runtime model with explicit state, transport, role, and isolation boundaries.
   - For analysis requests, default to a best-effort explanation rather than optional clarification unless a missing choice would materially change the answer.

## Core Rules

- Make the task board the coordination backbone, not the prompt transcript.
- Use mailbox messages for negotiation, approvals, blockers, and control signals.
- Treat the team lead as a protocol authority, not just another worker.
- Keep workers isolated by default: fresh context, narrow tool set, explicit ownership.
- Give code-writing workers their own worktree or equivalent filesystem isolation when possible.
- Use a separate verifier or verification pass before claiming the team completed the task.
- Prefer source-backed protocol names and flows when analyzing Claude Code specifically.
- Explicitly state that the task board is the shared state and the mailbox is the transport.
- Explicitly distinguish default teammate isolation from optional worktree isolation.

## Output Contract

Unless the user asks for something narrower, include:

- coordination spine
- source-backed protocol/message types
- spawn modes and teammate identity model
- task board model
- coordinator or lead role
- isolation stance
- verification stance
- source-backed facts vs inferred or gated behavior

## When To Read References

- Always read `references/agent-teams-mechanism.md` before substantial work on this topic.
- Read `references/agent-teams-workflow.md` when:
  - building a new team runtime
  - refactoring an existing swarm/coordinator system
  - deciding where to place spawn, mailbox, task, or verifier logic
