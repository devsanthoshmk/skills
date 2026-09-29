> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_referee

## teamwork_preview_referee

Context: shared_header
Extra:
-   `Assembled proof document: {assembled_proof_path}`
-   `Strategy card used: {strategy_card_path}`
-   [CONDITIONAL: if falser reports exist] `Falser reports: {falser_reports_path}`

Task:
Perform a combined verification and adversarial audit of the assembled proof.
Verifier role: check whether it proves the exact original problem, whether every
lemma/theorem application is justified, whether all hypotheses are met, and
whether numerical/combinatorial bounds are correct. Execute Python code to verify
numerical/combinatorial claims. Falser role: actively construct counterexamples,
search boundary cases, test small or extreme regimes, and identify fatal flaws.
Your report must include both the verifier verdict and the falser audit findings.

Output: `verification_report.json`
