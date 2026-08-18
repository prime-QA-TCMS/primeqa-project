# GitHub Project Model — Prime QA AI-SDLC

This is the canonical GitHub Projects v2 field model for the automated SDLC. GitHub Projects should mirror this file; the repository remains the source of truth if UI configuration drifts.

## Project

Recommended name: `Prime QA AI-SDLC`

## Required fields

| Field | Type | Values / purpose |
| --- | --- | --- |
| Status | Single select | Intake, Analysis, Architecture, Ready, Implementation, Review, QA, Verification, Human Decision, Blocked, Done |
| Work Type | Single select | Requirement, Capability, Story, Repo Task, Defect, Analysis, Architecture, QA, Security, Performance |
| Agent | Single select | Orchestrator, Business Analyst, Product Analyst, Technical Analyst, Architect, Security Analyst, Developer, Reviewer, QA Analyst, Automation Engineer, Performance Analyst, Objective Auditor |
| Priority | Single select | P0, P1, P2, P3 |
| Risk | Single select | Critical, High, Medium, Low |
| Target Repo | Text | `owner/repo` |
| Requirement IDs | Text | Comma-separated requirement IDs |
| Depends On | Text | Issue/work-item IDs |
| Evidence | Text | Links to PRs, CI runs, test evidence, ADRs |
| Human Gate | Single select | None, Product, Architecture, Security, Destructive, Release |
| Attempt | Number | Automated retry count |

## Labels

The automation treats labels as machine state and project fields as human-readable portfolio state.

### State labels

- `state:intake`
- `state:analysis`
- `state:architecture`
- `state:ready`
- `state:implementation`
- `state:review`
- `state:qa`
- `state:verification`
- `state:human-decision`
- `state:blocked`
- `state:done`

Exactly one `state:*` label should be present on an active work item.

### Agent labels

- `agent:orchestrator`
- `agent:business`
- `agent:product`
- `agent:technical`
- `agent:architect`
- `agent:security`
- `agent:developer`
- `agent:reviewer`
- `agent:qa`
- `agent:automation`
- `agent:performance`
- `agent:auditor`

### Control labels

- `ai:managed` — item is governed by the AI-SDLC.
- `ai:ready` — orchestrator may process it.
- `ai:running` — one automated execution is active.
- `ai:retry` — eligible for controlled retry.
- `ai:stop` — automation must not take further action.
- `human:required` — human decision/approval required.
- `cross-repo` — work affects more than one repository.
- `shared-package` — touches `primeqa-common` or `fog-ui`; stricter gates apply.

## Recommended views

1. **Lifecycle board** grouped by Status.
2. **Agent queue** grouped by Agent, filtered to not Done.
3. **Human decisions** filtered to `human:required`.
4. **Blocked / retry** filtered to Blocked or `ai:retry`.
5. **Shared package changes** filtered to `shared-package`.
6. **Release evidence** filtered to Verification and Done.

## Synchronization rule

The orchestrator may update labels automatically. Projects field synchronization can be added through a GitHub App/PAT with Projects v2 GraphQL permission; until that credential is configured, labels remain the executable state source.