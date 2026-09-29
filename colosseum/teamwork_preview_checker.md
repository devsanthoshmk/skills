> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_checker

## teamwork_preview_checker

Context: shared_header + subproblem_context
Extra:
-   Background: You are verifying one section inside a larger proof draft assembled from ordered subproblems.
-   `Current section LaTeX under review: {section_latex_path}`
-   `Current master LaTeX document: {master_document_path}`

Task:
Decide whether this section is acceptable as written. Check mathematical
correctness, local completeness, and consistency with the earlier written
sections in the master document. If there is any missing detail that prevents
this section from being safely used by later sections, reject it.

Output: `verification_report.json`
