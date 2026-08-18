# AI-SDLC Orchestration Rules

## Purpose
The orchestrator coordinates work; it does not replace product ownership, architecture authority or independent verification.

## Work selection
An item is eligible for autonomous execution only when:

- its parent requirement/objective is approved;
- required analysis is complete or explicitly not applicable;
- acceptance criteria exist;
- affected repositories are known enough to begin safely;
- blocking dependencies are resolved;
- no human-decision gate is open;
- risk classification has been assigned.

The orchestrator should prioritize:

1. blockers/dependencies that unlock multiple items;
2. critical defects/security issues;
3. high-value/high-risk foundation work;
4. approved roadmap priority;
5. smaller independent items that can progress safely in parallel.

## Dispatch
Dispatch by need rather than using one general agent for everything.

Examples:

- unclear workflow → Business + Product Analyst
- unknown current behaviour → Technical Analyst
- cross-service/data/deployment choice → Architect
- auth/upload/tenant/delete change → Security Analyst
- acceptance/test strategy → QA Analyst
- code implementation → Implementation Agent
- completed diff → Independent Reviewer
- observable behaviour → QA Verification Agent
- final requirement closure → Objective Auditor

## Parallelism
Parallel work is allowed when:

- tasks do not modify the same contract/file boundary incompatibly;
- dependency ordering is explicit;
- shared-package versions/interfaces are stable enough for consumers;
- integration testing can reconcile the outputs.

Do not parallelize coupled changes merely for throughput when it increases merge ambiguity.

## Failure handling
The orchestrator must classify failures before redispatch:

- requirement ambiguity;
- architecture conflict;
- code defect;
- test defect/flakiness;
- environment/infrastructure failure;
- dependency unavailable;
- security/NFR failure;
- reviewer disagreement.

Two materially similar failed implementation attempts without new evidence trigger root-cause analysis rather than another blind retry.

## Dependency handling
Cross-repository dependencies must be represented explicitly. A work item may be blocked by:

- shared package release/change;
- API contract availability;
- schema/migration readiness;
- UI component availability;
- test-engine capability;
- external service/integration configuration;
- human product/architecture decision.

## Evidence/state updates
After each meaningful stage, record:

- state transition;
- agent/lens used;
- outputs/evidence;
- decisions made;
- new risks/open questions;
- linked branch/PR/checks;
- next eligible transition.

## Daily/periodic reporting
The orchestrator should produce a programme report containing:

- objectives progressed;
- items completed/merged;
- current in-progress work by lifecycle state;
- blocking failures and root causes;
- human decisions required;
- CI/QA/security/NFR health;
- cross-repository dependency risks;
- requirement coverage/evidence gaps;
- next highest-priority eligible work.

Activity without objective progress should not be presented as delivery progress.

## Completion
When all child work is merged and evidence is available, dispatch the Objective Auditor. If `PASS`, update objective/work-item status and reporting. If `FAIL`, route the specific evidence gap/defect back to the responsible lifecycle stage. If `BLOCKED`, preserve the item as blocked and surface the exact decision/dependency required.

## Initial implementation recommendation
Use GitHub Issues/Projects as the durable work/state layer, repository branches/PRs as implementation units, GitHub Actions as deterministic CI gates, the three QA automation repositories as behavioural validation infrastructure, and `primeqa-project` as requirements/evidence/governance control plane.

The orchestration implementation must remain replaceable: lifecycle rules belong in this specification, not only inside one proprietary agent prompt or workflow script.
