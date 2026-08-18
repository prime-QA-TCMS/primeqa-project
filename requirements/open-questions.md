# Prime QA TCMS — Open Technical Questions

Status: **Owner/evidence decisions required**  
Last updated: **2026-08-18**

This file records matters that are not sufficiently clear to implement safely. Agents must not invent answers.

## Priority open questions

### OQ-001 — Final target product scope

We have an existing implementation and architectural direction, but the authoritative final capability list and acceptance boundaries are not yet consolidated.

Need to establish:

- required end-user roles/personas
- required workflows per role
- required TCMS capabilities for first complete release
- explicitly out-of-scope features
- acceptance criteria for programme completion

### OQ-002 — Service boundary completeness

Current services are user, project, testcase, results and configuration. It remains unclear whether the intended target needs additional bounded services (for example reporting, notifications, integrations, audit, attachments, imports/exports) or whether those responsibilities belong inside existing services.

Decision should follow current-state analysis rather than creating services pre-emptively.

### OQ-003 — API style and versioning policy

HTTP/REST is clearly present. Prior skills/context also referenced GraphQL generally, but it is not clear that GraphQL is an approved TCMS requirement.

Need explicit decisions on:

- REST-only vs any GraphQL requirement
- API versioning convention
- backwards-compatibility/deprecation policy
- shared response envelope/error schema
- pagination/filter/sort standards
- idempotency expectations for write operations

### OQ-004 — Authentication and authorization model

The user service uses JWT-related tooling, but the target authorization model is not yet fully documented.

Need to define:

- authentication flow(s)
- token/session lifecycle
- refresh/revocation behaviour
- role model
- project/tenant membership model
- permission granularity
- administrator capabilities
- service-to-service authentication, if required

### OQ-005 — Tenant model

Tenant-aware data is a requirement, but tenancy semantics are unclear.

Need to define:

- what constitutes a tenant
- whether a user can belong to multiple tenants
- tenant administrator model
- data partition/isolation strategy
- tenant deletion/export obligations
- tenant-specific configuration scope

### OQ-006 — Data retention matrix

The standard direction is approximately 3 years active plus 10 years archived, with shorter retention for some failure/transient data.

Exact values and lifecycle rules remain unclear for:

- execution results
- failed execution diagnostic payloads/logs
- audit/history records
- scorecard data
- rule/configuration history
- tenant data after tenant termination/deletion
- mappings/reference data
- attachments/export artefacts, if present

Need a formal matrix containing data class, active period, archive period, deletion trigger, legal/business rationale, restoration requirements, and reference-integrity behaviour.

### OQ-007 — Archive implementation

It is unclear whether archive means:

- same database with archived state/index strategy
- separate collections
- separate database/storage tier
- exported immutable files/object storage
- another archival mechanism

This affects query behaviour, cost, restore capability and deletion controls.

### OQ-008 — Five-user self-hosted free tier enforcement

The product direction states self-hosted use is free up to 5 users, but technical enforcement is not specified.

Need to decide whether this is:

- licence-enforced in software
- configuration-controlled
- honour/commercial-policy only
- enforced only for official distributions/support

Also clarify what counts as a user: active users, invited users, administrators, service accounts, disabled users, etc.

### OQ-009 — Packaging/distribution model

Self-hosting is required, but target distribution is unclear.

Need to define:

- Docker Compose as supported baseline or merely development tooling
- Kubernetes requirement, if any
- packaged release/version strategy
- database provisioning/migrations
- upgrade/rollback process
- minimum supported infrastructure
- backup/restore expectations

### OQ-010 — Deployment topologies

Need to decide whether all services must always deploy independently or whether an all-in-one self-hosted topology is supported/expected for small installations.

### OQ-011 — Shared API package scope

`primeqa-common` is confirmed as the common API standardization package, but the exact mandatory/common surface must be documented after inspection.

Need to determine which concerns are canonical there versus service-owned, including:

- error model
- API response model
- validation
- JWT/auth utilities
- middleware
- logging/correlation IDs
- request context
- configuration loading
- health/readiness endpoints
- pagination
- shared DTO/types

### OQ-012 — `fog-ui` governance

`fog-ui` is confirmed as the reusable/custom UI component package, but its component admission/versioning rules need definition.

Need decisions on:

- what qualifies as reusable enough for `fog-ui`
- semantic versioning expectations
- design tokens/theme ownership
- accessibility baseline
- visual regression/component documentation approach
- whether Storybook or equivalent is required

### OQ-013 — UI architecture modernization

The current UI is React/TypeScript and still uses `react-scripts` while `fog-ui` uses Vite. It is unclear whether migrating the main UI build tooling is a requirement or merely technical debt.

Do not migrate solely for preference.

### OQ-014 — Reporting/scorecard requirements

Prior discussions referenced scorecard/quality measurement data, but the exact product capability, calculations, sources, user workflows and retention requirements are unclear.

### OQ-015 — Rule/configuration model

Rule/configuration data has been identified as a distinct data class, but the system of record, versioning, audit history, evaluation behaviour and rollback requirements are unclear.

### OQ-016 — Mapping/reference data

Mapping data was called out separately in retention discussions. Need to define what entities are mapped, whether mappings are tenant-specific, and how historical results remain interpretable after mappings change.

### OQ-017 — Audit trail

It is unclear which user/admin actions require immutable or durable audit history. This is especially important for permissions, test changes, execution/results changes and configuration/rule changes.

### OQ-018 — Test case versioning

Need to establish whether test cases are mutable in place or versioned, and how historical executions reference the exact test definition used at execution time.

### OQ-019 — Test execution model

Need explicit definitions for:

- test runs/cycles/plans/suites (which entities are required)
- assignment/ownership
- execution status model
- step-level vs case-level results
- evidence/attachments
- reruns/retests
- environment/build/version metadata
- automated vs manual result ingestion

### OQ-020 — External automation/result ingestion

The intended mechanism for automated frameworks to submit/associate execution results with TCMS entities needs to be defined, including authentication, idempotency, mapping, partial results and retry behaviour.

### OQ-021 — Integration requirements

No authoritative requirement is currently captured for Jira, GitHub, CI systems, webhooks, email/chat notifications, identity providers or other external integrations.

### OQ-022 — Non-functional targets

Need measurable targets for:

- expected tenant/user/test-case/execution volumes
- API response/performance expectations
- concurrency
- availability
- recovery objectives
- browser support
- accessibility level
- security baseline
- observability

### OQ-023 — Data export/import and portability

Self-hosted customers may require import/export or migration capabilities, but exact requirements are unclear.

### OQ-024 — Attachments and object storage

It is unclear whether test cases/results support screenshots, files, videos or other evidence, and if so where they are stored and retained.

### OQ-025 — Deletion semantics

Need domain-specific rules for soft delete, hard delete, restore, cascade prevention, anonymization, and historical references.

## Resolution rule

An open question may be closed only by one of:

1. explicit project-owner decision;
2. authoritative existing requirement/product documentation;
3. clear current-system evidence where the intent is to preserve existing behaviour;
4. an approved ADR when the question is genuinely architectural rather than product-owned.

When resolved, update the technical requirements and/or architecture documents and record the evidence/decision source.
