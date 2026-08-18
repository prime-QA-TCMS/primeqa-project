# Prime QA TCMS — Project Control Plane

This repository is the central source of truth for the Prime QA TCMS programme. It coordinates work across the product repositories without containing production application code.

## Purpose

Use this repository for:

- product and business requirements
- target-user analysis
- system architecture and technical decisions
- cross-repository dependency mapping
- delivery roadmap and objectives
- QA strategy and automation planning
- Codex/agent operating instructions
- project status and daily reports
- risks, assumptions, decisions, and open questions
- automated AI-SDLC lifecycle rules and evidence gates

## Authoritative technical baseline

Before making cross-repository technical changes, read:

- `requirements/technical-requirements.md` — consolidated owner-approved technical requirements and architectural standards
- `requirements/open-questions.md` — remaining matters that require evidence/analysis and must not be guessed
- `registry/repositories.md` — repository roles and dependency boundaries
- `architecture/qa-automation.md` — QA automation engine/orchestration architecture
- `AGENTS.md` — global operating rules
- `ai-sdlc/README.md` — automated Codex/AI SDLC operating specification

## Product repositories

| Repository | Responsibility |
| --- | --- |
| `primeQA-UI` | Main React/TypeScript TCMS frontend application |
| `fog-ui` | Shared/custom UI component and primitive platform |
| `primeqa-common` | Shared backend/API utilities, middleware, types and platform standards (`prime-qa-api-common`) |
| `user-service` | User and authentication capabilities |
| `project-service` | Project management capabilities |
| `testcase-service` | Test case management capabilities |
| `results-service` | Test execution/results capabilities |
| `configuration-service` | TCMS configuration capabilities |

Additional approved service boundaries for reporting, audit/history, notifications, integrations, attachments/files and import/export are documented in the technical baseline.

## QA automation repositories

| Repository | Responsibility |
| --- | --- |
| `primeqa-qa-ui-engine` | Generic browser/UI automation engine |
| `primeqa-qa-api-engine` | Generic API automation engine |
| `primeqa-qa-test-automation` | TCMS business test scripting, cross-layer orchestration and reporting |

The QA engines remain independent. Business scenarios live in the central automation repository, which can pass state/results between UI and API execution within one scenario.

## AI-SDLC operating model

The `ai-sdlc/` directory defines the automated engineering lifecycle:

- `README.md` — lifecycle overview and core principles
- `agents.md` — specialist agent roles and hand-off contracts
- `state-machine.md` — canonical lifecycle states and failure routing
- `work-item-schema.md` — machine-readable Requirement → Work → Evidence structure
- `definition-of-done.md` — mandatory completion evidence and gates
- `permissions-and-gates.md` — autonomous permissions, human approvals and escalation
- `branching-and-prs.md` — branch, PR and cross-repository change strategy
- `orchestration.md` — work selection, dispatch, dependency, retry and reporting rules

The implementation agent is never the final authority on its own change. Independent code review, applicable QA/security/NFR verification and objective auditing are separate stages.

## Control-plane structure

- `requirements/` — approved technical/product requirements and unresolved questions
- `project/` — vision, objectives, scope, roadmap and status
- `analysis/` — business, user, technical and gap analysis
- `architecture/` — system architecture and ADRs
- `qa/` — QA strategy and automation plans
- `ai-sdlc/` — automated engineering operating model
- `delivery/` — workstreams, dependencies, risks and execution tracking
- `reports/` — daily and periodic project reports
- `registry/` — repository and system inventory

## Operating principle

Analysis comes before autonomous implementation. Agents must establish the current state, distinguish evidence from assumptions, identify gaps against agreed objectives, decompose work into traceable items, implement through branches/PRs, validate independently, and attach evidence before declaring work complete.

Open questions are deliberately preserved as open questions. Existing code may provide evidence, but agents must not silently turn ambiguous historical direction into new requirements.
