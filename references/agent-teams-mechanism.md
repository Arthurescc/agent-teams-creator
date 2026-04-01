# Agent Teams Mechanism

This reference summarizes the Claude Code agent teams mechanism from these repositories:

- `oboard/claude-code-rev`
- `sanbuphy/claude-code-source-code`

Use it to analyze or reproduce the architecture. Keep facts and inferences separate.

## 1. Mental model

Claude Code agent teams are not "a parent agent with random children."

They are a coordinated runtime with:

- a team file as durable membership state
- a team-scoped task board as shared truth
- structured mailbox messages as the control protocol
- isolated teammates that run either in-process or in panes/windows
- a coordinator/lead prompt that tells the lead how to delegate, synthesize, and continue workers
- optional worktree isolation for filesystem safety

The extracted analysis repo summarizes this ladder as:

- `s09` agent teams
- `s10` team protocols
- `s11` autonomous task claiming
- `s12` worktree isolation

See:

- [README.md](/E:/Claude_code_resource/claude-code-source-code/README.md)

## 2. Team bootstrap

Primary file:

- [TeamCreateTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/TeamCreateTool/TeamCreateTool.ts)

Observed behavior:

- `TeamCreateTool` creates one team per leader session.
- Team names are uniquified rather than failing on collision.
- The team lead gets a deterministic ID using `team-lead@teamName`.
- A team file is written to disk and registered for session cleanup.
- A team-scoped task list directory is reset and created immediately.
- The leader's `getTaskListId()` path is rebound to the team name so leader and teammates share the same task board.
- Team state is mirrored in `AppState.teamContext`.

Implication:

- Team lifecycle is explicit and durable.
- The task board is created at team bootstrap, not bolted on later.

## 3. Spawn modes and teammate identity

Primary files:

- [spawnMultiAgent.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/shared/spawnMultiAgent.ts)
- [spawnInProcess.ts](/E:/Claude_code_resource/claude-code-rev/src/utils/swarm/spawnInProcess.ts)
- [InProcessTeammateTask.tsx](/E:/Claude_code_resource/claude-code-rev/src/tasks/InProcessTeammateTask/InProcessTeammateTask.tsx)
- [README.md](/E:/Claude_code_resource/claude-code-source-code/README.md)

Observed behavior:

- Spawn path supports:
  - in-process teammates
  - split-pane teammates
  - separate-window tmux teammates
  - broader fork/worktree/remote patterns at the harness level
- Teammates receive deterministic IDs from agent name plus team name.
- Teammates inherit selected CLI flags, permission modes, model choices, and plugin settings.
- Initial instructions are delivered through the mailbox for pane-based teammates.
- In-process teammates skip mailbox bootstrapping and receive prompt directly at runner startup.

Key design point:

- Spawn is both an execution concern and an identity concern.
- Teammate runtime identity is stable enough to drive messaging, task ownership, and UI state.

## 4. Mailbox protocol

Primary files:

- [teammateMailbox.ts](/E:/Claude_code_resource/claude-code-rev/src/utils/teammateMailbox.ts)
- [SendMessageTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/SendMessageTool/SendMessageTool.ts)

Observed behavior:

- Mailboxes are file-backed inboxes under the team directory.
- Writes use lock files with retries to serialize concurrent senders.
- Messages can be plain text or structured protocol objects.
- Structured messages include:
  - permission request / response
  - sandbox permission request / response
  - plan approval request / response
  - shutdown request / approved / rejected
  - task assignment
  - team permission update
  - mode set request
  - idle notification
- `SendMessageTool` enforces routing and schema rules:
  - some message types must go only to the team lead
  - structured messages cannot be broadcast
  - cross-session paths are limited
- In-process routing can short-circuit into task state or background resume instead of only writing a file.

Implication:

- Mailbox is the protocol bus.
- Team interaction is not free-form chat; it is explicit message types plus limited text messages.

## 5. In-process teammate runtime

Primary files:

- [inProcessRunner.ts](/E:/Claude_code_resource/claude-code-rev/src/utils/swarm/inProcessRunner.ts)
- [InProcessTeammateTask.tsx](/E:/Claude_code_resource/claude-code-rev/src/tasks/InProcessTeammateTask/InProcessTeammateTask.tsx)

Observed behavior:

- In-process teammates run inside the same Node process with AsyncLocalStorage-based identity/context isolation.
- They wrap `runAgent()` rather than using a separate logic path.
- They maintain their own task state, message history, idle/running status, and abort controllers.
- They can request permission from the leader either through a UI bridge or mailbox fallback.
- They can compact their own history and continue with preserved context.
- They emit idle notifications back to the leader when finishing or becoming available again.

Implication:

