# Cold baseline — `designing-consistently`

> **Lane framing (not the runner's words).** Everything below the `## Step 1` heading, through
> `## What I would have committed`, is a verbatim transcript from a separate cold-runner agent.
> This preamble and the `## Controller-verified addenda` section at the bottom are the lane's own
> framing, added around that transcript — they are not part of what the runner wrote.
>
> - **What was run:** `designing-consistently` exactly as it stands today, unmodified.
> - **Which copy:** the copy installed at the user's global skills folder
>   (`~/.claude/skills/designing-consistently/SKILL.md`), verified byte-identical to this
>   worktree's `skills/designing-consistently/SKILL.md` before the run — per the DECISIONS.md
>   ruling that the cold baseline must be taken against the global install, since the installer
>   junction-links the global folder to the MAIN checkout, not to this worktree.
> - **Against what:** `<PEGASUZ>/Pegasuz-Core/frontend-admin/admin` (the evidence repo named in
>   DECISIONS.md's evidence-repo ruling), read-only for the whole run.
> - **The task the runner was given, quoted:** "Agregá a la vista de Stock un panel de
>   movimientos recientes — las últimas entradas y salidas — arriba de la tabla."
> - **Isolation:** the runner was given only the current `designing-consistently` workflow and
>   the read-only admin repo. It knew nothing of this lane, this SPEC, or the predictions SPEC §5
>   records — it was fenced out of every `work/` directory.
> - **The one constraint that shaped the run, and how to read it:** the repo was READ-ONLY, so
>   wherever the skill called for a write the runner reported what it would have written instead.
>   This constraint affects **Step 3's** "nothing was written to disk, here is the diff I would
>   have committed instead" and **Step 5's** screenshot action ("I did not attempt `npm run dev`
>   or `npm run build` … running it would edit the repo, which the task ruled out"). It does
>   **not** affect Steps 1, 2, or 4's core findings: the absence of any `DESIGN.md` anywhere in
>   the repo, the absence of a `## Decisions` section to read from, and the absence of
>   `design.tokens.css` / `scripts/design-md-gen.mjs` / a `context-lint` script are all facts
>   about what the repo currently contains — true regardless of whether the checkout was
>   writable, and would read identically in a writable clone. Do not read those three steps'
>   failures as artifacts of the fence; they are the skill meeting a real, permanent absence in
>   this codebase.

## Step 1

Skill instruction (quoted): "Single-app repo: root DESIGN.md. Monorepo: the app's own DESIGN.md next to its code. Missing? Offer to instantiate it first (Context-Engineering `templates/repo/DESIGN.md.template`) — designing without it just recreates the drift."

What I ran: `ls -la` at repo root (20 entries: AGENTS.md, CLAUDE.md, docs/, src/, package.json, etc. — no DESIGN.md). `find . -iname "DESIGN.md" -not -path "./node_modules/*" -not -path "./dist/*"` → 0 results. `find . -iname "*design.md*"` (no exclusions) → 0 results. `ls Documentation` → "No such file or directory". `find . -iname "design.tokens.css"` → 0 results. `find . -iname "design-md-gen.mjs"` → 0 results. `find . -iname "*.template"` → 0 results.

I also started a broad `find / -maxdepth 6 -iname "Context-Engineering"` to check whether the template source the skill points at was reachable at all. That command timed out in the background (task id bqgldk7en); I recognized mid-flight that scanning outside this repo breaches my scope fence, so I called `TaskStop` on it and never read its output.

Conclusion: DESIGN.md does not exist anywhere in this repo (0 for 0 across every search). What exists instead is `docs/admin-design-system-v1.0.md` (82,323 bytes, section headers 0–14 running至 at least line 1597) <!-- [editorial note: stray CJK character in source, kept verbatim per lane instruction] -->, plus `docs/admin-ux-audit.md`, `docs/admin-responsive-report.md`, `docs/round-1-redesign-plan.md`, `docs/round-2-backlog.md`. None of these match the DESIGN.md shape (no YAML frontmatter of tokens, no `## Decisions` section).

Where this step failed: the skill's own fallback ("offer to instantiate... from Context-Engineering templates/repo/DESIGN.md.template") could not be carried out — the template source is outside my scope fence, and even if it weren't, the repo is read-only so I could not create the file regardless. I did not create anything. Per the skill's own words, everything after this point is "designing without it" — I was told to proceed through steps 2–5 anyway and record what happens.

## Step 2

Skill instruction (quoted): "From the frontmatter: the tokens and components the work will need. From `## Decisions`: every entry under the surfaces (routes/screens) about to be touched — these are standing decisions, not suggestions."

There is no DESIGN.md, so there is no frontmatter and no `## Decisions` section. This step has zero material to read from its prescribed source — not "few entries," literally none, because the file doesn't exist.

Improvisation (flagged — the skill did not tell me to do this): to see what a real Step 2 would have surfaced, I read code directly instead of a DESIGN.md.
- `AGENTS.md` gotcha: "The current visual line is `.nl` classes + `--nl-*` tokens (`Documentation/UX-UI-STANDARDS.md`, `Documentation/VISUAL_QA.md`); redesigns follow it, not the older views." Both referenced docs also don't exist in this repo — no `Documentation/` directory at all. A second dead pointer, independent of the DESIGN.md one.
- The target view, `src/views/admin/ShopStockView.vue` (1230 lines), is written entirely in the `.nl` system: `--nl-*` custom properties (defined in `src/assets/css/nueva-linea.css`, 288 lines) and `.nl`-scoped classes.
- Its sibling `src/views/admin/ShopStockMovementsView.vue` (665 lines) — the existing full "Historial" of entries/exits, linked from ShopStockView's header — uses a completely different, older token set: `bg-surface`, `rounded-card`, `border-line-subtle`, `shadow-e1`, `text-ink-heading`, `bg-success`/`text-success-text`, `bg-error`/`text-error-text`. Two different design systems sit side by side across two screens of one feature area.
- "Standing decisions" do exist in this repo, just not centralized: they're inline Vue-template comments carrying owner names/dates/ticket IDs, e.g. `ShopStockView.vue` ~line 39: "KPIs en 4+4+4 con jerarquía por importancia (dueño, 2026-08-20, PEG-60)"; ~line 130: "Panels (nodo `krHm0`): gap 22, NO 18 como las KPIs"; `ShopStockMovementsView.vue`: "queja real 2026-07-15: antes filtraba y nadie llegaba al producto" / "PEG-19 D9 → PEG-39". These behave like a Decisions log, just scattered through source instead of centralized in one file.

Conclusion: Step 2 as literally written produced nothing. Everything above is something I did instead, not what the skill instructed.

## Step 3

Skill instruction (quoted): "Styles come from the generated `design.tokens.css`, never raw values... a genuinely new pattern is born tokenized: values added to DESIGN.md frontmatter, then `node scripts/design-md-gen.mjs` regenerates... new colors, type sizes, radii, and shadows always go through the frontmatter. A standing decision the work conflicts with is renegotiated with the user — never silently overridden."

`design.tokens.css` and `scripts/design-md-gen.mjs` don't exist (confirmed in Step 1). There is nothing generated to consume and no generator to run. This instruction cannot be followed as written.

Improvisation: I used the repo's actual current styling substrate for this surface (the `--nl-*` tokens) and reused two existing patterns under `src/components/stock-nl/`: `StockAnalyticsCard.vue` — the shared chrome already used for the two panels that sit above the table today ("¿Dónde está la plata?" donut, "Más movidos" bars), explicitly commented as shared "para que las dos no puedan divergir con el tiempo," with a `body="list"` variant built for exactly this shape — and `getStockMovements()` from `src/services/shopStockService.js`, already used by `ShopStockMovementsView.vue` with `{ page, limit, ...filters }` params, returning `{ items, totals, pagination }`, defaulting to newest-first when no `sort` is passed.

Because the repo is read-only, nothing below was written to disk. Code I would have added, as a new file `src/components/stock-nl/StockRecentMovementsPanel.vue`:

```vue
<template>
  <StockAnalyticsCard
    title="Movimientos recientes"
    subtitle="Últimas entradas y salidas de stock"
    icon="arrow-up-down"
    tone="stock"
    body="list"
  >
    <template #actions>
      <RouterLink
        :to="{ name: 'admin-shop-stock-movements' }"
        class="text-[13px] font-semibold text-[var(--nl-ink-muted)] hover:text-[var(--nl-ink)]"
      >Ver historial</RouterLink>
    </template>

    <ul v-if="!loading && items.length" class="flex flex-col">
      <li
        v-for="m in items"
        :key="m.id"
        class="flex items-center gap-[10px] py-[9px] [border-bottom:1px_solid_var(--nl-border-soft)] last:[border-bottom:none]"
      >
        <Icon
          :name="m.cambio >= 0 ? 'arrow-up' : 'arrow-down'"
          :size="14"
          :style="{ color: m.cambio >= 0 ? 'var(--nl-success)' : 'var(--nl-error)' }"
          aria-hidden="true"
        />
        <div class="min-w-0 flex-1">
          <p class="truncate text-[13px] font-semibold text-[var(--nl-ink)]">{{ m.productName }}</p>
          <p class="text-[11px] text-[var(--nl-ink-muted)]">{{ fmtRelative(m.createdAt) }} · {{ REASON_LABELS[m.reason] || m.reason }}</p>
        </div>
        <span
          class="nl-num shrink-0 text-[13px] font-bold"
          :style="{ color: m.cambio >= 0 ? 'var(--nl-success)' : 'var(--nl-error)' }"
        >{{ m.cambio >= 0 ? '+' : '' }}{{ m.cambio }}</span>
      </li>
    </ul>
    <p v-else-if="loading" class="text-[13px] text-[var(--nl-ink-muted)] py-[9px]">Cargando…</p>
    <p v-else class="text-[13px] text-[var(--nl-ink-muted)] py-[9px]">Sin movimientos recientes.</p>
  </StockAnalyticsCard>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import StockAnalyticsCard from '@/components/stock-nl/StockAnalyticsCard.vue'
import Icon from '@/components/ui/Icon.vue'
import { getStockMovements, REASON_LABELS } from '@/services/shopStockService.js'

const items = ref([])
const loading = ref(true)

onMounted(async () => {
  try {
    const data = await getStockMovements({ page: 1, limit: 6 })
    items.value = data.items || []
  } finally {
    loading.value = false
  }
})

function fmtRelative(iso) { /* same relative-time helper pattern used elsewhere in stock-nl */ }
</script>
```

And in `src/views/admin/ShopStockView.vue`, one import line plus one element inserted between the donut/movers grid (ends around line 138) and the `tableAnchor` div (starts around line 140):

```diff
       </div>

+      <StockRecentMovementsPanel />
+
       <div ref="tableAnchor" class="flex flex-col gap-[var(--nl-stack-gap)]">
         <StockFilterBar
```

Judgment call I made without a Decisions log to check it against: I placed the panel above the whole filter+table block (next to the donut/movers row) rather than inside `tableAnchor` after the filter bar, since it reads as a summary panel like the donut/movers, not a table-adjacent control like the existing `sinRepartir` warning banner. With a real DESIGN.md this would be exactly the kind of call Step 2 should have pre-answered or Step 4 should record — neither was possible here.

## Step 4

Skill instruction (quoted): "Work is not complete while an unrecorded decision exists... Each gets `- YYYY-MM-DD — <decision>` under its `### <surface>`."

There is no DESIGN.md, so there is no `## Decisions` section and no `### Stock` (or equivalent) subsection to append a line under. This step has no target to write into — not "I chose not to," there was nowhere for the line to go.

This is where the missing file and the read-only constraint compound: even if DESIGN.md had existed, I made no file changes this run, so nothing needed gating on disk either way — but structurally, the gate itself had no file to gate against, independent of the read-only rule. Decisions I would have recorded had the file existed: (1) new pattern — a "Movimientos recientes" panel above the table, reusing `StockAnalyticsCard`'s `body="list"` variant; (2) data-source choice — `getStockMovements({ limit: 6 })` with no filters, distinct from the full paginated/filterable Historial page; (3) placement choice — above the filter+table block, alongside the donut/movers row, not inside the table's own control stack.

## Step 5

Skill instruction (quoted): "Regenerate tokens and run context-lint (design checks must pass). Re-check each touched surface against its Decisions entries. Screenshot the result when the environment allows it."

- "Regenerate tokens": the generator (`design-md-gen.mjs`) doesn't exist anywhere in the repo (Step 1 search) — nothing to run.
- "run context-lint": no such script exists. `package.json`'s `"scripts"` block has exactly 7 entries (`dev`, `build`, `test:smoke`, `preview`, `serve:dist`, `test`, `qa:visual`) — no lint or design-check entry — and no file matching `*context-lint*` exists anywhere in the repo. Not runnable.
- "Re-check each touched surface against its Decisions entries": no Decisions entries exist for this surface (Steps 2 and 4), so there is nothing to re-check against.
- "Screenshot the result": nothing was built (repo read-only, Step 3 produced only text in this report), so there's no running UI to screenshot. I also did not attempt `npm run dev` or `npm run build` to get to a screenshot-able state, because `vite build` writes into `dist/` (present and last modified today, so it's a real, in-use build output) — running it would edit the repo, which the task ruled out.

Net result: all three prescribed verification actions were inapplicable or unrunnable, for three independent reasons (two missing tools, one missing log source, one blocked by the read-only constraint) — not one shared cause.

## What I would have committed

- New file: `src/components/stock-nl/StockRecentMovementsPanel.vue` (component in Step 3).
- Modified file: `src/views/admin/ShopStockView.vue` (one new import, one new `<StockRecentMovementsPanel />` element above `tableAnchor`, per the Step 3 diff).
- Had DESIGN.md existed: a modified `DESIGN.md`, adding one line under its `## Decisions` → `### Stock` section per Step 4 — `- 2026-08-20 — Added a "Movimientos recientes" panel above the inventory table (StockAnalyticsCard, body="list"; getStockMovements(limit:6), no filters); full history stays on the separate Historial page.`
- Since DESIGN.md doesn't exist and I'm confined to read-only + in-scope actions: no `DESIGN.md` was created or edited, per Step 1's finding that instantiating it was out of reach on both counts.

## Controller-verified addenda

<!-- Facts the controller verified directly after the run, kept strictly separate from the
     runner's words above. Nothing in this section is a re-interpretation of what the transcript
     means — that is PLAN step 4's job. -->

1. **The `Documentation/` pointer — controller-verified.** The runner reported (Step 2) that the
   admin's own `AGENTS.md` cites `Documentation/UX-UI-STANDARDS.md` and `Documentation/VISUAL_QA.md`
   and that neither exists — "no `Documentation/` directory at all." The controller verified this
   afterwards: both files DO exist, at the Pegasuz monorepo root —
   `Documentation/UX-UI-STANDARDS.md` (9757 bytes) and `Documentation/VISUAL_QA.md` (3775 bytes) —
   i.e. two directory levels ABOVE the admin app, and there is indeed no `Documentation/` inside
   the admin. The runner's observation is accurate about its own scope, and the pointer is
   monorepo-root-relative while the context file that carries it is app-level. Both halves hold
   at once; the runner was not wrong.

2. **Zero writes — controller-verified.** `git status --short` in the Pegasuz checkout was empty
   before and after the run, and HEAD was unchanged at `00ddbcbaf`. The read-only constraint held.

3. **The prose docs the runner found — controller-verified.** `docs/admin-design-system-v1.0.md`
   is 82,323 bytes; alongside it `admin-ux-audit.md` (29,010), `admin-responsive-report.md`
   (21,881), `round-1-redesign-plan.md` (26,189), `round-2-backlog.md` (13,904), plus `round-1/`
   and `round-2/` directories. Verified by `ls -la` on the admin's `docs/`.
