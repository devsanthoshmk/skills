---
name: agy-terminal-subagent
description: Custom agy cli based better subagent Whenever a task would normally benefit from a subagent use this skill.
---

# AGY Subagent Delegation

Delegate tasks to the Antigravity CLI (agy) as headless sub-processes. This can replace built-in subagents with fully configurable CLI workers that support custom model routing with reasoning tiers, session continuation.

## Invocation

### New session

```bash
agy -p "<prompt>" \
  --dangerously-skip-permissions \
  --output-format json \
  --model "<model>" \
  --print-timeout 24h \
  | jq .
```
#### example output:
{ "conversation_id": "6d36ef13-ca69-4b55-aaac-5b57aa02cba2", "status": "SUCCESS", "response": "Agent response text...", "duration_seconds": 12.1, "num_turns": 1, "usage": { "input_tokens": 28160, "output_tokens": 292, "total_tokens": 28452 } }

### Continue a session

```bash
agy --conversation "<CONVERSATION_ID>" \
  -p "<follow-up>" \
  --dangerously-skip-permissions \
  --output-format json \
  --model "<model>"\
  --print-timeout 24h \
  | jq .
```

## Model selection

Choose the cheapest model that can reliably complete the task:

| Model                      | Use for                                                                                   |
| -------------------------- | ----------------------------------------------------------------------------------------- |
| `gemini-3.7-flash-low`     | Simple edits, extraction, formatting, straightforward text processing               |
| `gemini-3.7-flash-high`    | Clear, self-contained implementation tasks with little independent decision-making        |
| `claude-sonnet-4-6`        | Complex implementation/debugging requiring more decisions while remaining token-efficient |
| `claude-opus-4-6-thinking` | Architecture, planning, difficult trade-offs, or high-value decisions (this model is the costliest model)|

Rule of thumb:

```text
very simple execution -> flash-low
have clear execution plan -> flash-high
complex execution but high token consumption -> sonnet
important decisions and complex execution -> opus
```

Do not use Opus merely because a task is large; use it when the quality of decisions matters.

## Delegation prompt

Give the subagent a self-contained task.

Include:

1. Goal
2. Relevant constraints
3. Expected output
4. Important files/context
5. Verification requirements

Avoid repeating information already available to the subagent through the repository or tools since the this agy agent run with all the rules skills that you have.

Prefer:

```text
Inspect the repository and fix X.

Constraints:
- Keep API compatibility.
- Do not change unrelated behavior.

Verify:
- Run the relevant tests.
- Report changed files and remaining issues.
```

## Session continuation

Save `conversation_id` from the JSON response when follow-up work is likely.

Use the same conversation for iterative work:

```text
initial investigation
        ↓
implementation
        ↓
verification
        ↓
follow-up fixes
```

Start a new session when the task changes substantially or the existing context becomes noisy.

## Transcript inspection

AGY stores session transcripts locally.

```text
~/.gemini/antigravity-cli/brain/<CONVERSATION_ID>/.system_generated/logs/
```

Important files:

```text
transcript.jsonl       # compact transcript
transcript_full.jsonl  # full raw transcript
```

Prefer `transcript.jsonl`. Use `transcript_full.jsonl` only when truncated output is actually required.

Never load the entire transcript into context.

### Extract user/assistant messages

```bash
jq -r '
  select(.type=="USER_INPUT" or .type=="PLANNER_RESPONSE")
  | "[\(.source)]: \(.content)"
' transcript.jsonl
```

### Extract tool activity

```bash
jq -c '
  select(.tool_calls != null)
  | {thinking, tools: .tool_calls}
' transcript.jsonl
```

Use `conversation_id` for continuation and `response` for the subagent result.
