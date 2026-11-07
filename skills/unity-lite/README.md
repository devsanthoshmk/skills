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
