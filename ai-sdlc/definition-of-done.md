# AI-SDLC Definition of Done

A work item is not `DONE` because code was written, tests were added, or a PR merged. Completion requires evidence appropriate to the scope and risk.

## Mandatory completion checks

Unless explicitly not applicable, completion requires:

- requirement/objective traceability;
- acceptance criteria satisfied;
- implementation complete in all affected repositories;
- developer-level tests added/updated;
- build/type/lint checks pass;
- independent code review completed with blocking findings resolved;
- API automation updated/passing where API behaviour changes;
- UI/E2E automation updated/passing where user-visible behaviour changes;
- cross-layer automation updated/passing where data/workflows span layers;
- authorization/tenant-boundary tests for permission-sensitive changes;
- security verification for security-relevant changes;
- NFR verification for performance/accessibility/reliability-sensitive changes;
- public contracts/documentation/configuration/deployment docs updated;
- migrations/rollback strategy validated where relevant;
- residual risks documented;
- Objective Auditor returns `PASS`.

## Shared-package gates

Changes to `primeqa-common` require:

- affected service inventory;
- backwards-compatibility assessment;
- package-level tests;
- consumer validation for affected services;
- API automation/regression evidence where contracts or middleware behaviour change.

Changes to `fog-ui` require:

- unit/component tests;
- catalogue/how-to updates;
- accessibility verification;
- automation-selector/interface stability assessment;
- affected `primeQA-UI` consumer validation;
- visual regression evidence when appearance/interaction changes materially.

## Security-sensitive changes

Authentication, authorization, secrets, uploads, integrations, tenancy, hard delete, configuration and external-input boundaries require explicit security review. No agent may waive a security requirement merely to make tests pass.

## Evidence package

The final evidence set should contain, as applicable:

- requirement IDs and acceptance criteria;
- PR/commit references;
- CI/check status;
- test reports;
- independent review result;
- security/NFR evidence;
- migration/deployment evidence;
- unresolved/residual risks;
- Objective Auditor outcome.

## Failed Definition of Done

If any mandatory applicable gate fails, the item returns to the lifecycle stage responsible for the failure. The orchestrator must record the failed evidence and preserve traceability rather than resetting the item as though the failed attempt never occurred.
