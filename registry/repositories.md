# Repository Registry

Last updated: 2026-08-18

## Existing repositories

| Repository | Current working classification | Default branch | Notes |
| --- | --- | --- | --- |
| `prime-QA-TCMS/primeQA-UI` | Frontend | `main` | Current-state analysis pending |
| `prime-QA-TCMS/primeqa-common` | Shared/common | `main` | Current-state analysis pending |
| `prime-QA-TCMS/user-service` | Backend service | `main` | Current-state analysis pending |
| `prime-QA-TCMS/project-service` | Backend service | `main` | Current-state analysis pending |
| `prime-QA-TCMS/testcase-service` | Backend service | `main` | Current-state analysis pending |
| `prime-QA-TCMS/results-service` | Backend service | `main` | Current-state analysis pending |
| `prime-QA-TCMS/configuration-service` | Backend service | `main` | Current-state analysis pending |
| `prime-QA-TCMS/fog-ui` | UI/component library | `main` | Current-state analysis pending |
| `prime-QA-TCMS/primeqa-project` | Project control plane | `main` | Programme source of truth |
| `prime-QA-TCMS/primeqa-qa-ui-engine` | QA UI execution engine | `main` | Generic Playwright browser primitives; no business scenarios |
| `prime-QA-TCMS/primeqa-qa-api-engine` | QA API execution engine | `main` | Generic API primitives; no business scenarios |
| `prime-QA-TCMS/primeqa-qa-test-automation` | QA test orchestration | `main` | TCMS scenarios, cross-layer execution and reporting |

## QA automation dependency model

`primeqa-qa-test-automation` depends on both QA engines. The engines must remain independent of each other and must not own Prime QA business test cases.

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
