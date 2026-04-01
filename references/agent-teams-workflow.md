# Agent Teams Workflow

This reference explains how to analyze, design, or build a Claude Code-style agent teams runtime.

## Build order

Do not start with a swarm. Start with the control plane and add complexity in this order:

1. single-agent loop
2. tool registry and permission model
3. planning plus shared task board
4. team bootstrap and persistent membership state
5. teammate spawn backends
6. mailbox protocol
7. coordinator prompt and worker roles
8. in-process or remote worker lifecycle
9. worktree isolation
10. verifier flow

If the single-agent loop is weak, adding agent teams will multiply confusion instead of throughput.

## Module boundaries

Use a layout close to this when building a new system:

```text
src/
  teams/
    bootstrap/
      team-file.ts
      team-context.ts
      lead-identity.ts
    tasks/
      board.ts
      claim.ts
      blockers.ts
      assignment.ts
    spawn/
      spawn-worker.ts
      backends/
        in-process.ts
        tmux.ts
        iterm.ts
        fallback.ts
    mailbox/
      inbox.ts
      lock.ts
      protocol.ts
      routing.ts
    coordinator/
      prompt.ts
      delegation.ts
      synthesis.ts
      verification.ts
    workers/
      runner.ts
      context.ts
      idle.ts
      permissions.ts
    isolation/
      worktree.ts
      cleanup.ts
    observability/
      events.ts
      audit.ts
```

The exact names can vary. The important part is separating:

- durable team state
- task state
- message transport
- worker execution
- isolation and cleanup

## Design checklist

### Team bootstrap

- Create a durable team file.
- Give the lead a deterministic identity.
- Bind the team to a shared task list immediately.
- Reflect the team in in-memory UI/runtime state too.

### Spawn contract

Every teammate spawn should specify:

- teammate name
- role or agent type
- prompt / initial objective
- team name
- model or inherit policy
- permission mode
- write scope
- whether plan approval is required

### Message contract

Do not rely on plain-text-only messaging.

Support at least:

- plain text work messages
- task assignment
- permission request / response
- plan approval request / response
- shutdown request / response
- idle notification
- optional team-wide permission update

Make each structured message parseable and routeable.

### Coordinator contract

The lead should:

- break work into tasks
- assign or let workers claim tasks
- synthesize findings before implementation follow-ups
- choose continue-vs-spawn based on context overlap
- send verification to a fresh or isolated actor

### Worker contract

Each worker should:

- own one bounded task at a time
- maintain isolated context
- know how to report blocked / failed / complete
- request help or permission through protocol messages
- avoid silently modifying unrelated files

### Isolation contract

For code-writing workers, prefer:

- separate worktree
- separate cwd
- explicit cleanup semantics
- refusal to destroy work without proof or confirmation

### Verification contract

Verification should be independent.

The verifier should:

- inspect the actual artifact or repo state
- run tests/checks
- report failure back into the task board
- not share the implementation worker's assumptions by default

## Forward-test scenarios

Use these to validate the skill or the runtime design:

1. Create a team and verify a new shared task list exists.
2. Spawn two workers with disjoint objectives and confirm identities stay stable.
3. Send a plain text message and a structured task assignment; confirm both route correctly.
4. Trigger a permission request from a worker and verify the lead can approve or reject it.
5. Trigger a plan approval request and confirm the worker does not continue before approval.
6. Simulate a worker completing a task and confirm idle notification reaches the lead.
7. Simulate a blocked task and verify blocker state is visible on the task board.
8. Put a code-writing worker in a worktree and confirm exit refuses destructive cleanup without confirmation.
9. Run a verifier against another worker's output and confirm failure flows back into tasks/messages.
10. Kill or stop a teammate and verify lifecycle state closes cleanly.

## Common mistakes

- Using the mailbox as the only state store.
- Letting workers coordinate only through natural-language transcript text.
- Treating coordinator mode as just "a nicer prompt" without durable runtime backing.
- Allowing workers to share one filesystem context without explicit ownership.
- Letting implementation workers self-certify success.
- Building auto-claim or swarm autonomy before the task model is dependable.
