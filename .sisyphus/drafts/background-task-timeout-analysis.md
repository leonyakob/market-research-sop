# Draft: Background Task Timeout Root-Cause (ses_25aed1cf9ffeO2oi0jYrqw42Sq)

## Facts (verified)
- Session: `ses_25aed1cf9ffeO2oi0jYrqw42Sq`.
- The system reminder reported: "Task timed out after 30 minutes of inactivity".
- In the captured transcript, the failed task is:
  - ID: `bg_a76a7679`
  - Description: `Review for minimal drift workflows`
  - Evidence: `/home/king/.local/share/opencode/tool-output/tool_dd7d5b21e001nbAckZUzPztMXX:2577-2586`.
- User-provided prompt snippet mentioned a different ID (`bg_7d317a5d`). That ID does NOT appear in the transcript file we inspected.
  - Evidence: `grep(bg_7d317a5d)` returned no matches in `/home/king/.local/share/opencode/tool-output/tool_dd7d5b21e001nbAckZUzPztMXX`.
- Attempting to retrieve output after the fact fails:
  - `background_output(task_id="bg_a76a7679")` -> `Task not found`
  - `background_output(task_id="bg_7d317a5d")` -> `Task not found`

## Timeline (from transcript)
- 2026-04-28T13:17:46Z: user states preference for near-zero drift.
- 2026-04-28T13:17:46Z: assistant triggers a tool call (`[tool: task]`).
  - Evidence: `/home/king/.local/share/opencode/tool-output/tool_dd7d5b21e001nbAckZUzPztMXX:2550-2556`.
- 2026-04-28T13:25:42Z: user answers `1` to a multiple-choice.
  - Evidence: `...:2569-2571`.
- 2026-04-28T13:25:42Z: assistant triggers `[tool: background_output]`.
  - Evidence: `...:2572-2574`.
- 2026-04-28T14:23:57Z: system reminder: `bg_a76a7679` timed out after 30 minutes of inactivity.
  - Evidence: `...:2577-2587`.
- 2026-04-28T14:26:44Z: user requests rerun.
  - Evidence: `...:2592-2596`.

## What "30 minutes of inactivity" most likely means
- The background task did not emit any observable activity events (tool calls, streamed output, progress updates) for >= 30 minutes.
- This is a watchdog-style timeout; it does NOT necessarily mean the task crashed immediately.

## Root-Cause Hypotheses (ranked)

### H1. Long-running task produced no heartbeats/progress events (most likely)
- The task appears to be a deep, synthesis-heavy review ("minimal drift workflows"). If the subagent spends a long time thinking before producing output, the orchestration layer can interpret this as inactivity.
- Supporting evidence: transcript shows the timeout, but does not show any intermediate tool events from that background task between the last status check and the reminder.
- Gap: we do not have the background task's internal logs or its exact prompt/agent type parameters.

### H2. Background task got stuck waiting on a tool/permission in a non-interactive context
- Some background contexts cannot complete if a permission prompt/interactive step is required.
- Supporting evidence: none specific for this task ID (the retained logs do not include the 2026-04-28 window).
- Gap: the relevant date's log file(s) are not present under `/home/king/.local/share/opencode/log` (only 2026-04-29 logs exist at the moment).

### H3. Provider/network stall (LLM request hung, no stream)
- If the provider call stalls and no tokens stream back, the task may appear "inactive".
- Supporting evidence: none directly tied to the 2026-04-28 window.
- Gap: same as above (missing logs).

## Why the ID mismatch matters (bg_a76a7679 vs bg_7d317a5d)
- It suggests either:
  - multiple retries produced different background task IDs, or
  - the user copied a later/earlier reminder from a different attempt, or
  - the UI displayed a different internal ID.
- Without disambiguation, we cannot correlate one task's internal state to another.

## Concrete remediation (actionable)

### For retrying similar background tasks
- Decompose the review into smaller background tasks (each <= 10-15 minutes expected):
  - "Enumerate drift sources"
  - "Evaluate mitigation levers"
  - "Draft strict workflow"
- Force periodic progress emission in the prompt:
  - Require the subagent to produce a progress message every N minutes / after each step.
- Prefer `run_in_background=false` for single, long, deep reasoning tasks so the main session keeps receiving streamed output and cannot go "inactive".

### For system-level robustness (if you can change OpenCode)
- Add "heartbeat" events for long-running agent tasks even when they are not calling tools.
- Make the inactivity watchdog configurable per task.
- Persist failed background task traces so `background_output` can still retrieve partial logs/results.

## Open Questions (need user confirmation)
- Which failure should we analyze as the canonical one: `bg_a76a7679` (found in transcript) or `bg_7d317a5d` (from your pasted reminder)? If you have the UI screenshot or a newer transcript that includes `bg_7d317a5d`, we can reconcile.
- Do you still have any `2026-04-28*.log` files under `/home/king/.local/share/opencode/log` (or were they rotated/deleted)? If present, we can pinpoint whether it was a permission wait vs provider hang.
