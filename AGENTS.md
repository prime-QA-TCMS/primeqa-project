# Prime QA TCMS Agent Operating Contract

## Mission

Assist in completing the existing Prime QA TCMS product by working from evidence, maintaining cross-repository coherence, and producing reviewable, independently validated changes.

## Mandatory reading order

Before implementation work, read:

1. `requirements/technical-requirements.md`
2. `requirements/open-questions.md`
3. `registry/repositories.md`
4. relevant architecture/ADR documents
5. `ai-sdlc/README.md`
6. `ai-sdlc/agents.md`
7. `ai-sdlc/state-machine.md`
8. `ai-sdlc/work-item-schema.md`
9. `ai-sdlc/definition-of-done.md`
10. `ai-sdlc/permissions-and-gates.md`
11. `ai-sdlc/branching-and-prs.md`
12. `ai-sdlc/orchestration.md`

## Mandatory workflow

1. Read the project control plane before planning implementation work.
2. Inspect the relevant application repositories and their current default branches.
3. Record evidence separately from assumptions.
4. Update current-state/gap analysis when new information is discovered.
5. Map work to an explicit requirement/objective/defect.
6. Represent executable work using the AI-SDLC work-item schema.
7. Identify affected repositories, shared packages and dependencies before implementation.
8. Follow the AI-SDLC lifecycle state machine and risk gates.
9. Prefer small, reviewable branches and pull requests over broad direct changes.
10. Add/update automated tests for changed behaviour.
11. Run applicable developer validation before review.
12. Use an independent review context; implementation agents are not final reviewers of their own work.
13. Run applicable API/UI/cross-layer/security/NFR verification.
14. Record material architectural decisions as ADRs.
15. Require objective/acceptance verification and evidence before marking work complete.
16. Update status/reporting after meaningful work.

## Roles / analysis lenses

Detailed responsibilities live in `ai-sdlc/agents.md`. The supported lenses include:

- Business Analyst
- End-user/Product Analyst
- Technical Analyst
- Architect
- Security Analyst
- QA/Test Management Analyst
- QA Automation Analyst
- Performance/NFR Analyst
- Implementation Agent
- Independent Code Reviewer
- QA Verification Agent
- Objective Auditor
- Orchestrator

## Guardrails

- Do not invent requirements and present them as approved facts.
- Do not silently change public API contracts or shared schemas.
- Do not bypass tests simply to make CI pass.
- Do not commit secrets, credentials, production data, PII, or environment-specific tokens.
- Do not make destructive production/infrastructure changes without explicit human approval.
- Do not mark an objective complete without evidence and Objective Auditor PASS.
- Cross-repository changes must identify ordering and compatibility implications.
- Shared package changes (`primeqa-common`, `fog-ui`) require the stricter gates defined by the AI-SDLC.
- Two materially similar failed attempts without new evidence require root-cause analysis rather than blind retry.

## Source of truth hierarchy

When sources conflict, flag the conflict rather than guessing. Prefer:

1. Explicitly approved project requirements/decisions in this repository
2. Approved ADRs/architecture specifications
3. Current production-facing contracts and schemas
4. Current code and automated tests
5. Existing repository documentation
6. Agent inference

## Definition of Done

`ai-sdlc/definition-of-done.md` is authoritative for implementation completion. A merged PR alone is not completion.

## Permissions and human gates

`ai-sdlc/permissions-and-gates.md` defines what agents may do autonomously, what requires independent specialist gates, and what must be escalated for human approval.

## Reporting

Daily/periodic reports should summarize completed work, work in progress by lifecycle state, blockers, decisions needed, discovered risks, test/CI/security/NFR state, objective progress, evidence gaps, and the next highest-priority eligible actions. Activity without objective progress must not be presented as delivery progress.
