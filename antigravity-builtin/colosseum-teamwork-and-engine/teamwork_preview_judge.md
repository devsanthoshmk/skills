> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_judge

## teamwork_preview_judge

Context: shared_header + exploration_context
Extra:
-   `Final strategy card under review: {strategy_card_path}`
-   [CONDITIONAL: if falser report exists] `Falser report for the final strategy card: {falser_report_path}`

Task:
Act as a Strategy Feasibility Tester. Do not solve the problem. Do not repair
the proposed strategy. Decide whether the proposed strategy is safe to decompose
into a full proof attempt. Be adversarial but fair. Focus on finding the first
fatal unsupported bridge claim, not on making the strategy look good.

Output: `gate_verdict.json`
