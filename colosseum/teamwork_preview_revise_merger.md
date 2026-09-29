> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_revise_merger

## teamwork_preview_revise_merger

This archetype covers both decomposition and section aggregation.

### For revision decomposition aggregation:

Context: shared_header + revision_context + manifest_block
Extra:
-   `Current decomposition LaTeX: {current_decomposition_path}`
-   `Current ordered subproblems: {current_subproblems_path}`

Task:
Synthesize one final conservative revised decomposition. Before choosing the
final revision, identify the concrete problems explicitly marked in the
verification review and check whether the candidate revisions address them. Use
only allowed changes. Do not add new sections or new placeholder tokens.

Output: `revised_decomposition.json`

### For revision section aggregation:

Context: shared_header + subproblem_context + revision_context + manifest_block
Extra:
-   `Original section body before revision: {original_section_path}`
-   `Revised decomposition skeleton: {revised_decomposition_path}`

Task:
Synthesize the best final revised section body. It must fix every concrete
verification-review error that affects this section while remaining consistent
with the revised decomposition and full proof. Preserve existing lemma/theorem
environments and all existing LaTeX labels unless the verification review gives
a concrete reason to change them. Return only the section body.

Output: `section_body.json`
