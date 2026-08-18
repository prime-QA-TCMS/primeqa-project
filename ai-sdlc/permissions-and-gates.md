# AI-SDLC Permissions, Gates and Escalation

## Autonomous actions allowed by default
Agents may, within an approved work item and their assigned scope:

- read any Prime QA repository and project-control documentation;
- inspect code, tests, CI configuration, schemas and contracts;
- create analysis/planning artifacts;
- create feature/fix branches;
- modify code/docs/tests in affected repositories;
- create/update automated tests;
- run available build/test/lint/static checks;
- open draft pull requests;
- respond to review findings with code changes;
- update work-item status/evidence;
- update non-authoritative reports/status files.

## Autonomous actions requiring additional gate but not owner approval in every case
The following require an independent specialist/reviewer gate before merge:

- changes to `primeqa-common`;
- changes to `fog-ui`;
- cross-repository contract changes;
- database schema/index/migration changes;
- authentication/authorization changes;
- tenant-isolation changes;
- retention/archive/delete logic;
- attachment/upload handling;
- integration/webhook handling;
- deployment/infrastructure code;
- changes that materially affect NFR targets.

## Human approval required
Agents must escalate before performing or approving:

- changes to approved product requirements or acceptance boundaries;
- new/removed service boundaries that materially alter the approved architecture;
- intentional breaking public API changes without approved migration/deprecation plan;
- weakening security/privacy/audit controls;
- destructive production/database operations;
- irreversible hard deletion of real data;
- production secrets/credential rotation where a human-owned secret is required;
- changes to commercial/licensing/tier behaviour;
- bypassing or waiving mandatory Definition-of-Done gates;
- resolving conflicting authoritative requirements;
- deployment to production unless a separate explicit deployment policy grants that authority.

## Merge authority
Default automated policy:

- agents may open and update PRs;
- agents may not merge a PR until all applicable gates pass;
- an implementation agent cannot be the sole approving reviewer of its own work;
- auto-merge may be enabled only after independent review + CI + applicable QA/security/NFR gates pass;
- high-risk items may require human approval even when automated gates pass.

## Risk classification

### Critical
Security boundary, auth, tenant isolation, destructive data lifecycle, release/deployment infrastructure, major shared-package breaking change.

### High
Cross-repository feature, schema migration, external integration, substantial shared-component/platform change, major performance path.

### Medium
Normal feature/change contained within an established boundary.

### Low
Documentation, isolated tests, non-behavioural refactor, clearly bounded maintenance.

Risk determines mandatory review depth; it must not be lowered merely to accelerate execution.

## Escalation packet
When escalating, the agent/orchestrator must provide:

- decision required;
- why it cannot be resolved from approved requirements/evidence;
- affected requirements/repos;
- options with consequences;
- recommended option where appropriate;
- security/data/migration implications;
- what work is blocked pending the decision.

Do not ask humans to rediscover context already available in the repository.
