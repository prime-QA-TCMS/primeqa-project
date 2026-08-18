# Repository Registry

Last updated: 2026-08-18

## Existing repositories

| Repository | Current working classification | Default branch | Notes |
| --- | --- | --- | --- |
| `prime-QA-TCMS/primeQA-UI` | Frontend application | `main` | React/TypeScript product UI; consumes `fog-ui`; deeper current-state analysis pending |
| `prime-QA-TCMS/primeqa-common` | Shared API standardization package | `main` | Published as `prime-qa-api-common`; shared utilities, middleware and types for backend services |
| `prime-QA-TCMS/user-service` | Backend service | `main` | Node/TypeScript/Express/Mongoose; consumes `prime-qa-api-common`; deeper domain analysis pending |
| `prime-QA-TCMS/project-service` | Backend service | `main` | Project domain; current-state analysis pending |
| `prime-QA-TCMS/testcase-service` | Backend service | `main` | Test-case domain; current-state analysis pending |
| `prime-QA-TCMS/results-service` | Backend service | `main` | Test execution/results domain; current-state analysis pending |
| `prime-QA-TCMS/configuration-service` | Backend service | `main` | Configuration/rule domain; current-state analysis pending |
| `prime-QA-TCMS/fog-ui` | Shared UI component library | `main` | Published reusable/custom UI primitives consumed by `primeQA-UI` |
| `prime-QA-TCMS/primeqa-project` | Project control plane | `main` | Programme source of truth |
| `prime-QA-TCMS/primeqa-qa-ui-engine` | QA UI execution engine | `main` | Generic Playwright browser primitives; no business scenarios |
| `prime-QA-TCMS/primeqa-qa-api-engine` | QA API execution engine | `main` | Generic API primitives; no business scenarios |
| `prime-QA-TCMS/primeqa-qa-test-automation` | QA test orchestration | `main` | TCMS scenarios, cross-layer execution and reporting |

## Architectural dependency standards

### Backend

Backend services consume `primeqa-common` / `prime-qa-api-common` for standardized shared backend concerns. The shared package must not own service-specific business logic.

### Frontend

`primeQA-UI` consumes `fog-ui` for reusable/custom components and primitives. Product-specific workflows and business composition stay in the main UI repository.

### QA automation

`primeqa-qa-test-automation` depends on both QA engines. The engines remain independent of each other and do not own Prime QA business test cases.

## Analysis fields to populate

For every application repository, capture:

- purpose and bounded context
- language/framework/runtime
- build and package tooling
- persistence/data ownership
- inbound/outbound APIs
- dependencies on other Prime QA repositories
- authentication/authorization model
- configuration and environment requirements
- automated test coverage
- CI/CD workflows
- deployment model
- known defects/debt
- current implementation completeness
- security/privacy concerns
- ownership and operational considerations
