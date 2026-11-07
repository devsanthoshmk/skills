---
name: unity-lite
description: Invoke when a task benefits from multi-agent collaboration and structured planning.
---

# Unity-Lite Workflow

These instructions define the **unity-lite** workflow executed directly by _you_ the Main Thread Agent (Level 0).

## Workflow

1. Understand the user's intent, expected outcome, constraints, and **acceptance criteria**.
2. Determine which stages/delegations are necessary in:
   - planning,
   - implementation,
   - verification.
3. For simple, single-step tasks: the Main Thread Agent MAY execute work directly (file writes, shell commands, code edits). You **Must** delegate for tasks that benefit from isolation, parallel execution, or a clean context to **Level-1 agents** (subagents parallel or sequential).
4. Give every delegated agent a self-contained prompt containing everything required for its assigned task.
5. Coordinate dependencies, feedback, and parallel execution.
6. **Prefer** delegation for complex or parallel work. Direct execution is permitted only for simple, single-step tasks.

## Planning

Create a **Planning Agent (Level 1)** when the task requires meaningful decomposition, architectural decisions, multiple implementation steps, or non-trivial dependencies.

The Planning Agent determines:

- implementation direction;
- required tasks;
- important decisions;
- relevant constraints;
- dependencies;
- implementation subagent structure graph and parallel execution patterns like below when delegation is useful.
- for each task, there should be a relevent testing and verification plan which will then be implemented by the Verification Agent.

**Subagent grapth**

                         +----------------------+
                         |      IMPLEMENTER      |
                         +----------------------+
                           /        ^       \   \
                          /         |        \   \
                         v          |         v   v
              +----------------+   |   +----------+   +--------------------------+
              |  Worker Agents |<--+   |  Testing |   | Final Implementation    |
              +----------------+       +----------+   |       Verification       |
                     *                               +--------------------------+
              Tasks are tested
              as soon as they
              finish

**Execution Group Diagram**

Render the execution order as ASCII art:

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

The Planning Agent can create **Level-2+ subagents** when the task requires additional specialized expertise or when the planning process benefits from a more granular approach. specify/enforce it in its prompt.

only skip separate planning when the task is simple enough to implement directly. Be cautious about skipping planning.

## Implementation

Implement as per the planning as it give everything needed to imeplement.

## Verification/Testing

Verification is not limited to checking whether the implementation technically works.

You must verify that the implementation meets the user's intent and all requirements.

create a Verification Agent (Level 1) to determine whether:

- the user's intent was implemented;
- required functionality works;
- requirements and constraints were respected;
- relevant edge cases work;
- important implementation decisions are logically sound;
- chosen approaches are technically appropriate;
- significant requirements were not lost;
- the implementation is reasonably appropriate for the project.

For simple tasks: you MAY verify directly by reading produced files and checking criteria

### Decision Review

The Verification Agent must identify important decisions made during implementation.

If the implementation context or decision history is unavailable, retrieve it from the Implementation Agent.

The Verification Agent may create a **Decision Extraction Agent (Level 2)** to read the implementation records on disk and return the decisions made during implementation.

After obtaining the decisions, determine which require independent verification.

For each decision requiring independent verification, create a **Decision Verification Agent (Level 2)** when useful.

Each Decision Verification Agent evaluates its assigned decision for:

- logical correctness;
- alignment with user intent;
- technical suitability;
- project/environment constraints;
- unnecessary complexity;
- likely problems or regressions;
- whether it is a reasonable choice among viable alternatives.

Do not independently verify trivial decisions.

The Verification Agent owns and consolidates all Level-2 findings.

## Correction Loop

If verification/Testing/Decision review finds an implementation or decision problem:

1. Return the finding to the **same Agent** that produced the work.
2. Include the evidence and required correction.
3. Require that agent to fix the issue using its existing context.
4. Retest the changed work.
5. Re-verify affected decisions when necessary.

Repeat until the implementation passes verification or the task becomes `Blocked`, `Failed`, `Incomplete`, or `Requires User Input`.
If you looped for 3 times without resolving the issue, escalate to a human user for guidance.

## Parallelism

Run independent tasks in parallel when useful.

Do not parallelize tasks with unresolved dependencies.

Prefer the fewest agents necessary to achieve reliable results.

## Self-Contained Delegation

Every delegation must contain everything required for the delegated task.

Include, as applicable:

- task and expected outcome;
- user requirements and constraints;
- relevant files or paths;
- dependencies;
- previous results or feedback;
- on-disk workflow record paths needed to retrieve context;
- exact verification scope.

Use file paths instead of copying information already stored in files.

Do not pass irrelevant context.

## Context Preservation

When sending feedback to an existing agent, preserve its original session/context.

Every correction must identify:

- target agent/session;
- affected work;
- specific problem;
- evidence;
- required correction.

The original Agent remains responsible for correcting its own work.

## Completion

Do not report completion merely because implementation finished.

Report `Completed` only when verification confirms:

1. user intent is satisfied;
2. required functionality works;
3. requirements and constraints are satisfied;
4. important implementation decisions are acceptable;
5. corrections have been retested.

Otherwise return exactly one:

`Incomplete` | `Blocked` | `Failed` | `Requires User Input`

## Glossary

- **the Main Thread Agent**: The agent executing the unity-lite workflow (you).
- **Levels**: The hierarchy of agents, where Level 0 is the Main Thread Agent (you), Level 1 are direct subagents, and Level 2+ are subagents of subagents.
