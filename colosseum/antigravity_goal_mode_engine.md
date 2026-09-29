> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Antigravity Goal Mode Engine Specification (GoalState)

This document details Antigravity's internal Goal Mode protocol, continuation loops, and state schema.

```protobuf
message GoalState {
  enum State {
    STATE_UNSPECIFIED = 0;
    STATE_ACTIVE = 1;
    STATE_PAUSED = 2;
    STATE_COMPLETED = 3;
    STATE_FAILED = 4;
  }
  string goal_text = 1;
  int64 started_at = 2;
  int32 start_step_index = 3;
  int32 continuation_count = 4;
  State state = 5;
  int64 goal_token_budget = 6;
}
```

## Runtime Behavior
1. **Autonomous Continuation**: Unlike standard single-turn assistant calls, Goal Mode runs an execution loop where the model evaluates its own step outputs against `goal_text`.
2. **Step Index & State Resumption**: Tracks `start_step_index` and increments `continuation_count` across tool invocations and subagent dispatches until exit criteria are met.
3. **Token Budget Enforcement**: Restricts total token consumption per goal run (`goal_token_budget`) preventing infinite loops.
4. **Handoff / Interruption**: Saves snapshot checkpoints allowing graceful pause/resume if user input or tool authorization is required.

## Harness mapping (opencode / OpenChamber — same fields, different transport)

- OpenChamber Code Mode: `session.create` with `goal=true`, `prompt=<goal_text>`,
  `goalTokenBudget=<goal_token_budget>`; `session.send` / `session.status` carry
  `start_step_index` and `continuation_count` forward. Pause/resume via
  `session.status` snapshots.
- Plain opencode: run the continuation loop explicitly in-conversation (or via a
  `general`/`opus-agent` subagent with the goal text as its task) and enforce the
  budget manually; `STATE_COMPLETED`/`STATE_FAILED` are the loop exit criteria.
