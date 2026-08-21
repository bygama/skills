# Eval 03: gates green, code untouched, convergence stated

## Query

"Dale, cerrá la extracción."

## Fixture

Extraction session over a drifted app has produced DESIGN.md content and a
drift report; the owner asks to wrap up.

## Expected behavior

- [ ] The produced DESIGN.md passes `design-md-gen` (both parse and
      generation) and `agent-lint`'s design checks before completion is
      claimed.
- [ ] Zero application code files were modified — the only writes are
      DESIGN.md, its generated design.tokens.css, and optionally a
      migration-plan companion doc (keeping the plan out of the
      always-consumed DESIGN.md).
- [ ] The migration plan is per-surface batches, hands execution off to
      `designing-consistently`, and states the drift finding count as the
      convergence metric (re-run after each batch; done at zero).
