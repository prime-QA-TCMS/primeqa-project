# Prime QA TCMS — Technical Requirements Baseline

Status: **Consolidated baseline**  
Last updated: **2026-08-18**

This document consolidates technical requirements and architectural decisions previously established for Prime QA TCMS. It distinguishes confirmed requirements from implementation details that still require validation. Where prior direction is incomplete, the requirement links conceptually to `requirements/open-questions.md` rather than inventing a decision.

## 1. System intent

Prime QA is a modular Test Case Management System (TCMS) intended to support test-management workflows while remaining maintainable, extensible, automatable, and suitable for self-hosted use.

The system must be designed so that UI, API services, shared libraries, and QA automation remain independently maintainable while conforming to common standards.

## 2. Architectural style

### TR-ARCH-001 — Modular service architecture

Backend capabilities are separated into independently maintained services rather than implemented as one monolithic backend.

Current service boundaries include:

- `user-service`
- `project-service`
- `testcase-service`
- `results-service`
- `configuration-service`

Service responsibilities must remain explicit. Cross-service logic must not be duplicated casually when it belongs in a shared contract or common utility.

### TR-ARCH-002 — Shared API standardization package

All backend services must use `primeqa-common` / the published `prime-qa-api-common` package for reusable API standards where applicable.

Its architectural purpose is to centralize reusable backend concerns such as:

- shared types and contracts
- middleware
- validation helpers
- authentication/authorization helpers where common
- HTTP/API response conventions where common
- error-handling conventions
- logging/request middleware where common
- security middleware/configuration where common

A service should not create a competing local implementation of a concern already standardized by the shared package unless there is a documented reason.

Changes to shared utilities are cross-service changes and require compatibility assessment.

### TR-ARCH-003 — Shared UI component library

Reusable UI primitives and custom components must live in the separate `fog-ui` repository/package rather than being duplicated across the application UI.

`primeQA-UI` is the product application. `fog-ui` is the reusable component/design primitive layer.

New reusable components should be implemented in `fog-ui` when they are sufficiently generic. Product-specific composition and business workflows remain in `primeQA-UI`.

Changes to `fog-ui` must consider backwards compatibility for the consuming UI.

### TR-ARCH-004 — Shared code must have clear ownership boundaries

`primeqa-common` is for backend/API standardization. `fog-ui` is for frontend component standardization. Neither repository should become a general dumping ground for unrelated product business logic.

## 3. Technology baseline

### TR-TECH-001 — Primary language

TypeScript is the primary implementation language for the UI, backend services, shared libraries, and QA automation unless a documented architectural decision explicitly justifies an exception.

### TR-TECH-002 — Frontend

The current frontend baseline is React with TypeScript.

The application currently uses a modern React ecosystem including routing, Redux Toolkit, React Hook Form, i18n, schema validation, charting, and reusable components through `fog-ui`.

Existing implementation choices should be preserved unless current-state analysis identifies a material reason to migrate them.

### TR-TECH-003 — Backend

Backend services use Node.js, TypeScript, Express, and MongoDB through Mongoose as the current architectural baseline.

Services expose HTTP APIs and must maintain consistent contracts and behaviour using the shared API package where applicable.

### TR-TECH-004 — API documentation

Service APIs should expose or maintain machine/human-readable API documentation. The current implementation includes Swagger/OpenAPI tooling and this should be treated as the baseline unless superseded by an ADR.

### TR-TECH-005 — Containerized/self-hosted operation

The product must support self-hosted deployment. Backend repositories already show container/Docker-oriented execution patterns and the completed system should have a documented repeatable deployment model.

Deployment must not require hidden developer-machine state.

## 4. Repository and dependency boundaries

### TR-REP-001 — Product repositories

The system currently consists of:

- `primeQA-UI` — main frontend application
- `primeqa-common` — shared backend/API utilities, middleware and types
- `fog-ui` — reusable frontend component library
- `user-service` — user/authentication domain
- `project-service` — project domain
- `testcase-service` — test-case domain
- `results-service` — execution/results domain
- `configuration-service` — configuration domain
- `primeqa-project` — programme control plane and source of approved cross-repository requirements/architecture

### TR-REP-002 — QA automation repositories

