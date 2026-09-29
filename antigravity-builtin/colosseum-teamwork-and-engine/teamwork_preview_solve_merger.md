> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_solve_merger

## teamwork_preview_solve_merger

Context: shared_header + subproblem_context + manifest_block
Extra:
-   `Trimmed context (document abstract + dependency sections only): {trimmed_context_path}`
-   `Strategy card: {strategy_card_path}`
-   [CONDITIONAL: if falser reports exist] `Relevant falser reports: {falser_reports_path}`
-   [CONDITIONAL: if gate verdict exists] `Gate verdict for the strategy card: {gate_verdict_path}`
-   [CONDITIONAL: if previous proof exists] `Previous full-proof draft: {previous_proof_path}`
-   [CONDITIONAL: if previous feedback exists]
    `Verification feedback: {previous_feedback_path}`

Task:
Write the full LaTeX content that should replace the placeholder for this section.
Do not write a full standalone document. Synthesize the candidate section solutions
into the strongest single section replacement. Preserve notation, assumptions,
definitions, and already-established claims from the earlier filled sections unless
there is a compelling mathematical reason to refine them.

Output: `section_body.json`
