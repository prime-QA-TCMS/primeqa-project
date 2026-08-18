# Prime QA TCMS — Technical Requirements Baseline

Status: **Owner-approved baseline**  
Last updated: **2026-08-18**

This document is the authoritative consolidated technical/product-architecture baseline for Prime QA TCMS. Requirements below reflect explicit owner decisions unless marked for analysis. Agents must not weaken or reinterpret them silently.

## 1. Product and access model

### TR-ROLE-001 — Default roles
Prime QA ships with five default roles:

- **Admin** — instance-level full access; may perform any system action and manage instance configuration.
- **Lead QA** — may create and fully manage projects within granted scope.
- **Tester** — may manage test results and comment on test cases and results.
- **Developer** — read-only access to relevant project results and may comment on results.
- **Stakeholder** — report-view access to relevant projects.

Detailed permission matrices must preserve these defaults and apply server-side authorization.

### TR-ROLE-002 — Hierarchical governance
Where a capability supports hierarchy, authority flows from **Instance Admin → Tenant (when enabled) → Project Lead**. Lower scopes may configure only within constraints permitted by the parent scope.

## 2. Service architecture

### TR-ARCH-001 — Strict separation of responsibility
Backend capabilities must use independently deployable services with complete division of responsibility. Existing services are:

- user
- project
- testcase
- results
- configuration

Dedicated bounded services are also required for:

- reporting
- audit/history
- notifications
- integrations
- attachments/files
- import/export

Additional boundaries may be proposed only where analysis demonstrates a distinct responsibility.

### TR-ARCH-002 — Shared backend standardization
`primeqa-common` / `prime-qa-api-common` is the mandatory common backend standardization package. It owns reusable platform concerns including:

- error/exception model
- response envelopes
- validation helpers/schemas
- authentication/JWT utilities
- common middleware
- logging and correlation IDs
- request context
- environment/config loading
- health/readiness standards
- pagination/filter/sort utilities
- shared DTOs/types
- security middleware/defaults
- retry/idempotency helpers
- database connection abstractions/helpers

Domain-specific business logic remains in its owning service.

### TR-ARCH-003 — Shared frontend component platform
`fog-ui` owns **all UI components** and frontend component standards. `primeQA-UI` composes those components into TCMS pages, routing, state and business workflows.

Every `fog-ui` component must be:

- automation-ready with stable semantic/test interfaces
- accessible
- theme/design-token compatible
- documented in an easy-to-read component catalogue
- accompanied by practical how-to guides/examples
- independently covered by its own unit/component tests

The catalogue must make component purpose, states, variants, usage, accessibility behaviour and automation hooks easy to understand.

### TR-ARCH-004 — UI tooling changes are evidence-driven
The main UI must not be migrated from `react-scripts` to Vite merely for preference. Technical analysis should assess migration risk, dependency health, maintainability, performance and `fog-ui` compatibility and propose migration only when justified.

## 3. API architecture

### TR-API-001 — REST with optional GraphQL
REST is the default API style. GraphQL is an optional platform-wide capability controlled by instance/master configuration. The precise configuration mechanism must be finalized by architecture/configuration analysis.

### TR-API-002 — Mandatory API versioning and standardization
All APIs must be explicitly versioned and standardized across services through `primeqa-common`, including contracts, errors, validation, pagination, filtering, sorting, authentication/security handling, logging/correlation, health/readiness and common DTO conventions.

### TR-API-003 — Resilience and idempotency
APIs must tolerate transient environment/dependency downtime and retry safely. Retry-sensitive writes must be idempotent and must not create duplicate records because a client or integration retried after a timeout/outage.

### TR-API-004 — Security
APIs must use strict authentication/authorization, boundary validation, secure defaults, rate/abuse controls where appropriate, secure headers, secret handling, auditability, dependency security and protections against relevant OWASP web/API attack classes.

### TR-API-005 — Contracts and documentation
Contracts must be explicit, typed where practical, machine/human readable and regression tested. OpenAPI/Swagger remains the REST documentation baseline unless superseded by an approved ADR.

## 4. Authentication and tenancy

