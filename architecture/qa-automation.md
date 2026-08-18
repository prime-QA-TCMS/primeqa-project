# QA Automation Architecture

## Decision

Prime QA QA automation is split into three repositories with one-way dependencies:

```text
primeqa-qa-test-automation
          |
     +----+----+
     |         |
     v         v
primeqa-qa-ui-engine   primeqa-qa-api-engine
     |                         |
     v                         v
 Browser/UI                 TCMS APIs
```

## Responsibilities

### `primeqa-qa-ui-engine`

Generic Playwright browser execution primitives. It owns browser-facing mechanics but not Prime QA business workflows.

### `primeqa-qa-api-engine`

Generic HTTP/API execution primitives. It owns transport mechanics, generic response handling and API execution but not Prime QA business workflows.

### `primeqa-qa-test-automation`

Owns TCMS business test scenarios, domain-specific UI flows, domain-specific API clients, test data, assertions, orchestration and reporting.

## Cross-layer verification

The central runner may use both engines in the same scenario. Examples include:

- create through UI and verify through API
- create through API and verify through UI
- change permissions through API and verify UI behaviour
- complete a UI workflow and verify resulting API state

State must be passed explicitly through return values or a test-scoped context. Engines must never depend on each other.

## Rationale

This design keeps execution mechanics reusable while preventing business logic from being duplicated across separate UI and API suites. It also makes integration correctness a first-class test concern rather than treating UI and API automation as isolated systems.

## Guardrails

- no business scenarios in engine repositories
- no UI-engine dependency on API engine or vice versa
- no production credentials or PII in automation repositories
- tests should be independently parallelizable where practical
- stable selectors and observable waits are required for UI automation
- cross-layer tests should prove meaningful integration behaviour rather than duplicate lower-level coverage
