---
name: teamwork
description: Antigravity Teamwork & Colosseum autonomous mathematical theorem-proving pipeline and tournament orchestration engine. Coordinates multi-phase proof exploration, LaTeX decomposition, parallel dependency-driven subproblem solving, adversarial falsification, tournament aggregation trees, and global referee verification waves under OpenCode / OpenChamber harnesses.
---

# Teamwork Autonomous Proof Orchestration & Tournament Engine

> Harness adapter (opencode / OpenChamber — behaviour unchanged):
> See [HARNESS_ADAPTER_OPENCODE.md](HARNESS_ADAPTER_OPENCODE.md) for full mapping.
> - Dispatch primitive: opencode `subagent` tool with `background=true` (batches of at most 16 per turn, back-to-back, refill on completion), or OpenChamber `session.create` + `session.send` in Code Mode.
> - File tools: `view_file` -> `read`, `write_to_file` -> `write` / `edit`.
> - Commands & search: `run_command` -> `shell`, web search -> `websearch` / `webfetch`.
> - Temperature asymmetry: creative phases (explorer, outliner, solver) use `general` / `explore` with creative prompts; strict audit & synthesis phases (mergers, falsers, judges, referees) use `sonnet-agent` / `opus-agent` with strict verification prompts.
> - Concurrency ceiling: 128 subagents, recursive partition into `teamwork_preview_layer` sub-orchestrators above 32 nodes, quorum ≥80% speculative progression.
> - Outer iteration loop: up to 32 iterations, tracking `## Iteration Status` in `progress.md`.

## System Overview

Colosseum executes mathematical theorem-proving and complex analytical workflows across 5 structured phases:

1. **Phase 1 — Strategy Exploration**: Parallel strategy generation (32 explorers) with adversarial falsification (explore_falser) and tournament aggregation (explore_merger), gated by feasibility judges (judge, gate_merger).
2. **Phase 2 — Problem Decomposition**: Expansion of approved strategy into master LaTeX skeleton with up to 10 subproblems (outliner, plan_merger).
3. **Phase 3 — Parallel Dependency-Driven Solving**: Concurrent solving of independent subproblems (solver) with interleaved falser audits (solve_falser) and tournament aggregation (solve_merger).
4. **Phase 4 — Parallel Global Verification**: Master proof assembly and combined referee review wave (referee) with verification tree aggregation (verify_merger).
5. **Phase 5 — Surgical Revision & Strategy Rollback**: Severity triage, conservative outline revision (replanner, revise_merger), and targeted subsection patching (patcher, solve_falser).

## Reference Documents

- [COLOSSEUM_FULL_SYSTEM_SPEC.md](COLOSSEUM_FULL_SYSTEM_SPEC.md): Complete 5-phase tournament architecture, manifest schemas, and subagent dispatch templates.
- [teamwork_preview_proof.md](teamwork_preview_proof.md): Stellar Orchestrator system prompt, handoff protocols, briefing templates, and fault tolerance ladder.
- [antigravity_boost_concurrency_engine.md](antigravity_boost_concurrency_engine.md): Concurrency scaling, sliding-window queue, and straggler injection.
- [antigravity_goal_mode_engine.md](antigravity_goal_mode_engine.md): Goal Mode continuation loop and state management.