- In-process mode is still strongly isolated at the runtime-context layer.
- Claude Code treats "same process" and "same context" as different things.

## 6. Coordinator mode

Primary files:

- [coordinatorMode.ts](/E:/Claude_code_resource/claude-code-rev/src/coordinator/coordinatorMode.ts)
- [README.md](/E:/Claude_code_resource/claude-code-source-code/README.md)

Observed behavior:

- Coordinator mode changes the system prompt of the lead.
- The lead is instructed to:
  - delegate higher-level work
  - synthesize worker findings before follow-up
  - continue workers when context overlap is high
  - spawn fresh workers for independent verification or clean retries
- The lead prompt explicitly models phases:
  - research
  - synthesis
  - implementation
  - verification
- Worker tools are described as a constrained capability set.

Implication:

- The team lead is itself a specialized role with protocol obligations.
- Coordination is prompt-guided, but backed by concrete task/message primitives.

## 7. Task board and claiming model

Primary files:

- [TaskCreateTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/TaskCreateTool/TaskCreateTool.ts)
- [TaskUpdateTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/TaskUpdateTool/TaskUpdateTool.ts)
- [TaskListTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/TaskListTool/TaskListTool.ts)
- [coordinatorMode.ts](/E:/Claude_code_resource/claude-code-rev/src/coordinator/coordinatorMode.ts)
- [README.md](/E:/Claude_code_resource/claude-code-source-code/README.md)

Observed behavior:

- Tasks live in a shared team-scoped list.
- Tasks support:
  - status
  - owner
  - blockers / blockedBy
  - metadata
- Task assignment can be emitted to teammates through the mailbox.
- The source-code analysis repo describes a more autonomous layer where teammates scan and claim tasks themselves.
- Even when autonomous claim logic is feature-gated or incomplete, the shared-task-board model is explicit.
- From the restored source, assignment itself is lightweight: initial work is often the spawn prompt, explicit reassignment is a structured mailbox message, and execution tracking is handled through registered background teammate tasks rather than a separate heavyweight central scheduler.

Implication:

- The task board is the real source of coordination truth.
- Mailbox messages are for control and notification, not as the long-term state store.

## 8. Plan approval and permission propagation

Primary files:

- [ExitPlanModeV2Tool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/ExitPlanModeTool/ExitPlanModeV2Tool.ts)
- [SendMessageTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/SendMessageTool/SendMessageTool.ts)
- [teammateMailbox.ts](/E:/Claude_code_resource/claude-code-rev/src/utils/teammateMailbox.ts)

Observed behavior:

- Teammates that require plan mode send a plan approval request to the team lead.
- The lead approves or rejects through structured mailbox responses.
- Approval can also carry permission mode inheritance.
- Permission changes for the team can be propagated via structured messages.

Implication:

- Team safety policy is centralized at the lead but transmitted through protocol messages.
- This is closer to a workflow engine than to ad hoc delegation.

## 9. Worktree isolation

Primary files:

- [EnterWorktreeTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/EnterWorktreeTool/EnterWorktreeTool.ts)
- [ExitWorktreeTool.ts](/E:/Claude_code_resource/claude-code-rev/src/tools/ExitWorktreeTool/ExitWorktreeTool.ts)
- [README.md](/E:/Claude_code_resource/claude-code-source-code/README.md)

Observed behavior:

- Worktrees are session-managed runtime state, not just shell commands.
- Entering a worktree updates cwd and clears caches that depend on repository context.
- Exiting a worktree refuses destructive removal unless the runtime can prove cleanliness or the user explicitly confirms discard.
- The analysis repo places worktree isolation at the top of the progressive team ladder.
- The broader source analysis suggests default team collaboration is still primarily shared-workspace coordination, with worktrees as an additional isolation primitive rather than the automatic default for every teammate.
- The restored spawn paths for swarm teammates do not automatically create a worktree per teammate; current best evidence points to process/pane isolation plus shared workspace by default, with worktrees handled by separate session-isolation tooling.

Implication:

- Worktree isolation is part of the team safety protocol, especially for code-writing workers.
- When documenting Claude Code specifically, do not claim every teammate automatically gets a separate worktree unless the source for that path is explicit.

## 10. What is source-backed vs inferred

Source-backed:

- team creation
- shared task-board binding
- spawn backends
- mailbox protocol
- structured plan/shutdown/permission messaging
- in-process teammate runtime
- coordinator-mode lead instructions
- worktree lifecycle tools

More inferred or feature-gated:

- fully autonomous worker claiming behavior
- some internal coordinator worker modules
- any Anthropic-internal swarm features not present in the public artifact

Rule:

- When documenting Claude Code agent teams, label internal or missing pieces as inferred.
- Do not flatten inferred behavior into a definitive implementation claim.