QA automation is intentionally separated into three repositories:

- `primeqa-qa-ui-engine` — reusable browser/UI execution engine
- `primeqa-qa-api-engine` — reusable API execution engine
- `primeqa-qa-test-automation` — TCMS test cases, business flows, orchestration, assertions and reporting

The two engines must remain independent from one another. The central test-automation repository depends on both.

### TR-REP-003 — Business scenarios do not belong in engines

The UI and API QA engines must provide generic execution primitives. TCMS-specific test scenarios, workflows, data builders, assertions, and cross-layer state live in `primeqa-qa-test-automation`.

## 5. API requirements

### TR-API-001 — Consistency across services

All APIs should follow shared conventions for request validation, authentication, error responses, status handling, logging and common headers where those concerns are standardized by `primeqa-common`.

### TR-API-002 — Explicit contracts

API request and response contracts must be explicit, typed where possible, documented, and regression tested.

Breaking contract changes must be deliberate and assessed across all consumers, including the UI and QA automation.

### TR-API-003 — Validation and error handling

Invalid input must fail predictably with standardized validation/error behaviour. Services must not silently accept malformed or ambiguous input.

### TR-API-004 — Authorization enforcement

Authorization must be enforced by the API, not only hidden or disabled in the UI.

### TR-API-005 — Cross-service integrity

Where a workflow spans multiple services, IDs, ownership boundaries, lifecycle states and failure behaviour must be explicit. Cross-service consistency must be covered by integration/API automation.

## 6. Frontend requirements

### TR-UI-001 — Reuse over duplication

The application UI should consume `fog-ui` for reusable custom components and primitives instead of implementing visually/functionally equivalent components repeatedly in `primeQA-UI`.

### TR-UI-002 — Product logic stays in the application

Business-specific pages, workflows, state orchestration, permissions, and product behaviour belong in `primeQA-UI`, even when composed from `fog-ui` components.

### TR-UI-003 — Stable automation interfaces

Critical interactive elements must expose stable selectors/semantics suitable for automated testing. QA automation must not depend primarily on fragile CSS structure or visible text where a stable semantic/test identifier is appropriate.

### TR-UI-004 — Internationalization baseline

The existing UI includes i18n support. New UI work should not hard-code architecture that prevents localization.

## 7. Data and persistence requirements

### TR-DATA-001 — MongoDB baseline

MongoDB/Mongoose is the current persistence baseline for services. Data ownership should follow service boundaries rather than allowing uncontrolled direct access to another service's internal persistence model.

### TR-DATA-002 — Tenant-aware data

Tenant/customer-scoped data must remain attributable to the correct tenant and must not leak across tenant boundaries.

The exact tenancy implementation and isolation model still require explicit verification/documentation.

### TR-DATA-003 — Data retention lifecycle

The previously established retention direction is:

1. Active operational data is retained for approximately **3 years** before archival where the data category is subject to the standard lifecycle.
2. Archived data is retained for approximately **10 additional years**, after which it is deleted where the standard lifecycle applies.
3. Some high-volume or low-value failure/transient data should have a **shorter retention period** rather than inheriting the full standard lifecycle.
4. Retention must distinguish data classes rather than apply one blanket policy to all records.

Retention design must explicitly account for at least:

- tenant/customer data
- test execution/results data
- failure/transient diagnostic data
- scorecard/quality measurement data
- rule/configuration data
- mapping/reference data

Exact retention periods for the non-standard classes remain open until documented in the retention matrix.

### TR-DATA-004 — Archive is not delete

Archival must preserve required historical usability/traceability. Deletion is a separate terminal lifecycle action.

### TR-DATA-005 — Referential integrity across lifecycle changes

Archival or deletion of a record must not silently corrupt retained data that references it. Required mappings, historical labels/snapshots, or tombstone/reference strategies must be defined per data domain.

## 8. Authentication, authorization and users

### TR-AUTH-001 — Dedicated user capability

User and authentication concerns belong primarily in `user-service`, with common reusable auth helpers standardized through the shared backend package where appropriate.

### TR-AUTH-002 — Role/permission behaviour must be testable

Permissions and role restrictions must have API-level enforcement and must be covered by automated tests at the appropriate API and UI layers.

