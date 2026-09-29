> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_explorer

## teamwork_preview_explorer

Context: shared_header + exploration_context

Task:
Search for one promising high-level solution strategy before decomposition.
Do not produce a section list, master LaTeX document, or placeholder tokens.
Return exactly one one-layer strategy card using the schema fields directly; do not put the card inside another wrapper field.

Output: `strategy_card.json`
