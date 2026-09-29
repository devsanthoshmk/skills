> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_solve_falser

## teamwork_preview_solve_falser

Context: shared_header + subproblem_context
Extra:
-   `Trimmed context (document abstract + dependency sections only): {trimmed_context_path}`
-   `Strategy card: {strategy_card_path}`
-   [CONDITIONAL: if falser reports exist] `Relevant falser reports: {falser_reports_path}`
-   [CONDITIONAL: if gate verdict exists] `Gate verdict for the strategy card: {gate_verdict_path}`
-   `Proposed section solution to audit: {section_solution_path}`
-   [CONDITIONAL: if previous falser report exists] `Previous falser report to preserve unless explicitly resolved: {previous_falser_report_path}`

Task:
Audit whether the proposed section actually proves the assigned subproblem.
Do not repair the section and do not write replacement LaTeX. Focus on the
first fatal bridge if one exists: false equivalence, wrong target, wrong-sided
bound, dropped factor, missing constraint, index mismatch, unsupported theorem
use, or code-probe misuse. Your output is an adversarial audit report, not a
repair plan.

Output: `falser_report.json`
