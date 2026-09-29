> Harness: opencode/OpenChamber transport only — semantics unchanged. See HARNESS_ADAPTER_OPENCODE.md. invoke_subagent->subagent tool (background=true, batches of 16), view_file->read, write_to_file->write/edit, T=1.0->diverse wording via general/explore, T=0.1-0.2->strict wording via sonnet/opus agents.

# Archetype: teamwork_preview_curator

## teamwork_preview_curator 

 Single mode (L0+ with one curator)

```
Working directory: {working_directory}
All scripts and scratch files MUST be written inside this directory.
Identity: {identity}

Problem statement: {problem_text_path}

Current iteration: {iteration}
Exploration round: {exploration_round}
Tournament level: {level}

Knowledge directory: {knowledge_dir}

Partition: 1 of 1

Manifest file: {manifest_file_path}
Read the manifest file. For each entry in the `entries` array, read its `result_path` and `falser_report_path` using read.
IMPORTANT: To avoid context overflow, process entries in batches of at most 8 entries
(16 files) per turn. Issue all read calls within a batch concurrently, then extract
knowledge and write output files for that batch BEFORE loading the next batch.

Read each file path above with read before starting your task.

Task:
Extract mathematical knowledge from the assigned strategy cards and their falser
reports. Read the hydrated context, then read the existing knowledge/index.md
(if it exists).

Failed approach handling:
- index.md uses a CATEGORIZED format with at most 20 failed approach categories.
- For each new failed_approach discovery, find the best-matching existing category
  in index.md and add the entry to that category's file (append a row to its
  Entries table, increment Count, update the count in index.md).
- Only create a new category if no existing one matches well AND total categories < 20.
- If at the cap of 20 categories, assign to the closest existing match.

Before creating any new theorem/observation/reference file, perform a SEMANTIC
DUPLICATE CHECK:
- Read the titles of all existing entries from index.md.
- If an existing entry covers the same mathematical concept (even with different
  wording), update the existing file instead of creating a new one.
- Only create a new file if no existing entry is semantically equivalent.

Node direction extraction (ONLY if aggregation level >
