> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_verify_merger

## teamwork_preview_verify_merger

Context: shared_header + manifest_block
Extra:
-   Background: The Solver's goal is to solve the mathematical problem stated in the provided input .tex file. Your job is to produce a final verification verdict about whether the candidate proof actually solves that original problem.
-   `Candidate proof under review: {proof_latex_path}`

Task:
Based on all verification results, produce a final comprehensive verdict.
Read the entire candidate proof carefully. Consolidate all feedback into a
single comprehensive response. Explicitly check whether the proof solves the
exact problem stated in the input .tex file. If rejecting, write the final
report in feedback_latex. Use verdict_summary to explicitly begin with either
"Outcome 1: Accept as Complete Proof" or "Outcome 2: Reject as Incomplete /
Partial Progress".

Output: `verification_report.json`
