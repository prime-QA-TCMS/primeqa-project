# Architecture

This area holds system-level architecture and Architecture Decision Records (ADRs).

## Required artefacts

- current architecture
- target architecture
- service boundaries and responsibilities
- integration/API map
- data ownership and flow
- authentication/authorization architecture
- deployment/runtime topology
- cross-cutting concerns
- ADRs

## ADR convention

Store decisions under `architecture/decisions/` using:

`ADR-NNN-short-title.md`

Each ADR should contain context, decision, alternatives considered, consequences, affected repositories, and date/status.

No major architectural change should be implemented solely because an agent prefers a different pattern. Changes must address an evidenced requirement, risk, or maintainability problem.
