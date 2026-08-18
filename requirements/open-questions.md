# Prime QA TCMS — Remaining Open Technical Questions

Status: **Major owner questions resolved; targeted analysis remains**  
Last updated: **2026-08-18**

The original 25-question technical clarification set has been consolidated into `requirements/technical-requirements.md`. This file now contains only matters that still require evidence, detailed design, or owner confirmation. Agents must not invent answers.

## Owner decisions now resolved

The following original areas have owner-approved direction and are no longer open at the product level:

- OQ-001 role model / principal product access model
- OQ-002 service boundary direction
- OQ-003 API style, versioning, standardization, resilience and security direction
- OQ-004 authentication-method configuration direction
- OQ-005 tenant purpose and optional-instance model
- OQ-006 hierarchical retention policy model
- OQ-007 external archive model
- OQ-008 self-hosted user-limit policy
- OQ-009 deployment/configuration experience direction
- OQ-010 independently deployable services + Deploy All
- OQ-011 mandatory `primeqa-common` scope
- OQ-012 `fog-ui` ownership/governance direction
- OQ-013 UI modernization decision method
- OQ-014 reporting direction
- OQ-015 configuration/rule versioning model
- OQ-017 comprehensive audit direction
- OQ-018 test-case versioning direction
- OQ-019 test-design, Plan, Run and assignment model
- OQ-020 external automation/result-ingestion model
- OQ-021 initial integration scope
- OQ-022 principal NFR targets
- OQ-023 import/export permissions and formats
- OQ-024 attachment format/storage direction
- OQ-025 deletion model

## Remaining questions / analysis assignments

### RQ-001 — Final release capability boundary
The role model and many technical capabilities are defined, but the authoritative **first complete release feature catalogue**, explicitly out-of-scope product features, and programme-level acceptance criteria still need consolidation from current implementation + product analysis.

**Required:** Business/Product analysis followed by owner confirmation where conflicts exist.

### RQ-002 — Detailed role/permission matrix
Default roles are defined, but granular permissions still require formalization, including:

- whether Lead QA manages project membership/roles;
- exact Tester result-edit scope;
- exact Developer visibility beyond results;
- exact Stakeholder report/dashboard visibility;
- whether users may belong to multiple tenants;
- whether a distinct tenant-admin role/permission set is required.

**Required:** Business Analyst + End-user/Product Analyst recommendation, constrained by the approved five default roles.

### RQ-003 — API protocol details
REST + optional globally configured GraphQL is approved. Detailed design remains for:

- concrete URL version convention;
- deprecation/backward-compatibility windows;
- canonical response/error schemas;
- pagination/filter/sort syntax;
- exact idempotency-key semantics;
- GraphQL exposure/versioning strategy.

**Required:** Architect + Technical Analyst; implement shared standards through `primeqa-common`.

### RQ-004 — Authentication/security lifecycle details
Supported authentication families are approved. Detailed design remains for:

- local credential identifier(s) and password policy;
- token/session lifetime and refresh/revocation;
- external identity linking;
- behaviour when an IdP is disabled;
- service-to-service authentication;
- MFA behaviour by provider;
- secure recovery/bootstrap flows.

**Required:** Security/Architecture analysis.

### RQ-005 — Tenant detailed semantics
Tenant purpose and optional enablement are approved. Still determine:

- multi-tenant membership for one user;
- tenant-admin capability model;
- exact tenant data partition/isolation implementation;
- tenant deletion/export workflow;
- which configuration categories are tenant-overridable.

**Required:** Business + Architecture analysis.

### RQ-006 — Retention defaults and data-class matrix
The configurable hierarchy and three policy classes are approved. Exact default values and category-specific rules remain to be defined for results, diagnostics, audit, reports/scorecards, configuration history, tenant data, attachments and other high-volume/transient data.

**Required:** Business/Compliance/Technical analysis and owner approval of defaults.

### RQ-007 — Archive provider contract and restoration
External configured archive storage is approved. Still define:

- supported provider interface/adapters;
- encryption/integrity requirements;
- archive package/data representation using standard formats;
- restoration workflow;
- verification before live-data removal;
- provider outage/retry behaviour.

**Required:** Architect + Technical Analyst.

