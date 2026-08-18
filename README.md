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

## Application repositories

| Repository | Responsibility |
| --- | --- |
| `primeQA-UI` | Main TCMS frontend |
| `primeqa-common` | Shared/common code |
| `user-service` | User and authentication capabilities |
| `project-service` | Project management capabilities |
| `testcase-service` | Test case management capabilities |
| `results-service` | Test execution/results capabilities |
| `configuration-service` | TCMS configuration capabilities |
| `fog-ui` | Shared UI/component library |

## Planned repositories

The programme still requires dedicated automation repositories for:

- Playwright end-to-end/UI automation
- API test automation

Their exact design and naming should be decided after current-state analysis.

## Control-plane structure

- `AGENTS.md` — global operating instructions for Codex/agents
- `project/` — vision, objectives, scope, roadmap, status
- `analysis/` — business, user, technical, and gap analysis
- `architecture/` — system architecture and ADRs
- `qa/` — QA strategy and automation plans
- `delivery/` — workstreams, dependencies, risks, and execution tracking
- `reports/` — daily and periodic project reports
- `registry/` — repository and system inventory

## Operating principle

Analysis comes before autonomous implementation. Agents must establish the current state, identify gaps against agreed objectives, propose an execution plan, and make changes through traceable branches and pull requests in the relevant repositories.