### TR-AUTH-001 — Configurable authentication methods
Prime QA must support combinations of local credentials and enterprise identity methods including SSO/OAuth/OIDC and LDAP/Active Directory. Enabled methods are instance configuration managed by Admin through the UI. Multiple methods may coexist. MFA must be supported where the selected identity mechanism allows it.

Identity linking, token/session lifecycle, revocation and provider-disable behaviour require detailed security design but must conform to the configured role/permission model.

### TR-TEN-001 — Optional client tenancy
Tenancy is an optional feature enabled/disabled by Instance Admin. Its primary use case is freelance QA managing different clients.

When enabled:

- a tenant represents a client workspace;
- each tenant supports multiple projects;
- tenant data must be strictly isolated from other tenants;
- tenant-level configuration is subordinate to instance configuration.

When disabled, the instance behaves as a single workspace without forcing tenant concepts into user workflows.

## 5. Configuration and versioning

### TR-CONF-001 — Hierarchical configuration
Configuration follows **Instance → Tenant → Project** where applicable. Lower scopes cannot exceed restrictions imposed above them.

### TR-CONF-002 — Configuration history feature
Configuration/rule version history is an optional feature toggled by Instance Admin. When enabled it must:

- retain the current version plus up to three prior versions (owner wording implies three historical versions; confirm during implementation);
- record who changed it and when;
- expose historical versions read-only;
- provide side-by-side comparison;
- allow authorized revert;
- record a revert as a new change rather than rewriting history.

## 6. Data lifecycle

### TR-DATA-001 — Configurable retention
Retention is configured by Instance Admin rather than hard-coded. Tenant retention may be configured where tenancy is enabled but **may never exceed instance-level limits**.

The retention model has three distinct policy classes:

1. **Archive rule** — when eligible live data is archived.
2. **Permanent-delete rule** — when eligible data is irreversibly deleted.
3. **Attachment rule** — lifecycle for file attachments.

Policies should support data-class-specific treatment where necessary.

### TR-DATA-002 — External archive provider required
Archive data is pushed out of the live data store to an Admin-configured external storage provider such as OneDrive or another supported cloud/object-storage provider. If no valid archive destination is configured, archival is unavailable.

An archive operation must preserve traceability/restoration metadata. Live data must never be removed as a consequence of an archive operation until successful durable archival has been verified. Failed archive operations must not cause data loss.

### TR-DATA-003 — Deletion
Deletion is soft-delete by default. Authorized users may explicitly hard-delete where permissions permit. Instance Admin configures automatic permanent purging of aged soft-deleted records.

Deletion/purge must preserve required historical/reference integrity and must not make retained execution history uninterpretable.

### TR-DATA-004 — Document database abstraction
Persistence must support MongoDB and Mongo-compatible/document-oriented databases such as Azure Cosmos DB where technically compatible. Database connection concerns should be abstracted through the common backend layer rather than hard-coded into domains.

## 7. Self-hosting and deployment

### TR-DEP-001 — Unlimited self-hosted users
Self-hosted Prime QA has no software-imposed user-count limit.

A future managed/hosted offering may enforce tier-based user limits controlled by the hosting provider. Hosted licensing/tier enforcement is **out of scope for the initial project** and must not constrain the self-hosted core.

### TR-DEP-002 — Deploy-to-server experience
GitHub distribution should provide a **Deploy to my server** flow/button where the operator supplies deployment details.

### TR-DEP-003 — First-run database configuration
If Prime QA cannot connect to a valid document database on initial run, it must provide an Admin configuration wizard to configure and test the database connection before the instance becomes operational. Credentials must be handled securely and never exposed client-side.

### TR-DEP-004 — Independent deployment plus Deploy All
Every backend service remains independently deployable and independently scalable. Deployment tooling must also provide a **Deploy All** option that orchestrates the complete stack without turning it into a monolithic process.

## 8. Reporting

### TR-REP-001 — ISTQB-aligned reporting
Default reporting/scorecards should be derived from appropriate ISTQB test-reporting principles/standards. The exact default metric/report catalogue should be established by QA/business analysis and documented rather than guessed.

### TR-REP-002 — Custom reports
Users must be able to define custom reports within their permitted scope.

### TR-REP-003 — Report access hierarchy
Reporting access/configuration follows **Instance Admin → Tenant (if applicable) → Project Lead**. Lower levels cannot grant access prohibited by the parent level.

