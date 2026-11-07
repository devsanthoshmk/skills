# unity-lite

Multi-agent coordination workflow for autonomous coding agents, OpenCode, and OpenChamber.

## Purpose

`unity-lite` gives coding agents a structured workflow for multi-agent coordination without directory dependencies or complex file trees. It runs directly within the Main Thread Agent (Level 0) to manage planning, execution delegation, and two-stage verification.

## Installation

Install `unity-lite` directly with the skills CLI:

```bash
npx skills add devsanthoshmk/skills --skill unity-lite
```

Or install by repository path:

```bash
npx skills add devsanthoshmk/skills/skills/unity-lite
```

## Agent roles

The workflow organizes work across hierarchical levels:

- **Main Thread Agent (Level 0).** Evaluates task scope, executes single-step actions directly, or spawns child subagents for isolated or parallel work.
- **Planning Agent (Level 1).** Decomposes complex tasks into execution groups, sets constraints, maps dependencies, and creates test requirements.
- **Worker Agents (Level 1).** Implement assigned tasks using self-contained prompts with file paths rather than inline dumps.
- **Verification Agent (Level 1).** Evaluates intent fulfillment, requirements adherence, and code health. Can spawn Level-2 agents for decision extraction and verification.
- **Decision Review Agents (Level 2).** Read workflow records to verify non-trivial architectural decisions.

## Execution workflow

```text
[Group 1] ──┬── <Task A>
            ├── <Task B>
            └── <Task C>
                 │
                 ▼
[Group 2] ──┬── <Task D>
            └── <Task E>
                 │
                 ▼
[Group 3] ──── <Task F>
```

Tasks within a group execute in parallel. Tasks with downstream dependencies wait for previous groups to finish.

## Correction loop

When verification or decision review detects an issue:

1. Return findings to the same agent that produced the work.
2. Include concrete evidence and the required fix.
3. Fix the problem in the existing agent context.
4. Re-test changed work.
5. Re-evaluate affected decisions.

The loop caps retries at 3 attempts before escalating to the user.

## Completion contract

Every workflow run reports exactly one terminal status:

- `Completed`
- `Incomplete`
- `Blocked`
- `Failed`
- `Requires User Input`

## Specification

The complete agent specification lives in [SKILL.md](SKILL.md).

# Recomendations:

To get the best result from this skill using **gemini models** add these in your agent.md if possibile add it in global or at even higher level like adding a global rules in you _proxy server_.

## Agent rules:

```md

# Efficiency directives for Gemini 3.8 model

## R1. Read file at most once
Maintain a ledger of file paths, ranges read, and edits since. Never re-read an unedited file or range already in context. Re-read only the modified region after an edit. When unsure whether a range is already in context, do NOT re-read to check **grep** for the target instead; re-reading to verify presence is itself a violation.

## R2. Continue reads forward without overlap
When a tool response shows lines X to Y of a file Z, start the next read at offset Z plus 1. Never request overlapping or repeated ranges.

## R3. Target reads and cap volume
You must limit reads to 200 lines or fewer around a known symbol or match. Read entire files only when the file is around 800 lines or fewer. 

## R4. Batch independent discovery and scripting
First think for a moment what and all you want from the tools call and you **MUST batch every reads, greps, globs or any tool call into multiple `tool_use` blocks** in a single assistant message. NEVER issue independent tool calls as separate messages. One assistant message with 'n' number of `tool_use` blocks = one batch. The harness returns all results in one user turn.

## R5. Gate verification on recorded changes
Do not rerun test, build, deploy, type-check, or lint commands unless relevant code or configuration changed since the previous run. Run at most 1 full verification per fix batch. Prefer targeted file checks before full project checks. **After 2 identical failures, stop and report**. Read-only diagnostic queries are exempt from this gate.

## R6. Tool-carrying message can carries text
Every response that emits `"type": "tool_use"` parts can also emit at least one `"type": "text"` part in the SAME message atleast one sentence stating things like why it is doing, if it could take long, or quick responses before the tool call to the user. Batched calls can get a one-line header naming the targets this way. So makes it plesant to work with you with this. **DO NOT ADD THIS FOR ALL TOOL CALLS** only when needed.

## R7. Validate arguments and recover narrowly
Check tool argument types and enum constraints before sending. On a tool failure, apply backoff and make one targeted recovery lookup. Do not start broad discovery over paths or APIs already seen.

## R8. Ledger gates verification and reporting
Record all high-level decisions not explicitly given by the user that are required to complete the task, including relevant changes, commands, outcomes, problems, and corrections in Ledger `.agent/ledger-<sessionId>.md` (gitignored), falling back to `/tmp/opencode/ledgers/ledger-<sessionId>.md`. Read it before the final response. Correct any wrong decision before responding. use the sessionId given by your harness for naming

## R9. Delegate tasks
You **MUST** dispatch any exploration tasks, broken down tasks, token heavy tasks, planning works and all to subagents.

## R10. Fact check response:
You **MUST run a subagent** with a AVAILABLE better model than you to confirm critical tasks.

## Load skills
You **MUST** load the skills required based on the work and the user prompt.


```
*Note:*
- I have that ledger thing just to give gemini a feeling of someone is watching or it is being monitored so that it works properly. That improves its outcome a meaning full amount but it also increases time it takes for the same task without ledger. *Observed through my internal session audits i burn roughly about 132.8M a day and 6.5B total and alalysed almost all of the sessions.