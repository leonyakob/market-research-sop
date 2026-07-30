# Issues

## 2026-04-11T06:00:00Z Task: 1
Minor wording ambiguity identified: "仅改 8.3" could be misread as forbidding adjacent-section read-only checks; recommended future clarification is to explicitly state adjacent checks are diagnostic-only and non-modifying.

## 2026-04-11T06:00:00Z Task: 2
Risk noted: the draft must keep the boundary between internal input items and formal project files very explicit, or later substitution into AGENTS.md could be read as forcing full-path notation everywhere.

## 2026-04-11T06:00:00Z Task: 3
Potential future inconsistency hotspot: sections 8.2 and 8.4 cite formal docs with repo-relative paths; if 8.3 canonical-path rule is enforced broadly later, these should be reviewed in a separate, explicit follow-up task.
2026-04-11: The plan text needed concrete applicability conditions for each example; otherwise the allowed/forbidden boundary stayed too abstract for execution.
2026-04-11T06:00:00Z: No residual ambiguity remains after the 8.3 rewrite; issues are recorded only to preserve the earlier ambiguity trail.
