> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_explore_falser

## teamwork_preview_explore_falser

Context: shared_header + exploration_context
Extra:
-   `Strategy card to falsify: {strategy_card_path}`
-   [CONDITIONAL: if aggregation-level falser (Level 1+)]
    Child candidate references 

 same candidates consumed by the
    aggregation node that produced the strategy card above:

    Manifest file: {manifest_file_path_1}
    Candidate entry indices: {candidate_entry_indices_1}

    Manifest file: {manifest_file_path_2}
    Candidate entry indices: {candidate_entry_indices_2}

    ... (repeat for each manifest file)

    IMPORTANT: Issue ALL read calls CONCURRENTLY in a single turn.

    For each manifest file, read it with `read`, then for each entry
    index look up the entry's `falser_report_path` and read that child
    falser report. Audit whether the aggregated card silently inherited
    flaws that child falsers already identified.

Task:
Attack this exact strategy card. Do not summarize it and do not make it sound
stronger than it is. Identify the top blocking objections: central lemmas,
missing assumptions, overstrong intermediate claims, boundary regimes, dropped
objections, and cheap symbolic or numerical tests that could refute the strategy.
Your output is not a proof and not a repair plan. It is an adversarial
falsification report for the downstream aggregation nodes.

Output: `falser_report.json`
