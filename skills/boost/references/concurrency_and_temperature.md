> Harness: opencode/OpenChamber transport only — semantics unchanged. See ../../teamwork/HARNESS_ADAPTER_OPENCODE.md (invoke_subagent->subagent background=true batches of 16, view_file->read, write_to_file->write/edit, T->wording+agent choice).

# Boost Concurrency & Temperature Reference

## 1. Sliding Window Queue Implementation

When coordinating $N$ tasks with max concurrency $C = 128$:
```
Queue: [Task 0, Task 1, ..., Task N]
Active Window: [Task 0 ... Task 127]
On task_complete(i):
    Pop Task 128 from Queue
    Dispatch Task 128 immediately
```

## 2. Inactivity Stuck Recovery
- If a running subagent shows no file writes or heartbeat events for $> 30$ minutes:
  1. Terminate the unresponsive worker PID/subagent session.
  2. Spawn a replacement worker with identical seed parameters.
  3. Limit maximum retries per node to 3 attempts.

## 3. Quorum Signal Structure
When quorum threshold is satisfied at Level $L$:
```
QUORUM_REACHED level_index=<L> manifest_path=<output_results_path>
```
Parent orchestrators register the quorum signal and immediately commence Level $L+1$ assignment mapping.