### TR-AUTH-003 — Self-hosted free-tier user limit

A prior product direction is that self-hosted use is free for installations with **up to 5 users**.

The mechanism for enforcing, licensing, or merely defining this commercial boundary has not yet been technically specified and must not be invented by implementation agents.

## 9. Quality and automation requirements

### TR-QA-001 — Quality is part of delivery

QA is integrated throughout implementation. A change is not considered complete merely because code compiles or a manual happy path works.

### TR-QA-002 — Layered automation

The project should maintain appropriate coverage across:

- unit/component tests
- service/API tests
- cross-service integration tests
- UI/E2E tests
- cross-layer tests that validate UI-created data through APIs and API-created data through the UI
- appropriate non-functional tests

### TR-QA-003 — Cross-layer verification

The automation architecture must support scenarios such as:

- create via UI, validate via API
- create via API, validate via UI
- mutate via one layer, verify state through another
- establish test state efficiently through API and validate user-visible behaviour in UI

State/results may flow between the UI and API engines through the central test runner's execution context.

### TR-QA-004 — Test engines remain generic

Neither engine should understand TCMS business workflows. Business-level clients, page/workflow abstractions, test data and assertions are owned by `primeqa-qa-test-automation`.

### TR-QA-005 — Deterministic tests

Automation should use isolated/unique test data where appropriate, avoid arbitrary sleeps, prefer stable selectors, produce diagnostic failure artifacts, and support smoke/regression/cross-layer classifications.

### TR-QA-006 — CI quality gates

Repositories should have automated validation appropriate to their type, including build/type checks, tests, linting and relevant security/dependency controls.

## 10. Maintainability and code quality

### TR-MNT-001 — Avoid duplicated platform standards

Common backend concerns belong in `primeqa-common`; reusable UI components belong in `fog-ui`. Product repositories should consume these shared packages rather than fork copies.

### TR-MNT-002 — Backwards compatibility is a cross-repository concern

Changes to shared packages, API contracts, shared models, authentication behaviour, or reusable UI components require analysis of all consumers.

### TR-MNT-003 — Architectural changes require rationale

Agents/developers must not rewrite working architecture purely based on preference. Material changes require evidence of a requirement, defect, risk, scalability need, security issue, or maintainability benefit and should be recorded through an ADR where appropriate.

### TR-MNT-004 — Documentation follows behaviour

Public contracts, deployment procedures, environment variables, architecture decisions and operational expectations must remain synchronized with implementation.

## 11. Security and privacy baseline

### TR-SEC-001 — No secrets in source

Credentials, tokens, production data and secrets must not be committed to repositories.

### TR-SEC-002 — Standardized service security

Reusable API security middleware/configuration should be centralized in the shared API package where practical to reduce inconsistent protection between services.

### TR-SEC-003 — Input and access boundaries

All externally supplied data must be validated at appropriate boundaries, and authorization must be checked server-side.

### TR-SEC-004 — Tenant isolation

Tenant data must not be accessible to another tenant through UI, API, identifier manipulation, search, reporting, exports, or automation/setup endpoints.

## 12. Self-hosting and commercial boundary

### TR-DEP-001 — Self-hostable product

Prime QA must remain deployable by a customer/operator outside a centrally managed Prime QA SaaS environment.

### TR-DEP-002 — Configuration must be externalized

Environment-specific URLs, secrets, database connections, ports and runtime configuration should be provided through documented configuration/environment mechanisms rather than source modifications.

### TR-DEP-003 — Commercial support must not contaminate architecture

Previously discussed commercial options (paid setup and support) are service/commercial offerings. Core technical architecture should remain operable without requiring paid support.

## 13. Traceability and completion

### TR-GOV-001 — `primeqa-project` is the cross-repository source of truth

Approved programme-level requirements, architecture and decisions belong in the project control-plane repository.

### TR-GOV-002 — Evidence before completion

Requirements/objectives must be traceable to implementation and validation evidence before being declared complete.

### TR-GOV-003 — Unknowns remain explicit

Anything marked unclear/open must not be silently converted into an implementation decision by Codex or another agent. Open matters must either be resolved by evidence from the current system or presented for owner decision.
