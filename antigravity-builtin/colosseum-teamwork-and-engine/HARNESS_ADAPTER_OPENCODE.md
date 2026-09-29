# Harness Adapter: Antigravity -> opencode / OpenChamber (behaviour unchanged)

All tournament semantics, tree shapes, falser coverage, phase order, and output
schemas in this folder are authoritative and unchanged. Only the execution
transport is remapped:

- `invoke_subagent` -> opencode `subagent` tool (preferred), or OpenChamber
  `session.create` + `session.send` in Code Mode. `Prompt` becomes `prompt` /
  `task` text verbatim from the Subagent Dispatch Templates; `Output:
  filename.json` means the worker writes that file under its working directory
  with `write` and reports the absolute path.
- `teamwork_preview_layer` (orchestrator) -> a `subagent` call with
  `background=true` acting purely as a manifest router. It reads only manifests,
  assignment files, and verdict summaries via `read`; it never inlines candidate
  bodies. Manifest schema (`type`, `num_nodes`, `entries[{node_index,
  result_path, falser_report_path}]`) is unchanged, under `.agents/`.
- `view_file` -> `read`. `write_to_file` -> `write` / `edit`. Working-directory
  rule unchanged: workers write scratch files only inside their own working
  directory, never the project root.
- `base_advanced` tools (Python exec, web search) -> `shell` (`python3 -c` /
  scripts) + `websearch` / `webfetch`.
- Temperature asymmetry (no `T` parameter in this harness): emulate by wording +
  agent choice. Creative generation (explorer, outliner, solver; Antigravity
  `T=1.0`): prompt "generate diverse, structurally varied candidates" and use
  `general` / `explore`. Strict synthesis and audit (mergers, falsers, judges,
  referees, verifiers; Antigravity `T=0.1-0.2`): prompt "be strict, adversarial,
  zero hallucination; verify every claim" and use `sonnet-agent` / `opus-agent`
  for verdicts. Budgets and quorum/straggler rules are unchanged.
- Concurrency: ceiling 128, initial batches of at most 16 `subagent` calls per
  turn issued back-to-back, refill-on-completion, partition into sub-layer
  orchestrators above 32 nodes, quorum `>=80%` speculative progression with
  `.stragglers.json` injection. Identical numbers, only the call shape changes.
- Goal Mode `GoalState` protobuf (`goal_text`, `start_step_index`,
  `continuation_count`, `goal_token_budget`) -> OpenChamber `session.create`
  with `goal=true` + `goalTokenBudget`, prompt seeded from `goal_text`; in plain
  opencode, run the outer iteration loop explicitly and track
  `## Iteration Status` in `progress.md` (cap 32).
