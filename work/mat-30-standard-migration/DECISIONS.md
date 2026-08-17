# skills → AE standard migration — decisions

<!-- Append-only: date — choice — why. -->
- 2026-08-17 — Migration plan approved by owner in chat (v1 → v2, keep
  all four skills and docs, dead-name fixes, tiers.md seed) — MAT-30
  owner direction.
- 2026-08-17 — External-dependency assumption: only the workstation
  junction depends on this repo, and the `skills/<name>/` layout does
  not change — owner was asked; no additional dependency named.
  Plainest reading applied; flag if this proves wrong.
- 2026-08-17 — Historical docs (ADR-001, specs, plans) keep their
  pre-rename wording — records are never rewritten; only living
  surfaces (README, context files) get the rename.
- 2026-08-17 — Executed via relay as its first production run —
  MAT-29's dogfood direction; friction findings feed MAT-33.
- 2026-08-17 — Linear project "skills" created; MAT-30 assigned — born
  at migration time per the issue's own direction.
