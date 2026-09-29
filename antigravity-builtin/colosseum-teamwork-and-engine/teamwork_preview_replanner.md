> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_replanner

## teamwork_preview_replanner

Context: shared_header + revision_context
Extra:
-   `Current decomposition LaTeX: {current_decomposition_path}`
-   `Current ordered subproblems: {current_subproblems_path}`

Task:
Revise the decomposition for a cleanup pass after verification feedback. First
identify the concrete problems explicitly marked in the verification review.
Allowed changes: rewrite abstract/roadmap material, slightly revise section
names and tasks, delete redundant sections. Forbidden changes: do not add
new sections, do not invent new placeholder tokens, do not reorder sections
unless deletion makes the remaining order collapse naturally, do not rewrite
section bodies.

Output: `revised_decomposition.json`
