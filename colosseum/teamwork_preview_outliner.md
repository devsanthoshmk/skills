> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_outliner

## teamwork_preview_outliner

Context: shared_header
Extra:
-   `Strategy card guiding decomposition: {strategy_card_path}`
-   [CONDITIONAL: if falser report exists] `Falser report for the strategy card: {falser_report_path}`
-   [CONDITIONAL: if gate verdict exists] `Gate verdict for the strategy card: {gate_verdict_path}`
-   [CONDITIONAL: if previous proof exists] `Previous full-proof draft: {previous_proof_path}`
-   [CONDITIONAL: if previous feedback exists]
    `Previous verification feedback: {previous_feedback_path}`

Task:
Decompose the problem into a master LaTeX document with sections. Use placeholder
tokens of the exact form [[SUBPROBLEM_01]], [[SUBPROBLEM_02]], etc. Provide a
short strategy summary explaining why this decomposition is useful. If a falser
report is provided, do not silently build the decomposition on unresolved attacked
claims.

Output: `decomposition.json`
