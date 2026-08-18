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

## Authoritative technical baseline

Before making cross-repository technical changes, read:

- `requirements/technical-requirements.md` — consolidated technical requirements and architectural standards
- `requirements/open-questions.md` — unresolved matters that must not be guessed by agents
- `registry/repositories.md` — repository roles and dependency boundaries
- `architecture/qa-automation.md` — QA automation engine/orchestration architecture
- `AGENTS.md` — global operating rules

## Product repositories

| Repository | Responsibility |
| --- | --- |
| `primeQA-UI` | Main React/TypeScript TCMS frontend application |
| `fog-ui` | Shared/custom UI component and primitive library |
| `primeqa-common` | Shared backend/API utilities, middleware and types (`prime-qa-api-common`) |
| `user-service` | User and authentication capabilities |
| `project-service` | Project management capabilities |
| `testcase-service` | Test case management capabilities |
| `results-service` | Test execution/results capabilities |
| `configuration-service` | TCMS configuration capabilities |

## QA automation repositories

| Repository | Responsibility |
| --- | --- |
| `primeqa-qa-ui-engine` | Generic browser/UI automation engine |
| `primeqa-qa-api-engine` | Generic API automation engine |
| `primeqa-qa-test-automation` | TCMS business test scripting, cross-layer orchestration and reporting |

The QA engines remain independent. Business scenarios live in the central automation repository, which can pass state/results between UI and API execution within one scenario.

## Control-plane structure

- `requirements/` — approved technical/product requirements and unresolved questions
- `project/` — vision, objectives, scope, roadmap and status
- `analysis/` — business, user, technical and gap analysis
- `architecture/` — system architecture and ADRs
- `qa/` — QA strategy and automation plans
- `delivery/` — workstreams, dependencies, risks and execution tracking
- `reports/` — daily and periodic project reports
- `registry/` — repository and system inventory

## Operating principle

Analysis comes before autonomous implementation. Agents must establish the current state, distinguish evidence from assumptions, identify gaps against agreed objectives, propose an execution plan, and make changes through traceable branches and pull requests in the relevant repositories.

Open questions are deliberately preserved as open questions. Existing code may provide evidence, but agents must not silently turn ambiguous historical direction into new requirements.
