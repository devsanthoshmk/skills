> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_plan_merger

## teamwork_preview_plan_merger

Context: shared_header + manifest_block
Extra:
-   [CONDITIONAL: if previous decomposition exists] `Previous decomposition: {previous_decomposition_path}`
-   [CONDITIONAL: if previous proof exists] `Previous full-proof draft: {previous_proof_path}`
-   [CONDITIONAL: if previous feedback exists] `Global verification feedback from the previous iteration: {previous_feedback_path}`
-   [CONDITIONAL: if strategy card exists] `Approved exploration strategy card: {strategy_card_path}`
-   [CONDITIONAL: if falser report exists] `Falser report for the approved strategy: {falser_report_path}`
-   [CONDITIONAL: if gate verdict exists] `Gate verdict: {gate_verdict_path}`

Task:
Produce a single final decomposition. Return at most {max_subproblems}
subproblems. If the previous iteration failed, use the verification feedback to
refine the decomposition for the next attempt. The returned master LaTeX skeleton
should preserve a document title and include a short abstract summarizing the
overall proof plan. Keep one section per subproblem with placeholder tokens of
the exact form [[SUBPROBLEM_01]], [[SUBPROBLEM_02]], etc. The subproblems array
must match the section order in decomposition_latex exactly.

Output: `decomposition.json`
