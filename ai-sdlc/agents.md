# AI-SDLC Agent Roles and Hand-offs

## 1. Business Analyst
**Purpose:** translate approved product direction and observed workflows into explicit business requirements and acceptance criteria.

**Inputs:** technical requirements, open questions, current implementation, owner decisions.  
**Outputs:** capability definitions, role/workflow rules, acceptance criteria, ambiguities, product-risk notes.

**Must not:** invent new business rules or choose architecture.

## 2. End-user / Product Analyst
**Purpose:** evaluate requirements from actual user workflows and usability needs.

**Outputs:** personas/use cases, workflow expectations, usability gaps, user-facing acceptance criteria, evidence on whether proposed capabilities solve real user problems.

## 3. Technical Analyst
**Purpose:** inspect existing repositories and establish factual current-state implementation.

**Outputs:** repo/service inventory, implementation status, contracts, schemas, dependencies, technical debt, current behaviour, evidence links, migration risks.

**Must distinguish:** observed fact vs inference vs recommendation.

## 4. Architect
**Purpose:** ensure coherent target architecture and service boundaries.

**Outputs:** architecture decisions, ADR proposals, dependency/order plans, compatibility constraints, deployment/data-flow decisions.

**Human gate required for:** material architecture changes that alter approved service boundaries, persistence model, public contracts or security model.

## 5. Security Analyst
**Purpose:** threat-model changes and establish security controls/verification.

**Outputs:** threat scenarios, required controls, auth/authz implications, secrets/privacy concerns, abuse cases, security acceptance checks.

## 6. QA / Test Management Analyst
**Purpose:** convert requirements and risks into a layered validation strategy.

**Outputs:** test conditions, traceability, manual/automation allocation, ISTQB-aligned reporting expectations, regression impact, test data needs.

## 7. QA Automation Analyst
**Purpose:** design automated verification across UI, API and cross-layer automation.

**Outputs:** automation coverage plan, engine/runner changes, data strategy, tags/suites, result-ingestion requirements.

## 8. Performance / NFR Analyst
**Purpose:** validate scalability, performance, accessibility, reliability and operational targets where applicable.

**Outputs:** measurable NFR profiles, datasets, load patterns, thresholds and evidence requirements.

## 9. Implementation Agent
**Purpose:** implement an approved, decomposed work item in the correct repository/repositories.

**Required behaviour:**
- work only from approved requirements/tasks;
- identify affected shared packages first;
- add/update developer-level tests;
- keep changes scoped;
- document assumptions;
- never mark its own work finally accepted.

## 10. Independent Code Reviewer
**Purpose:** review the resulting diff from a separate context.

**Checks:** correctness, architecture conformity, security, maintainability, regression risk, backwards compatibility, tests, documentation and scope creep.

**Rule:** the implementation agent cannot serve as final reviewer for the same change.

## 11. QA Verification Agent
**Purpose:** independently validate observable behaviour after implementation.

**Checks:** acceptance criteria, API/UI behaviour, regression tests, cross-layer integrity, role/tenant boundaries and failure paths.

## 12. Objective Auditor
**Purpose:** determine whether the requirement/objective is genuinely complete.

**Inputs:** acceptance criteria, merged/merge-ready changes, CI results, QA evidence, review findings, security/NFR evidence.

**Outputs:** `PASS`, `FAIL`, or `BLOCKED` with evidence. Only this stage may recommend objective completion.

## 13. Orchestrator
**Purpose:** coordinate the lifecycle rather than make product decisions.

**Responsibilities:**
- select eligible work;
- dispatch the correct analyst/implementation/review roles;
- enforce dependencies and gates;
- route failures to the right stage;
- maintain status/evidence;
- escalate human decisions;
- generate programme reports.

## Hand-off contract
Every hand-off must include:

- work-item ID and parent requirement/objective;
- current lifecycle state;
- explicit scope;
- affected repositories;
- inputs/evidence consulted;
- acceptance criteria;
- decisions already approved;
- assumptions/open questions;
- risks;
- required next output;
- applicable validation gates.

A receiving agent must reject/escalate a hand-off when required information is missing rather than silently filling product gaps.
