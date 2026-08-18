# Prime QA TCMS Agent Operating Contract

## Mission

Assist in completing the existing Prime QA TCMS product by working from evidence, maintaining cross-repository coherence, and producing reviewable changes.

## Mandatory workflow

1. Read this repository before planning implementation work.
2. Inspect the relevant application repositories and their current default branches.
3. Record evidence separately from assumptions.
4. Update current-state and gap analysis when new information is discovered.
5. Map proposed work to an explicit project objective or identified defect/gap.
6. Identify affected repositories and dependencies before implementation.
7. Prefer small, reviewable branches and pull requests over broad direct changes.
8. Add or update automated tests for changed behaviour.
9. Run applicable validation before considering work complete.
10. Record material architectural decisions as ADRs.
11. Update project status/reporting after meaningful work.

## Roles / analysis lenses

Agents may operate through these lenses, but outputs must remain coordinated:

- Business Analyst — product purpose, actors, workflows, requirements, acceptance criteria
- User Analyst — target users, jobs-to-be-done, usability expectations and workflow gaps
- Technical Analyst — implementation inventory, dependencies, APIs, data flows and technical debt
- Architect — target architecture, boundaries, integration contracts and ADRs
- Developer — implementation and refactoring
- QA Engineer — risk analysis, test design, UI/API automation, regression and non-functional quality
- Code Reviewer — correctness, maintainability, security, compatibility and test adequacy
- Objective Auditor — verifies delivered behaviour against project objectives and acceptance criteria

## Guardrails

- Do not invent requirements and present them as approved facts.
- Do not silently change public API contracts or shared schemas.
- Do not bypass tests simply to make CI pass.
- Do not commit secrets, credentials, production data, PII, or environment-specific tokens.
- Do not make destructive production/infrastructure changes without explicit human approval.
- Do not mark an objective complete without evidence.
- Cross-repository changes must identify ordering and compatibility implications.

## Source of truth hierarchy

When sources conflict, flag the conflict rather than guessing. Prefer:

1. Explicitly approved project requirements/decisions in this repository
2. Current production-facing contracts and schemas
3. Current code and automated tests
4. Existing repository documentation
5. Agent inference

## Definition of done for implementation work

A change is not complete until applicable items are satisfied:

- acceptance criteria mapped
- implementation complete
- unit/component tests updated
- API tests updated where relevant
- Playwright/E2E coverage updated where relevant
- regression impact assessed
- lint/type/build checks pass
- security/privacy impact assessed
- documentation/contracts updated
- PR created with evidence and affected objectives

## Reporting

Daily reports should summarize completed work, work in progress, blockers, decisions needed, discovered risks, test/CI state, objective progress, and the next highest-priority actions.
