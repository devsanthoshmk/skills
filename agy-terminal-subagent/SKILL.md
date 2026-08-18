---
name: agy-terminal-subagent
description: Custom agy cli based better subagent Whenever a task would normally benefit from a subagent, first evaluate whether it can be delegated to AGY Terminal Subagent, and use AGY when it is suitable.
---

# AGY Terminal Subagent

Delegate eligible work to AGY by default.

AGY is an external terminal-based execution system. Each AGY invocation is an isolated worker that only knows what is explicitly included in its prompt. It has **no access** to the primary agent's hidden reasoning, prior conversation, memory, or shared state.

Use this skill whenever a task can be completed independently in a single execution.

---

# When to Use

Delegate the task if it is:

- independent
- self-contained
- bounded
- read-heavy
- easier to verify than perform directly
- based on supplied files, logs, images, audio, video, documents, URLs, or other artifacts
- primarily extraction, search, comparison, verification, classification, summarization, analysis, or reporting

If a task can be safely split into multiple independent units, launch multiple AGY workers instead of overloading one prompt.

When possible, isolate noisy or expensive work from the primary conversation.

---

# When NOT to Use

Do not delegate when the task:

- depends on hidden conversation context
- requires interactive back-and-forth
- requires tools or permissions unavailable to AGY
- modifies shared state across multiple coupled operations
- is too small to justify delegation
- cannot be fully specified before execution

If information is missing, obtain it **before** launching AGY.

Example:

> Give me the implementation details and execution log for X before continuing.

---

# Preparing the Prompt

Every AGY prompt must be completely self-contained.

Include:

- the complete task
- required context only
- relevant files, excerpts, or URLs
- constraints
- required output format
- validation requirements
- logging requirements

AGY is single-pass.

Once execution begins, it cannot ask follow-up questions.

Everything required to complete the task must already be present in the prompt.

---

# Required Output

Request only the result—not internal reasoning.

Prefer outputs that are:

- concise
- verifiable
- artifact-based
- deterministic where possible

Always specify the expected output structure before launching AGY.

---

# Execution Log

Every AGY worker should generate a temporary execution log containing significant actions such as:

- commands executed
- files accessed
- searches performed
- warnings
- errors
- decisions
- generated artifacts

The final response should include the path to this temporary execution log.

---

# Missing Information

If AGY cannot complete the task because required information is missing, it should return:

```
NEEDS_INPUT: <missing information>
```

Do not continue from incomplete work.

Instead:

1. gather the missing information
2. create a new self-contained prompt
3. launch a new AGY worker
4. ignore any incomplete output from the previous execution

---

# Model Selection

Choose the smallest capable model.

| Model | Use For |
|--------|----------|
| gemini-3.7-flash-medium | very small, routine, or trivial extraction and scanning work |
| gemini-3.7-flash-high | standard reasoning, structured reading, summarization, comparison, and moderate analysis |
| claude-opus-4-6-thinking | complex reasoning, high-value decisions, difficult analysis, or situations where maximum accuracy is worth the higher cost |

---

# Verification

Treat AGY output as evidence rather than unquestionable truth.

For important work, verify using one or more of:

- another AGY worker
- a different model
- direct tool verification
- a fresh independent pass

---

# Default Behavior

When a task satisfies the delegation criteria:

1. Gather any missing context.
2. Construct a complete self-contained prompt.
3. Choose the smallest capable model.
4. Launch AGY.
5. Review the result.
6. Verify when appropriate.

Perform work directly only when delegation is unnecessary or unsuitable.

---

# Invocation

Launch AGY using:

```bash
agy -p "<complete self-contained prompt>" --model "<exact model name>"
```

Where:

- `-p` is the entire prompt
- `--model` must exactly match the selected model identifier

Example:

```bash
agy -p "Summarize the following document and extract all security risks: docs/security.md. Return a markdown table and include the execution log path." --model "gemini-3.7-flash-medium" --dangerously-skip-permissions
```

If the task contains multiple unrelated units of work, invoke multiple AGY workers instead of combining everything into one prompt.

---

# Glossary

## Primary Agent

The AI currently interacting with the user.

---

## AGY

The external terminal CLI used to execute delegated work.

---

## Worker

A single AGY execution.

Each worker is isolated and executes exactly one self-contained task.

---

## Subagent

An AGY worker launched by the primary agent.

Within this skill, **worker** and **subagent** are interchangeable terms.

---

## Terminal Worker

An AGY worker executing inside a terminal environment using only the context provided in its prompt.

---

## Terminal Subagent

A terminal worker launched for one isolated task.

It:

- has no access to hidden conversation context
- cannot perform follow-up interaction
- cannot access previous AGY executions
- communicates only through its final output and execution log

---

## Prompt

The complete instructions supplied to an AGY worker.

Because AGY is single-pass, the prompt must contain everything needed for successful execution.

---

## Execution Log

A temporary file generated during execution that records significant actions performed by the worker.

Its path should always be included in the final response.