## 9. Test design and execution model

### TR-TEST-001 — Master planning hierarchy
The master test-design hierarchy is:

**Project → Suite → Section (optional) → Test Case**

Suites and test cases are planning/master entities. There is one logical current instance of each plus version history; executions reference them rather than duplicating master definitions.

### TR-TEST-002 — Test-case versioning
Test cases are versioned. The system must record who changed them and when, support read-only historical viewing, side-by-side comparison and authorized revert. Revert creates a new version rather than rewriting history.

Every execution/result must permanently reference the exact test-case version executed. The number of historical test-case versions retained remains to be confirmed.

### TR-TEST-003 — Run model
A Run belongs to a Project and executes a **single Suite** (including its optional Sections and selected Test Cases). A Run has a custom name that defaults to the Suite name.

A whole Run may be assigned to a user.

### TR-TEST-004 — Plan model
A Plan belongs to a Project, must have a custom name, and may contain multiple Suite execution configurations.

A Plan may include the **same master Suite multiple times**. Each inclusion is an execution configuration/reference, not a copied Suite. Each suite inclusion may:

- carry optional execution configuration/context such as device, browser, environment, build/version or extensible parameters;
- include all or a selected subset of Test Cases;
- have a user assignment;
- independently configure the same Suite differently from another inclusion.

Example: the same Suite may appear as `Suite A (iPhone 11)` and `Suite A (Samsung Galaxy)` in one Plan.

### TR-TEST-005 — Hierarchical execution assignment
Assignment can occur at:

- whole Run;
- whole Plan;
- Suite execution within a Plan;
- Section within a Suite execution;
- individual Test Case within a Suite execution.

More-specific assignment overrides broader assignment for that execution scope.

### TR-TEST-006 — Results preserve execution context
Results must preserve enough context to identify the Project, Run/Plan, Suite execution configuration, optional Section, Test Case, exact Test Case version, assignee where relevant and execution configuration/environment.

## 10. External automation

### TR-AUTO-001 — Admin-controlled integration
External automation/result ingestion is enabled and configured by Instance Admin.

### TR-AUTO-002 — Non-unique Automation ID
Automation ID is an Admin-configured concept and is intentionally **not unique**. Multiple TCMS Test Cases may share one Automation ID because one automated test may cover multiple assertions/cases.

### TR-AUTO-003 — Configurable creation and field mapping
When automation integration is enabled, Instance Admin controls:

- whether automation may create its own execution data/context (including supported Runs/Plans/context records);
- which incoming automation fields may update which Prime QA fields;
- the permitted scope of automated writes.

External systems cannot grant themselves additional write capability.

### TR-AUTO-004 — Shared-ID result propagation
When one Automation ID maps to multiple Test Cases:

- a **pass** marks all matching cases as Passed;
- for **failure/warning**, Instance Admin configures the propagation policy, e.g. fail all or set all to Review;
- ingestion must remain idempotent so retries do not duplicate results/executions.

## 11. External integrations

### TR-INT-001 — Required integration families
The dedicated integration service must support an adapter-based architecture for at least:

- Jira
- Azure Boards
- GitHub Projects/Boards
- GitHub
- GitLab
- Bitbucket
- CI/CD systems
- generic webhooks
- Slack
- Microsoft Teams
- email notifications

Provider-specific logic should remain isolated from core domain services.

## 12. Import/export and portability

### TR-PORT-001 — Permissions
- **Instance Admin:** may import/export everything, including system/instance configuration.
- **Project Lead:** may import/export everything within projects they manage.
- **Tester, Developer, Stakeholder:** no import/export capability by default.

### TR-PORT-002 — Standard formats only
Supported interchange mechanisms are standard formats/protocols only:

- CSV
- Excel
- JSON
- XML
- API-based transfer

No proprietary Prime QA package/archive format is required.

## 13. Attachments

### TR-ATT-001 — Any file format
Attachments may be any file format. Security controls may still apply file-size, malware scanning, safe-content handling and authorization rules; these are security controls, not business-format restrictions.

### TR-ATT-002 — Configurable storage backend
Instance Admin selects the attachment storage backend. Supported models include:

- document database storage;
- external cloud/object storage;
- OneDrive;
- SharePoint.

