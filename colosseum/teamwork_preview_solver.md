> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_solver

## teamwork_preview_solver

Context: shared_header
Extra:
-   `Trimmed context (document abstract + dependency sections only): {trimmed_context_path}`
-   `Section to solve: {section_id}`
-   `Section description: {section_description}`
-   `Strategy card: {strategy_card_path}`
-   [CONDITIONAL: if falser reports exist] `Relevant falser reports: {falser_reports_path}`
-   [CONDITIONAL: if gate verdict exists] `Gate verdict for the strategy card: {gate_verdict_path}`
-   [CONDITIONAL: if previous section draft exists] `Previous section draft: {previous_section_path}`
-   [CONDITIONAL: if previous feedback exists]
    `Previous verification feedback for this section: {section_feedback_path}`

Task:
Write a rigorous proof for the assigned section. Return valid LaTeX for the
section body. Your proof must be self-contained within the section while being
consistent with the master document's notation and structure.

Output: `section_body.json`
