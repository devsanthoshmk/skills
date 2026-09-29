---
name: boost
description: Antigravity Boost and Concurrency Acceleration skill. Activates high-throughput parallel subagent orchestration, temperature asymmetry routing (high temperature for exploration, low temperature for strict verification), token budget allocation, and sliding-window concurrency scaling up to 128 workers. Use this skill when executing compute-intensive tasks, parallel code generation, multi-candidate tournament aggregation, or deep reasoning benchmarks.
---

# Antigravity Boost & Concurrency Acceleration

> Harness adapter (opencode / OpenChamber — numbers and regimes unchanged):
> `subagent` tool with `background=true` is the dispatch primitive (batches of
> at most 16 per turn, back-to-back, refill on completion); `teamwork_preview_layer`
> is a manifest-only router subagent (see `../teamwork/HARNESS_ADAPTER_OPENCODE.md`).
> No `T` parameter exists here — `T=1.0` means "diverse candidates" wording via
> `general`/`explore`, `T=0.1-0.2` means "strict, zero-hallucination" wording via
> `sonnet-agent`/`opus-agent`. Ceiling 128, partition above 32, quorum ≥80%.

Boost is Antigravity's high-throughput performance engine designed to eliminate serialization bottlenecks, accelerate multi-agent workflows, and maximize inference quality through adaptive temperature routing and concurrency scaling.

## 1. Core Operating Principles

### A. Sliding Window Subagent Concurrency
When executing massive multi-node tasks (e.g. generating multiple candidate architectures, parallel section solving, or bulk testing):
- **Concurrency Ceiling**: Dispatches up to **128 parallel subagents** concurrently.
- **Dynamic Refill**: Dispatches an initial batch of workers up to available limits. As individual child tasks finish, immediately backfills open slots with pending tasks rather than waiting for entire batches to finish (eliminates head-of-line blocking).
- **Partitioning Threshold**: When assigned work exceeds 32 nodes, partition work into slices of at most 32 nodes managed by sub-layer orchestrators (`teamwork_preview_layer`).

### B. Temperature Asymmetry Routing
Enforce dual-temperature regime across execution phases:
- **Creative Generation ($T = 1.0$)**: Applied during Strategy Exploration, Problem Decomposition, and Subsection Solving to maximize structural variance, algorithmic exploration, and novel candidate generation.
- **Rigorous Evaluation & Synthesis ($T = 0.2$)**: Applied during Tournament Mergers, Adversarial Audits (Falsers), and Referee Verification to ensure strict correctness, mathematical soundness, and zero hallucination.

### C. Quorum Advancement & Straggler Handling
- **Speculative Progression**: When a level reaches its quorum threshold (e.g., $\ge 80\%$ completed nodes), trigger speculative downstream aggregation rather than stalling on tail-latency workers.
- **Straggler Injection**: Late-arriving outputs from straggler subagents are dynamically injected into the candidate pool via `.stragglers.json` without invalidating already completed aggregation trees.

---

## 2. Dispatch Protocols & Procedures

### Step 1: Compute Partitioning and Concurrency
1. Assess total candidate requirements ($N$).
2. If $N \le 32$: Dispatch in non-blocking batches of 16 workers.
3. If $N > 32$: Calculate partitions $P = \lceil N / 32 \rceil$. Spawn $P$ sub-layer orchestrators with assigned `node_offset` ranges.

### Step 2: Set Inference Parameters
| Phase | Role / Archetype | Target Temperature | Token Budget |
| :--- | :--- | :--- | :--- |
| **Exploration / Brainstorming** | `explorer`, `outliner`, `solver` | $T = 1.0$ | Standard / High |
| **Adversarial Falsification** | `falser`, `critic`, `judge` | $T = 0.2$ | Focused / Low |
| **Tournament Synthesis** | `merger`, `patcher`, `curator` | $T = 0.2$ | Standard |
| **Global Verification** | `referee`, `verifier` | $T = 0.1 - 0.2$ | High |

### Step 3: Result Collection via Manifests
- **Never serialize large candidate payloads in orchestrator contexts.**
- Write all subagent outputs to isolated JSON/markdown files under `.agents/`.
- Orchestrators manage and pass **file path manifests** only:
  ```json
  {
    "type": "manifest",
    "num_nodes": 32,
    "entries": [
      {
        "node_index": 0,
        "result_path": ".agents/task_00/result.json",
        "falser_report_path": ".agents/task_00/falser_report.json"
      }
    ]
  }
  ```

---

## 3. Reference Sub-Documentation

For detailed algorithms on coprime prime-stride sampling, straggler recovery, and memory persistence across outer iterations, see [references/concurrency_and_temperature.md](references/concurrency_and_temperature.md).