The attachment service abstracts the backend from consuming domains. Attachment lifecycle follows the configured attachment retention policy.

## 14. Audit

### TR-AUD-001 — Comprehensive audit trail
Prime QA audits all material system activity, including projects, test cases, test results, version reverts, tenants, permissions, configuration, imports/exports, integrations, archive/delete operations and administrative actions.

Audit events must record at minimum:

- who performed the action;
- what action occurred;
- when it occurred;
- affected entity/context;
- relevant before/after state where applicable.

Audit records are immutable to normal users and subject to configured retention policy.

Diagnostic/application logging and audit history are separate concerns; disabling ordinary logging must not silently disable mandatory auditability.

## 15. Non-functional requirements

### TR-NFR-001 — Scale targets
Design targets per instance:

- users: **1–10,000**
- tenants: **0–100**
- projects: **1,000+**
- test cases: **100,000+**
- results: **1,000,000+**
- concurrent users: **1–5,000**

### TR-NFR-002 — Performance
Normal API operations should target approximately **≤200 ms** response time. Responses approaching **1,000 ms** are acceptable only for genuinely large/complex returns. 1,000 ms is the intended upper bound for those large responses under supported operating conditions; detailed load-test acceptance profiles must be established from these targets.

### TR-NFR-003 — Availability
Target uptime is **99%**.

### TR-NFR-004 — Backup/recovery
Backup and recovery behaviour is configurable by Instance Admin. Technical design must define supported mechanisms and safe defaults.

### TR-NFR-005 — Browser/device support
The UI must support Chromium-family browsers, Firefox, Opera, embedded/in-app browsers where technically feasible, and responsive/mobile-friendly use.

### TR-NFR-006 — Accessibility
Accessibility must be implemented comprehensively wherever technically possible. Analysis must translate this into measurable conformance criteria (e.g. an appropriate WCAG target) rather than leaving acceptance subjective.

### TR-NFR-007 — Security posture
The product must pursue the strongest reasonably achievable security posture. Security analysis must translate this into explicit standards, threat controls, verification and release gates rather than relying on the phrase alone.

### TR-NFR-008 — Configurable logging
Instance Admin can configure diagnostic/application logging from very verbose/system-wide logging through selected categories/severity levels to no diagnostic logging. Mandatory audit/security records are governed separately and must not be disabled merely by setting application logging to none.

## 16. QA automation architecture

### TR-QA-001 — QA repositories
Automation is separated into:

- `primeqa-qa-ui-engine` — generic UI/browser execution engine;
- `primeqa-qa-api-engine` — generic API execution engine;
- `primeqa-qa-test-automation` — TCMS business scenarios, orchestration, assertions, data and reporting.

The two engines remain independent. The central runner depends on both.

### TR-QA-002 — Cross-layer verification
The central runner must support UI→API and API→UI validation, shared execution state, cross-service workflows and result correlation.

### TR-QA-003 — Quality gates
Repositories require appropriate unit/component/API/integration/UI automation, build/type/lint validation, security/dependency controls and traceable CI evidence.

## 17. Analysis-required item

### TR-AN-001 — Mapping/reference data
The previous concept of “mapping/reference data” is **not yet confirmed as a product requirement**. It must be investigated by four lenses before implementation:

1. **Business Analyst** — identify whether a real business workflow requires mapping/reference entities.
2. **Architect** — determine whether mappings are needed for integrations, service boundaries, historical references, automation or integrity.
3. **Technical Analyst** — inspect current schemas, APIs and code to determine what mapping currently means/does.
4. **End-user/Product Analyst** — determine whether users need explicit mapping functionality and what problem it solves.

Output must contain evidence, recommendation and unresolved questions. Do not preserve or implement mapping solely because it appeared in earlier retention discussions.

## 18. Governance

### TR-GOV-001 — Source of truth
`primeqa-project` is the authoritative cross-repository source for approved programme requirements and architecture.

### TR-GOV-002 — Evidence before completion
Requirements/objectives must map to implementation and validation evidence before being marked complete.

### TR-GOV-003 — Unknowns remain explicit
Implementation agents must not invent unresolved product behaviour. Remaining uncertainties are tracked in `requirements/open-questions.md` and resolved by owner decision, evidence or approved ADR as appropriate.
