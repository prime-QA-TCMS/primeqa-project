# Risks, Assumptions, Issues, and Decisions

## Risks

| ID | Risk | Impact | Likelihood | Mitigation | Owner | Status |
| --- | --- | --- | --- | --- | --- | --- |
| R-001 | Existing repositories may contain incomplete or inconsistent cross-service contracts | High | Unknown | Perform contract/dependency analysis before broad implementation | Project | Open |
| R-002 | Missing regression automation may allow existing behaviour to break during completion work | High | High | Establish risk-based API and Playwright suites early in execution | QA | Open |
| R-003 | Target requirements may differ from assumptions encoded in current code | High | Unknown | Separate observed behaviour from approved requirements and obtain decisions for conflicts | Product/Project | Open |

## Assumptions

| ID | Assumption | Validation needed | Status |
| --- | --- | --- | --- |
| A-001 | The existing repositories collectively represent the current TCMS implementation baseline | Repository analysis | Open |

## Issues

| ID | Issue | Impact | Action | Status |
| --- | --- | --- | --- | --- |

## Decisions needed

| ID | Decision | Why needed | Status |
| --- | --- | --- | --- |
| D-001 | Confirm final target product brief and acceptance boundaries | Required for authoritative gap analysis | Open |
| D-002 | Confirm automation repository names after technical analysis | Required before creating new automation repos | Open |
