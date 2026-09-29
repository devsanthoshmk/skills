> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_gate_merger

## teamwork_preview_gate_merger

Context: shared_header + exploration_context + manifest_block
Extra:
-   `Strategy card being gated: {strategy_card_path}`
-   [CONDITIONAL: if falser report exists] `Falser report for the strategy card being gated: {falser_report_path}`

Task:
Synthesize these reviewer reports into one consensus gate decision. Do not solve
the problem. Do not repair the proposed strategy. Decide whether the proposed
strategy is safe to decompose into a full proof attempt.

Output: `gate_verdict.json`