### RQ-008 — Deployment implementation details
Deploy-to-server, database wizard, independent services and Deploy All are approved. Still define:

- concrete GitHub deployment mechanism;
- container/orchestrator baseline;
- release/version strategy;
- DB migrations/upgrades/rollback;
- minimum infrastructure;
- secrets management;
- Admin-configurable backup/recovery mechanisms.

**Required:** Architect/DevOps analysis.

### RQ-009 — `fog-ui` catalogue/tooling implementation
Component ownership, documentation, accessibility, automation-readiness and unit testing are approved. Select the implementation approach for the catalogue/how-to experience (e.g. Storybook or equivalent), visual regression strategy, semantic-versioning policy and measurable accessibility gates.

**Required:** Frontend Architect + QA analysis.

### RQ-010 — Configuration history count wording
Owner direction was “retain up to 3 versions” after discussion of historical versions. The baseline currently interprets this as **current + up to 3 previous versions**. Confirm during implementation before schema finalization.

### RQ-011 — ISTQB reporting catalogue
ISTQB-aligned default reporting plus custom reports is approved. Determine the concrete default metrics, calculations, report templates, data sources and trend/history views appropriate to the product.

**Required:** Business Analyst + QA/Test Management Analyst.

### RQ-012 — Mapping/reference data investigation
Do not assume this is a requirement. Investigate using four lenses:

- Business Analyst
- Architect
- Technical Analyst
- End-user/Product Analyst

Determine whether a real mapping/reference capability exists or is needed, what problem it solves, and how historical integrity should work. Return evidence + recommendation + unresolved questions.

### RQ-013 — Test-case version retention count
Test-case versioning, comparison, revert and exact-version execution references are approved. The number/lifecycle of historical test-case versions is not yet specified.

**Required:** Business/Product recommendation considering storage and audit requirements.

### RQ-014 — Detailed result/status model
Plan/Run structures are approved. Still define:

- canonical result statuses (including Review/Warning semantics);
- step-level versus case-level result representation;
- rerun/retest semantics;
- execution completion/locking rules;
- comments/evidence behaviour;
- exact environment/build/configuration metadata model.

**Required:** Business Analyst + QA/Test Management Analyst.

### RQ-015 — Automation ingestion contract
Admin-controlled automation creation, field mapping, non-unique Automation IDs and propagation rules are approved. Still define the wire/API contract, authentication, execution correlation, partial submissions, retry semantics and exact warning/failure policy options.

**Required:** Architect + QA Automation Analyst.

### RQ-016 — Integration adapter specifications
Required providers/families are approved. Define supported operations and authentication per provider, webhook model, sync direction, conflict/retry behaviour and minimum first-release provider coverage.

**Required:** Integration/Technical analysis.

### RQ-017 — Measurable accessibility/security standards
“Comprehensive accessibility” and strongest reasonably achievable security are approved directions. Convert these into testable standards and release gates (e.g. WCAG target, OWASP/API controls, dependency scanning, SAST/DAST expectations, secrets controls, threat modelling).

**Required:** Accessibility + Security + QA analysis.

### RQ-018 — Performance/load acceptance profile
Scale and response targets are approved. Define representative datasets, normal vs large-return definitions, percentile targets, concurrency ramps and test environments so the NFRs can be objectively verified.

**Required:** Performance QA + Architect.

### RQ-019 — Attachment security/storage implementation
Any file format and Admin-selected DB/cloud/OneDrive/SharePoint storage are approved. Define size limits, malware scanning, content-disposition/preview rules, provider adapters, encryption and migration behaviour when storage backend changes.

**Required:** Security + Architect + Technical Analyst.

### RQ-020 — Hard-delete authorization and reference rules
Soft-delete default, optional hard delete and Admin-configured automatic purge are approved. Define which roles may hard-delete each domain entity and when hard delete must be blocked/anonymized/snapshotted to preserve historical integrity.

**Required:** Business + Data Architecture analysis.

## Resolution rule

A remaining question may be closed by:

1. explicit project-owner decision;
2. authoritative product/compliance requirement;
3. clear current-system evidence where preserving behaviour is intended;
4. approved ADR for architectural decisions.

When resolved, update the authoritative technical baseline and record the evidence/decision source. Do not silently resolve uncertainties in implementation code.
