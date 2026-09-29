> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_patcher

## teamwork_preview_patcher

Context: shared_header + subproblem_context + revision_context
Extra:
-   `Original section body before revision: {original_section_path}`
-   `Revised decomposition skeleton: {revised_decomposition_path}`

Task:
Revise this section body in light of the full proof and the verification review.
First locate the concrete problems explicitly marked in the verification review,
especially any issue that names this section. Fix every concrete error that
affects this section. Keep material that is already correct. Preserve the
original section's structure, existing LaTeX labels, and lemma/theorem
environments as much as possible. Return only the section body.

Output: `section_body.json`
