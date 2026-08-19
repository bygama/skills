# DECISIONS — MAT-94

## D1 — SPEC approved as written; NOTICE carries the full license text (parent, 2026-08-19)

Asked at the design-first gate whether the SPEC (including the
lane-proposed normalized notice format) stands. Parent ruling, verbatim:

> APPROVED as written, including the normalized notice format — an HTML
> comment after frontmatter travels with the file (the point of
> per-file), and the README Provenance section provides the
> reader-visible layer, so the pair covers both needs. One nuance to
> honor while executing: if any file is classified SUBSTANTIAL and a
> NOTICE file turns out to be needed (your item 4), the NOTICE is where
> the full upstream MIT permission text lives — the per-file comment
> stays short and points there. Shape PLAN.md and proceed. Record this
> ruling in your DECISIONS.md.

Consequences for the plan: the NOTICE judgment runs BEFORE notices are
applied, so per-file comments can point at NOTICE when it exists; the
per-file comment never inlines the full MIT permission text.
