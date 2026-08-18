# Prime QA Automated AI-SDLC

Status: **Initial operating specification**  
Last updated: **2026-08-18**

This directory defines how Codex/AI agents participate in the Prime QA software-development lifecycle. The goal is not unrestricted autonomous coding; it is a controlled, evidence-driven engineering system with explicit roles, hand-offs, validation gates and escalation rules.

## Core principles

1. Requirements and approved architecture in `primeqa-project` are the source of truth.
2. Analysis precedes implementation when requirements, architecture or current-state behaviour are unclear.
3. No implementation agent is the final reviewer of its own work.
4. Shared packages (`primeqa-common`, `fog-ui`) have stricter review and compatibility gates because they can affect multiple repositories.
5. Observable behaviour must be validated through the appropriate unit, API, UI and cross-layer tests.
6. A merged PR is not equivalent to a completed requirement.
7. Completion requires traceable evidence against acceptance criteria and Definition of Done.
8. Human escalation is required for unresolved product choices, destructive operations, security/compliance exceptions and material architectural changes.
9. Agents must prefer small, independently reviewable changes over large speculative rewrites.
10. Failures route back to the stage capable of correcting the root cause rather than repeatedly retrying implementation without analysis.

## AI-SDLC documents

- `agents.md` — agent roles, responsibilities and hand-off contracts
- `state-machine.md` — lifecycle states, transitions, failure loops and gates
- `work-item-schema.md` — standard machine-readable structure for requirements, stories, tasks and evidence
- `definition-of-done.md` — mandatory evidence and completion rules
- `permissions-and-gates.md` — autonomous permissions, human gates and escalation rules
- `branching-and-prs.md` — repository/branch/PR conventions
- `orchestration.md` — rules for selecting, dispatching and coordinating work

## Standard lifecycle

```text
Requirement / Objective
        ↓
Business + Product Analysis
        ↓
Technical + Architecture Analysis
        ↓
Security / QA / NFR Analysis
        ↓
Implementation Plan
        ↓
Development
        ↓
Developer Validation
        ↓
Independent Code Review
        ↓
API / UI / Cross-layer QA
        ↓
Security + NFR Verification (where applicable)
        ↓
Objective / Acceptance Verification
        ↓
Merge
        ↓
Evidence + Status + Reporting
```

Not every work item needs every specialist stage. The orchestrator must determine applicable gates from risk, affected repositories and acceptance criteria, while never skipping mandatory gates for shared libraries, security-sensitive behaviour or cross-repository changes.

## Human role

The project owner remains the authority for unresolved product requirements and acceptance boundaries. Human approval is not required for every low-risk implementation step, but it is mandatory for the escalation categories defined in `permissions-and-gates.md`.
