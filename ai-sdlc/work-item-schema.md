# AI-SDLC Work Item Schema

Every executable work item must be representable using the following logical schema. The implementation may use GitHub Issues/Projects fields, YAML/JSON metadata, or another approved machine-readable format, but these fields are mandatory unless explicitly not applicable.

```yaml
id: WI-0001
parent_requirement: TR-...
parent_objective: OBJ-...
title: short outcome-oriented title
type: capability | story | technical-task | defect | analysis | qa | architecture | security | nfr
state: PROPOSED
priority: critical | high | medium | low
risk: critical | high | medium | low

scope:
  summary: ""
  in_scope: []
  out_of_scope: []

affected_repositories: []
dependencies: []
blocked_by: []

requirements:
  source_refs: []
  acceptance_criteria: []
  non_functional_criteria: []

analysis:
  evidence: []
  assumptions: []
  open_questions: []
  decisions: []

implementation:
  plan: []
  branch_refs: []
  pull_requests: []
  migrations: []

validation:
  unit_component: []
  api: []
  ui_e2e: []
  cross_layer: []
  security: []
  performance_nfr: []
  documentation: []

review:
  reviewer_context: "independent"
  findings: []
  resolution_evidence: []

completion:
  evidence_refs: []
  residual_risks: []
  objective_audit: pending | pass | fail | blocked
```

## Decomposition hierarchy

Preferred hierarchy:

**Requirement / Objective → Capability/Epic → Story/Behaviour → Repository Task → Test/Evidence**

A single story may produce multiple repository tasks when implementation spans service, shared package, UI and automation repos.

## Acceptance criteria rules

Acceptance criteria must:

- describe observable outcomes rather than implementation preference;
- be independently verifiable;
- include role/permission behaviour where applicable;
- include failure/negative behaviour where material;
- identify data/tenant boundaries where applicable;
- identify compatibility/migration expectations when modifying existing behaviour.

## Repository task rules

Each repository task must state:

- why this repository is affected;
- files/modules likely involved if already known;
- dependency ordering;
- expected public/internal contract changes;
- required developer tests;
- downstream consumers that may be impacted.

## Evidence references

Evidence should reference durable artifacts when possible:

- requirement/ADR IDs
- GitHub issues/PRs/commits
- CI run/check identifiers
- automated test reports
- code-review findings
- security/performance reports
- screenshots/traces only where they add meaningful acceptance evidence

Narrative claims such as “works now” are not sufficient completion evidence.
