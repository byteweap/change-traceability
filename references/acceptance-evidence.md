# Acceptance Evidence

Every material acceptance criterion should map to a concrete test, command, review, artifact, or explicitly documented manual check.

Use this structure:

| ID | Acceptance criterion | Verification method | Result | Evidence or limitation |
|---|---|---|---|---|
| AC-001 |  |  |  |  |

Use `Not run` when a check was not executed and state why. Do not convert an unexecuted or inconclusive check into `Passed`.

## Review status

When code review is required or has been performed, it may be recorded alongside verification evidence:

| Check | Result | Evidence or limitation |
|---|---|---|
| Code review | Not required / Not run / In progress / Completed | Scope, findings, reviewer or review record |

Code-review status is separate from test results and business acceptance. Do not mark review as completed without evidence, and record unresolved findings as risks or remaining work.
