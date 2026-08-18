# AI-SDLC State Machine

## Canonical states

1. `PROPOSED`
2. `ANALYSIS_REQUIRED`
3. `ANALYSIS_IN_PROGRESS`
4. `READY_FOR_ARCHITECTURE`
5. `ARCHITECTURE_IN_PROGRESS`
6. `READY_FOR_PLANNING`
7. `PLANNED`
8. `READY_FOR_IMPLEMENTATION`
9. `IMPLEMENTING`
10. `DEVELOPER_VALIDATION`
11. `READY_FOR_REVIEW`
12. `CODE_REVIEW`
13. `QA_VERIFICATION`
14. `SECURITY_NFR_VERIFICATION`
15. `ACCEPTANCE_VERIFICATION`
16. `READY_TO_MERGE`
17. `MERGED`
18. `DONE`
19. `BLOCKED`
20. `HUMAN_DECISION_REQUIRED`
21. `REJECTED`

## Transition rules

- A work item may skip a specialist analysis state only when that analysis is explicitly not applicable.
- `READY_FOR_IMPLEMENTATION` requires explicit acceptance criteria and affected repositories.
- `READY_FOR_REVIEW` requires developer-level validation evidence.
- `READY_TO_MERGE` requires all applicable independent gates to pass.
- `DONE` requires merge plus objective/evidence closure; merge alone is insufficient.

## Failure routing

Failures route to root cause:

- unclear/missing business requirement → `HUMAN_DECISION_REQUIRED` or Business/Product Analysis
- architecture incompatibility → Architecture Analysis
- implementation defect → `IMPLEMENTING`
- unit/build/type/lint failure → `IMPLEMENTING`
- independent code-review failure → `IMPLEMENTING` or Architecture Analysis depending on cause
- functional/API/UI regression → `IMPLEMENTING` with QA evidence attached
- security failure → Security Analysis / `IMPLEMENTING`
- performance/NFR failure → Architecture/Implementation depending on bottleneck
- acceptance mismatch despite green tests → Business/Product Analysis or `IMPLEMENTING`

## Retry policy

The orchestrator must not endlessly redispatch the same prompt after repeated failure. After two materially similar failed attempts without new evidence, the item should be escalated to root-cause analysis. Repeated environmental/CI failures should be distinguished from code failures.

## Human-decision triggers

Move to `HUMAN_DECISION_REQUIRED` when:

- two approved requirements conflict;
- an unresolved product choice blocks implementation;
- material service boundaries would change;
- a public contract must intentionally break;
- a destructive migration/delete is proposed;
- a security control would be weakened/excepted;
- data-retention/compliance behaviour lacks authority;
- acceptance criteria are inherently ambiguous and cannot be resolved from evidence.

## Cross-repository work

A parent work item may have repository-specific child tasks. The parent cannot enter `ACCEPTANCE_VERIFICATION` until all required child tasks have reached the appropriate integration-ready state and cross-repository compatibility has been verified.
