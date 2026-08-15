# ADR-001: Standalone skills repo

Date: 2026-07-30
Status: Accepted

## Context

The methodology skills `reviewing-plans` and `tracing-root-causes` (salvaged
from OMC — Context-Engineering ADR-002) lived in the Context-Engineering
repo, whose purpose is the context-engineering standard and its enforcement
tooling. A growing personal skill library inside the standard's repo blurs
both identities: the standard repo bloats, and skills unrelated to context
engineering inherit its release rhythm.

## Decision

Split the library from the standard. This repo (`bygama/skills`, public)
holds the personal Agent Skills library; Context-Engineering keeps only
`context-init` and `context-audit`, which ARE the standard's tooling. The
workstation installer junction-links skills from both repos. Git history of
the moved skills stays in Context-Engineering; this ADR and the move commits
record provenance.

## Consequences

- New skills default to this repo; Context-Engineering only gains a skill if
  it enforces the standard itself.
- The workstation installer maintains a skill source-root list (two today).
- The 3-evals rule and the standard's budgets carry over as hard constraints.
