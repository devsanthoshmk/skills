> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Antigravity Boost & Concurrency Engine Specification

## Core Mechanisms

### 1. Sliding Window Subagent Concurrency (Max 128 Workers)
- **Initial Batch Dispatch**: Dispatches initial parallel workers up to the concurrency ceiling (128 subagents) without blocking.
- **Refill on Completion**: As individual child subagents exit or return results, pending tasks immediately backfill vacant slots.
- **Partitioning for Scale**: If a level exceeds 32 nodes, work is partitioned into `ceil(num_nodes / 32)` slices managed by sub-layer orchestrators (`teamwork_preview_layer`), avoiding serial aggregation bottlenecks.

### 2. Temperature Asymmetry
- **Creative Generation ($T = 1.0$)**: Applied during initial exploration (`teamwork_preview_explorer`), decomposition (`teamwork_preview_outliner`), and subproblem solving (`teamwork_preview_solver`) to maximize structural and algorithmic variance.
- **Strict Synthesis & Evaluation ($T = 0.2$)**: Applied during tournament aggregation (`teamwork_preview_explore_merger`, `teamwork_preview_solve_merger`) and referee verification (`teamwork_preview_referee`, `teamwork_preview_verify_merger`) to eliminate hallucinations and guarantee rigorous proof/code validity.

### 3. Straggler Injection & Quorum Advancement
- At aggregation levels, once a quorum threshold of nodes completes, parent orchestrators can trigger speculative downstream tree building. Late-arriving worker outputs ("stragglers") are appended dynamically via `.stragglers.json` manifests.
