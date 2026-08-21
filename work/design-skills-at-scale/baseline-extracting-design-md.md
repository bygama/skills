# Cold baseline — `extracting-design-md`

> **Lane framing (not the runner's words).** Everything below the `# Cold run:` heading, through
> the end of the file, is a verbatim transcript from a separate cold-runner agent. This preamble
> and the `## Controller-verified addenda` section at the bottom are the lane's own framing, added
> around that transcript — they are not part of what the runner wrote.
>
> - **What was run:** `extracting-design-md` exactly as it stands today, unmodified.
> - **Which copy:** the copy installed at the user's global skills folder
>   (`~/.claude/skills/extracting-design-md/SKILL.md`) — per the DECISIONS.md ruling that the cold
>   baseline must be taken against the global install, since the installer junction-links the
>   global folder to the MAIN checkout, not to this worktree. The controller verified this copy
>   byte-identical to this worktree's `skills/extracting-design-md/SKILL.md` before dispatching the
>   cold runner. The implementer transcribing this file (2026-08-20) independently re-verified the
>   same byte-identity at transcription time, after the run: `diff
>   ~/.claude/skills/extracting-design-md/SKILL.md skills/extracting-design-md/SKILL.md` exits with
>   no output (byte-identical).
> - **Against what:** `<PEGASUZ>/Pegasuz-Core/frontend-admin/admin` (the evidence repo named in
>   DECISIONS.md's evidence-repo ruling), read-only for the whole run.
> - **The task the runner was given, quoted:** "che, el diseño de este admin quedó re
>   inconsistente — cada botón es distinto y hay como mil grises. Armá el DESIGN.md desde lo que ya
>   hay y decime qué tan desprolijo está."
> - **Isolation:** the runner knew nothing of this lane, this SPEC, or SPEC §5's predictions; it
>   was fenced out of every `work/` directory. Its own Step 6 records a further scope fence, in its
>   own words: it was barred from reading this session's other repositories, "the skills repo
>   included," which is why it could not reach the Context-Engineering/Agent-Engineering clone
>   that houses `design-md-gen`/`context-lint` — a fence distinct from both constraints below, and
>   not one this preamble is asserting a cause for beyond what the runner itself states.
> - **The two constraints that shaped this run, and how to read them — honestly, not just what
>   they were called:**
>   1. **READ-ONLY.** Wherever the skill called for a write, the runner reported what it would
>      have written instead. This touches **Step 7** ("Zero code edits were made in this run —
>      consistent both with this instruction and with the read-only constraint I was given") and
>      the final **"What I would have committed"** section (the `DESIGN.md` draft, as a new file
>      at repo root, never actually written). It does **not** touch Steps 1-5's harvest, drift
>      report, or token elections — those are read actions (finds, greps, file reads, counts) that
>      would read identically in a writable clone. It also does **not** explain two other things
>      the runner could not do: Step 4's inability to "confirm with the owner" (the runner
>      attributes that to this being an unattended cold run with no live owner in the loop, not to
>      write permission) or Step 6's inability to run `design-md-gen`/`context-lint` (attributed by
>      the runner to the separate repo-scope fence recorded in the isolation bullet above, not to
>      write permission).
>   2. **NO SUBAGENTS.** The runner was barred from dispatching any subagent, for any part of the
>      run — the transcript's own opening line states this plainly ("No subagent was dispatched at
>      any point"). This one matters more than it looks: the skill's own `## Scaling (bounded)`
>      section names fan-out — "one subagent per app or major section" — as the answer once a repo
>      passes roughly 15 surfaces, and the runner recorded (Step 2) that this repo's nav config
>      lists 60+ distinct menu labels before it stopped counting, i.e. roughly 10x that threshold,
>      and that the fan-out lever the skill names was unavailable to it "by direct instruction" —
>      not by the skill's own choice, and not because the repo turned out to be small enough that
>      the question never came up. The runner substituted repo-wide `grep -r` aggregation instead
>      and states plainly that the resulting counts are real and exhaustive over the patterns
>      searched, even though most of the ~500 individual files were never opened by hand. This
>      fence is the lane's, not the skill's. Say what it was, not what it means for the harvest's
>      practicality or for SPEC §5 — any observation about that has to be read with this fence in
>      view, and the reconciliation itself belongs to PLAN step 4, not here. It does **not** touch
>      Step 1's dead-pointer findings, Step 3's drift ordering, Step 4's token-election reasoning,
>      Step 5's backfill, or Step 6's scope-fenced gate failure — none of those turn on whether a
>      subagent was available.

# Cold run: extracting-design-md on Pegasuz-Core/frontend-admin/admin

Repo: `/c/Briar/repos/work/Pegasuz/Pegasuz-Core/frontend-admin/admin`
Read-only throughout — no file in the target repo was created, edited, deleted, or moved.
No subagent was dispatched at any point (see Step 2 and the note on the skill's
"Scaling (bounded)" section).

## Step 1

**Instruction quoted:** "Inventory style sources. Stylesheets/@theme, tailwind config,
and the classes and inline styles components actually use. Monorepo: one extraction
per app."

What I did: listed the repo root, then `AGENTS.md`/`CLAUDE.md`, then everything under
`src/assets` and the tailwind/postcss config.

Commands and results:
- `ls -la` at repo root — single-app repo (not a monorepo), so "one extraction per app"
  reduces to "one extraction, full stop."
- `AGENTS.md` gotcha: "The current visual line is `.nl` classes + `--nl-*` tokens
  (`Documentation/UX-UI-STANDARDS.md`, `Documentation/VISUAL_QA.md`); redesigns follow
  it, not the older views." I tried to open both referenced files and the
  `Documentation/` directory they imply:
  `find . -iname "Documentation" -not -path "*/node_modules/*"` and
  `find . -ipath "*Documentation*" -not -path "*/node_modules/*" -type f` both returned
  nothing. **The two files AGENTS.md names as the authority for the current visual
  line do not exist anywhere in this repo.** Same for `Documentation/Ecommerce/ADMIN_UX_ROADMAP.md`
  and `Documentation/Ecommerce/modulo-tienda-facturacion-arca/47-FRONTEND-DESIGN-SYSTEM.md`,
  the latter cited inline in `src/assets/css/tokens.css:4` as "doc 47" (the codebase's
  own comments repeatedly cite "doc 47 §N" as the source of truth — that document is not
  in this repo). This is not a skill failure, it's a repo-state fact worth recording:
  the inventory step pointed me at named authorities that are dead links.
- `find . -iname "DESIGN.md"` — no existing DESIGN.md anywhere. Confirmed clean slate.
- Style sources found and read in full:
  - `tailwind.config.js` (377 lines) — three generations of color/radius/type scale
    stacked in one file, explicitly labelled in its own comments: "TOKENS v2 (doc 47
    §32)" as current, "WARM NEUTRALS (v1.1 — nuevo)", "PRIMARY (v1.0 — heredado, sin
    cambios)", "LEGACY (v1.0 — tolerado, no usar en código nuevo)".
  - `src/assets/css/tokens.css` (126 lines) — self-labelled "ÚNICA fuente de verdad de
    color" (doc 47 §32), CSS custom properties for color/radius/spacing/elevation/motion,
    plus two `:root[data-tenant="…"]` overrides.
  - `src/assets/css/main.css` (405 lines) — Tailwind base/components/utilities, containing
    a block explicitly commented `/* DEPRECATED — purga en corridas siguientes... PROHIBIDO
    usar estas clases o los colores midnight/charcoal/silver/neutral/primary que arrastran. */`
    around `.btn`, `.btn-primary`, `.card`, `.input`, etc.
  - `src/assets/css/nueva-linea.css` (288 lines) — a second, fully independent token
    namespace (`--nl-*`) whose own header comment says: "PRECEDENCIA: si `tokens.css`
    (v2, doc 47 §32) contradice a este archivo, manda ESTE. tokens.css queda LEGACY para
    vistas rediseñadas." — i.e. the file that calls itself canonical (tokens.css) is
    told by a later file that it is not canonical for redesigned views.
  - `src/assets/css/servicio.css` (113 lines) — a third independent namespace (`--sh-*`)
    for the "Servicio Técnico" module, citing yet another external authority
    ("`.pen` `diseñosgenerales`", "`C:\Users\nicol\rcsistemas-design-stage\NODOS.md`")
    that is also outside this repo and unverifiable from here.
  - `src/assets/styles/main.css` (318 lines) exists as a **sibling** to
    `src/assets/css/main.css`. I checked `src/main.js` (the only place CSS is imported)
    and it imports `./assets/css/main.css`, `./assets/css/tokens.css`,
    `./assets/css/nueva-linea.css`, `./assets/css/servicio.css` — never
    `./assets/styles/main.css`. Grepped the whole `src/` tree for any reference to
    `assets/styles` — zero hits anywhere. **`src/assets/styles/main.css` is a dead,
    unimported stylesheet** (I confirmed it is orphaned via import-graph grep; I did
    not read its 318 lines of content, since by the skill's own framing — "the classes
    and inline styles components actually use" — a file nothing imports is out of scope
    for inventory. Flagging its existence and dead status is as far as I took it.)
  - `tenant` override in `tokens.css` (`data-tenant="emilopez"`, `data-tenant="ecommerce-test"`)
    is a selector-scoped reassignment of the same `--tenant-*` variables — per the skill's
    own rule this is a **mode**, not drift, and I carried it as such into Step 4/6.
- Existing design documentation already in the repo (not "style sources" in the
  stylesheet/tailwind-config/component-class sense, but directly relevant background):
  `docs/admin-design-system-v1.0.md` (1726 lines) and `docs/round-1/admin-design-system-v1.1.md`
  (1358 lines). I read their section headers only (`grep -n "^#"`), not their bodies —
  4364 combined lines across just these two files plus the other four docs in `docs/`
  is a large detour from the harvest-by-grep method the skill prescribes for Step 1/2,
  and the skill's Step 1 instruction is about stylesheets/tailwind config/component
  classes, not prior planning prose. I did use the headers directly later (Step 3/4):
  v1.1 §3.7 is literally titled "Migración `neutral-*` → `warm-*`" — a written migration
  plan from one gray scale to another that the harvest in Step 2 shows was never
  finished.

Nothing in Step 1 required a decision I couldn't make; the friction was entirely the
dead `Documentation/` references, which I'm recording rather than silently ignoring.

## Step 2

**Instruction quoted:** "Harvest. Grep the values by family (colors, radii, spacing,
type, shadows) with occurrence counts and file:line locations — counts are what make
the report objective instead of vibes."

**Scale problem, with numbers, first:** `find src -type f | wc -l` = 518 files;
`find src -name "*.vue" | wc -l` = 337; `find src -name "*.js" | wc -l` = 174.
`src/views` alone has 157 files, `src/components` 187. The nav config
(`src/data/navConfig.js`, the sole place views are registered per AGENTS.md) lists at
least 60+ distinct menu labels before I stopped counting at a `head -60` cutoff — i.e.
well past the skill's own stated fan-out threshold ("worth proposing only past roughly
15 surfaces"). **The skill's "Scaling (bounded)" section explicitly says harvesting
"may fan out — one subagent per app or major section" once a repo is this size, and
names ~15 surfaces as the threshold past which fanning out is worth proposing to the
owner. This repo is roughly 10x that. I did not fan out, because the task that sent me
here explicitly barred dispatching any subagent for any part of this run, full stop —
not "propose it and wait," but "do not do it." So the exact scaling lever the skill
names as the answer to this size was unavailable to me by direct instruction, and I
recorded that rather than silently either (a) fanning out anyway or (b) pretending the
repo was small enough that the question never came up.**

What I actually did instead: I did not open all 337 Vue files by hand — that would
've meant reading roughly 500 files one at a time in a single-threaded session, which
is not practical. Instead I leaned entirely on repo-wide `grep -r` aggregation across
`src --include=*.vue --include=*.js`, which **is** exhaustive over the search pattern
even though I never opened most of the individual files — the counts below are real,
complete counts against the patterns I searched, not samples extrapolated from a
subset of files. Where I show "file:line" evidence it's a small illustrative sample
out of a much larger match set, and I say the total count next to it every time so the
sample is never mistaken for the whole.

Two self-caught methodology mistakes worth recording plainly (not the skill's fault,
mine, and I fixed them before using the numbers):
- My first pass at "does the deprecated `.btn`/`.card` class still get used" used
  `grep -E '\b(btn|card)\b'`, which matches "btn" as a substring boundary even inside
  hyphenated custom classes like `news-btn` or `s-btn` (`-` is a non-word character, so
  `\b` fires around it). That produced a false list of 34 "hits" that were mostly
  unrelated scoped classes (`news-btn`, `s-btn`, `card-dark`-adjacent noise). I re-ran
  with a stricter space/quote-delimited match and got real numbers instead (below).
- My first pass at counting bare `rounded` (Tailwind's unsuffixed default radius) used
  `grep -Eo '\brounded\b'` and returned 5418 — which is actually close to the total of
  *all* `rounded-*` variants combined, because `\brounded\b` also matches inside
  `rounded-lg`, `rounded-card`, etc. Fixed with a negative-lookahead PCRE pattern
  (`(?<![-\w])rounded(?!-)(?!\w)`), giving a real count of 346.
- I also, once, ran a per-file breakdown of button-height classes (`h-7` through `h-12`)
  that dumped several hundred file:count lines with no aggregate — expensive and not
  useful in that form. I re-ran it as a straight aggregate afterward; the per-file dump
  is not reproduced in this report, only the aggregate totals are, further down.

### Colors — gray/neutral family (the owner's literal "hay como mil grises")

Counted with `grep -rEo '(bg|text|border|ring|divide|from|via|to|placeholder|decoration)-<family>(-N)?' src --include=*.vue --include=*.js`:

| family | total occurrences | files touched | notes |
|---|---|---|---|
| `neutral-*` | 2009 | 49 | Tailwind's own default gray scale, used directly |
| `warm-*` | 2308 | 39 | v1.1's custom warm-neutral scale, meant to replace `neutral-*` |
| `slate-*` | 7 | — | stray, no config entry for it at all (not even declared in tailwind.config.js) |
| `ink-*` (v2 semantic) | 6821 | — | `ink-muted` 2442, `ink-secondary` 1853, `ink-heading` 1304, `ink` 707, `ink-disabled` 515 |
| `muted`/`muted-strong` (v1.0 alias) | 1 | 1 | `src/components/charts/SalesFlowChart.vue` — essentially dead |
| `midnight-*`/`charcoal-*`/`silver-*`/`accent-*` (v1.0 legacy) | 0 | 0 | confirmed dead in live components; only exist inside `main.css`'s own explicitly-marked DEPRECATED block. (One `silver` hit in `MlCentroView.vue:276/282` is a MercadoLibre medal-tier string, not this color family — checked and excluded.) |

Only **1 file** uses both `neutral-*` and `warm-*` together — meaning the drift is not
"random per-line" chaos so much as **whole surfaces committed to one gray scale or the
other**, which is arguably a more damning finding: this isn't accidental sprinkling,
it's two camps of views that were each internally consistent but never reconciled with
each other.

`text-muted` (the deprecated single-file case above) is real, but the semantic
replacement (`ink-muted`, 2442 uses) has clearly already won in practice — this is
the one place the "propose one token" step is nearly frictionless (see Step 4).

Deprecated global button/card classes (`.btn`, `.btn-primary`) — re-run with the
corrected regex, exact space/quote-delimited match:
- `btn`: 41 real occurrences. `btn-primary`: 15.
- Files (7, sampled by hand from the corrected grep, not exhaustive of every line):
  `src/components/admin/AdminMediaLibrary.vue`,
  `src/components/admin/facturacion/OrderFiscalMLBlock.vue`,
  `src/components/admin/ImageUploader.vue`,
  `src/views/admin/InmobiliariaDashboardView.vue`,
  `src/views/admin/NewsletterSettingsView.vue`,
  `src/views/admin/properties/PropertiesListView.vue`,
  `src/views/admin/properties/PropertyFormView.vue`.
- Several of these mix the deprecated `.btn` class with **raw, untokenized** Tailwind
  defaults instead of any of the 4 token systems, e.g.
  `src/views/admin/properties/PropertiesListView.vue:304`:
  `class="btn bg-red-600 text-white hover:bg-red-700"` and
  `src/components/admin/AdminMediaLibrary.vue:127`:
  `bg-green-100 text-green-500` — instead of `bg-error`/`text-error`/`domain-*` or
  `bg-success`/`text-success`. This is the strongest single piece of evidence that the
  "Inmobiliaria"/properties module and the media-library component are working from a
  different (older) rulebook than the rest of the app — carried into Step 5 as a
  contradiction, not silently decided.

Stray inline hex literals (`bg-[#…]`/`text-[#…]`/etc., i.e. colors that aren't in *any*
of the 4 systems): 43 occurrences, 21 distinct hex values. Most are legitimate
third-party brand colors that shouldn't be tokenized at all (Facebook `#1877F2`/`#0866FF`,
WhatsApp `#25D366`, Instagram-adjacent `#dbdbdb`/`#dadde1`/`#1c1e21`/`#050505`), but a
handful (`#262626`, `#8e8e8e`, `#f2f2f2`, `#a8a8a8`, `#fafafa`) are just more untracked
grays duplicating `ink-*`/`neutral-*`/`warm-*` with a fifth, completely untokenized set
of values. I did not chase down every one of the 21 — flagging the bucket and its size
rather than resolving each one, since several are clearly out-of-scope brand colors and
distinguishing all of them by hand across every call site was outside what this pass
could cover.

### Type scale

Tailwind's own default scale, still in heavy live use:

| class | count | | class | count |
|---|---|---|---|---|
| `text-xs` | 460 | | `text-2xl` | 151 |
| `text-sm` | 801 | | `text-3xl` | 64 |
| `text-base` | 184 | | `text-4xl` | 19 |
| `text-lg` | 150 | | `text-5xl` | 10 |
| `text-xl` | 102 | | **sum** | **1941** |

The custom "v2/doc47" scale (the one `tailwind.config.js` comments call current):

| class | count | | class | count |
|---|---|---|---|---|
| `text-ui` | 2644 | | `text-title` | 74 |
| `text-caption` | 2984 | | `text-body-lg` | 23 |
| `text-micro` | 662 | | `text-lead` | 22 |
| `text-small` | 398 | | `text-subtitle` | 17 |
| `text-body` | 138 | | `text-table` | 25 |
| `text-heading` | 156 | | `text-hero` | 1 |
| | | | `text-2xs` | 0 |
| | | | **sum** | **7144** |

Both scales are simultaneously alive at real scale (1941 vs 7144) — the doc47 scale
is dominant by volume but the Tailwind default scale is not a rounding error, it's
~1900 real call sites.

Arbitrary pixel sizes (`text-[Npx]`), aggregated across the whole tree:

| value | count | value | count | value | count |
|---|---|---|---|---|---|
| 13px | 92 | 15px | 29 | 9px | 3 |
| 11px | 76 | 18px | 5 | 26px | 3 |
| 14px | 74 | 16px | 5 | 31px | 2 |
| 10px | 68 | 21px | 4 | 24px | 2 |
| 12px | 61 | | | 17px | 2 |
| | | | | 8px/34px/33px/12.5px | 1 each |

**430 total arbitrary-pixel text sizes**, across 17 distinct raw values. These
concentrate hard in the `.nl`-line components — e.g. `src/components/productos-nl/ProductoExpandPanel.vue`
alone has `text-[11px]`×21, `text-[13px]`×10, `text-[10px]`×11, `text-[12px]`×15,
`text-[14px]`×5, `text-[21px]`×4 — **six different hardcoded pixel sizes in one file**.
This matters because `nueva-linea.css` *defines* a type scale for exactly this purpose
(`--nl-t-11` through `--nl-t-33`, with a comment explaining the numbers were
"extraídos MECÁNICAMENTE" from a Pencil export) — but I grepped for consumption of
`nl-t-` anywhere in `src` outside `nueva-linea.css` itself and got **zero hits**. The
`.nl` line's own declared type-scale tokens are completely unused; the components that
are supposed to consume them hardcode raw arbitrary pixels instead, and those pixels
don't even consistently match the token scale (`21px` appears in the arbitrary-value
data but nowhere in `--nl-t-*`).

### Radius

Named Tailwind radius classes:

| class | px | count | generation |
|---|---|---|---|
| `rounded-ctl` | 10 | 1832 | v2/doc47 |
| `rounded-card` | 14 | 984 | v2/doc47 |
| `rounded-pill` | 9999 | 447 | semantic alias (v1.0) |
| `rounded-full` | 9999 | 424 | Tailwind default |
| `rounded-boutique` | 8 | 373 | v1.1 override |
| `rounded-lg` | 8 | 340 | Tailwind default |
| `rounded-xl` | 12 | 260 | Tailwind default |
| `rounded-boutique-sm` | 6 | 77 | v1.1 |
| `rounded-boutique-lg` | 12 | 68 | v1.1 |
| `rounded-md` | 6 | 49 | Tailwind default |
| `rounded-2xl` | 16 | 24 | Tailwind default |
| `rounded-sheet` | 20 | 12 | v2/doc47 |
| `rounded-sm` | 2 | 8 | Tailwind default |
| `rounded-boutique-xl` | 16 | 0 | v1.1 — dead |
| `rounded-3xl`/`rounded-none` | — | 0 | dead |
| bare `rounded` | 4 | 346 | Tailwind default |

Arbitrary `rounded-[Npx]`: `[8px]`38, `[4px]`35, `[10px]`32, `[12px]`23, `[999px]`22,
`[9px]`18, `[3px]`16, `[7px]`14, `[6px]`8, `[14px]`8, `[5px]`7, `[16px]`4, `[44px]`2,
`[34px]`2, `[11px]`2 (231 total).

Two dead/parallel CSS-variable radius scales, checked by grepping for `--radius-` and
`nl-r-` usage outside their own definition files:
- `tokens.css` defines `--radius-sm/md/lg/xl/pill` (6/10/14/20/999px, numerically
  identical to `ctl/card/sheet/pill`, just named differently) — **grepped the whole
  `src` tree for `--radius-` and got exactly one match: the definition in `tokens.css`
  itself. Zero consumers.** A dead duplicate scale.
- `nueva-linea.css` defines `--nl-r-xs/sm/md/lg/lg2/ctl/card/card-lg/pill`
  (3/5/7/8/9/10/14/16/999px) — this one **is** consumed, via a
  `rounded-[var(--nl-r-X)]` pattern, in 15 files besides its own definition (confirmed
  sample: `src/components/stock-nl/StockActionButton.vue:15`,
  `class="... rounded-[var(--nl-r-ctl)] ..."`).

I did the exact per-role decision-rule work Step 4 asked for here — written up there,
not repeated twice.

### Shadows and motion (not explicitly asked for, but I harvested them for
completeness since the skill says "each family")

Shadows are the one family that is **not** meaningfully drifted: `shadow-e1` (646),
`shadow-e2` (92), `shadow-e3` (15), `shadow-e4` (58) dominate, and `tailwind.config.js`'s
named `boxShadow.e1..e4` literally wire to `var(--e1)`..`var(--e4)` from `tokens.css` —
one real source, one real consumer chain. `shadow-card`/`shadow-focus`/etc. are
secondary, intentional, named utilities layered on top of the same `--eN` scale, not a
second competing scale. Worth stating plainly since not every family here is bad.

Motion is drifted, and precisely: `tokens.css` defines `--motion-fast: 120ms` (and
`--duration-fast: var(--motion-fast)`), while `tailwind.config.js`'s
`transitionDuration.fast` is `'150ms'` — **two different numeric values for the same
semantic name "fast."** In practice: the Tailwind semantic class `duration-fast` is
used 261 times (resolving to 150ms); the raw numeric class `duration-120` is used
**1226 times** (resolving to 120ms, which is what `--motion-fast` actually says);
`var(--duration-fast)`/`var(--motion-fast)` are consumed directly (via inline style or
JS) **zero times** anywhere outside the CSS files that define them. So the value that's
actually dominant in practice (120ms, 1226 uses) isn't reachable through the "fast"
name at all in Tailwind-class form — the class named `duration-fast` gives you a
different number than the codebase's own dominant "fast" duration.

### Control height (proxy for "every button is different")

`h-7` through `h-12` aggregated across all `.vue` files: `h-7` 128, `h-8` 245, `h-9`
455, `h-10` 662, `h-11` 189, `h-12` 170 (total 1849). `--control-height: 40px` in
`tokens.css` corresponds to `h-10`, which is the plurality at 662/1849 (36%) — but that
means **64% of button/control heights in the codebase are something other than the
one value `tokens.css` declares as the standard control height**, spread across five
other values with no second-place value close to dominant either.

I did not harvest the spacing/gap scale (margin/padding/gap utilities) as its own
family beyond this control-height proxy — that's a real, disclosed cut: this pass
covers color, type-scale, radius, shadow, motion, and control-height, not the full
spacing scale, because the marginal harvesting cost was large and the six families
above already carry the report's argument with hard numbers.

## Step 3

**Instruction quoted:** "Drift report. Near-duplicates, outliers, competing patterns;
each family with its variant count, evidence, and the surfaces it touches. Order by
spread (surfaces affected), not by discovery order. Theme modes are not drift... Drift
is the unscoped, scattered variation."

Ordering by spread (broadest first), using the Step 2 numbers:

1. **Type scale** (touches essentially every view/component that renders text — the
   broadest possible surface). Two full scales alive at once (1941 Tailwind-default vs
   7144 doc47-custom occurrences) plus 430 arbitrary pixel values concentrated in the
   `.nl` line, whose own declared type tokens (`--nl-t-*`) are never consumed.
2. **Gray/neutral colors** — 2009 (`neutral-*`) + 2308 (`warm-*`) + 6821 (`ink-*`)
   occurrences split across at least 49+39 files for the raw scales alone, i.e. the
   literal "hay como mil grises" complaint, quantified.
3. **Radius** — every one of `ctl`/`card`/`sheet` (the "current" generation) has a
   same-value or same-role competitor still in heavy live use: `card`(14px,984) vs.
   `boutique`(8px,373)+`lg`(8px,340)+`[8px]`(38) ≈ 751 competing 8px-family uses for
   the same "card" role; `sheet`(20px,12) is *outnumbered* by `boutique-lg`(12px,68)
   for the same documented "modales, drawers" role. `pill`(447) vs `full`(424) vs
   `[999px]`(22) is a pure naming split with no value disagreement — the easy case.
4. **Motion durations** — `duration-fast`(150ms,261) vs. dominant `duration-120`
   (120ms,1226), with the CSS-variable version of "fast" (`--motion-fast`,120ms) never
   consumed as a class or var() anywhere live. Narrower in surface count than 1–3 but
   sharper in numeric contradiction.
5. **Buttons as a component category** — no shared generic `Button.vue` exists
   anywhere in `src/components/ui/` (I listed all 31 files in that directory; the only
   button-shaped components are `IconButton.vue` and two module-scoped ones,
   `servicio/SButton.vue` and `stock-nl/StockActionButton.vue`). 1902 `<button` tags
   exist in `.vue` templates. Control height alone varies across 6 values with no
   majority (Step 2). This is the closest one-to-one match to the owner's literal
   words ("cada botón es distinto").
6. **Deprecated global classes still alive** — `.btn`/`.btn-primary` (41/15 real
   occurrences, corrected count) in at least 7 files, several combined with raw
   untokenized Tailwind reds/greens instead of any of the 4 systems. Narrower surface
   (7 files) but a clear, explicit contradiction of the codebase's own
   "PROHIBIDO usar" comment.
7. **Two fully dead CSS-variable scales**: `tokens.css`'s `--radius-*` (zero consumers
   anywhere) and the orphaned `src/assets/styles/main.css` (zero importers anywhere).
   Not "drift" in the sense of scattered inconsistent usage — nobody's using these
   wrong, because nobody's using them at all — but they're exactly the kind of dead
   weight that makes a codebase look like it has more competing systems than it
   actually has live traffic on.

Not drift, and explicitly excluded per the skill's own carve-out: the two
`:root[data-tenant="…"]` blocks in `tokens.css` are a scoped reassignment of the same
`--tenant-*` variables for two named tenants — an intentional mode, going to
frontmatter `modes:` in Step 6, not counted above.

## Step 4

**Instruction quoted:** "Propose tokens. Semantic role + frequency decide: the
dominant or correct variant becomes the token; the other variants become migration
targets, NOT tokens... Confirm names and palette intent with the owner before writing
the file."

**I could not do the "confirm with the owner" half of this instruction.** This is a
cold, unattended run with no live owner in the loop — the task that sent me here is
not a simulated conversation with the repo's owner, so there was no one to confirm
names or palette intent with. I proceeded by applying the decision rule
mechanically and flagging every case where the rule's output is genuinely uncertain as
an open Decision (Step 5) instead of silently picking a side. That is a deviation from
what the skill says to do (confirm before writing), forced by the run's setup, and I'm
recording it rather than pretending a confirmation happened.

**Decision rule I actually applied:** for each semantic role, take the variant with
(a) the largest live occurrence count, and (b) alignment with the codebase's own most
recent self-declared "source of truth" comment, when (a) and (b) agree. When they
disagree, or when the counts are close enough that frequency alone doesn't settle it,
I did not elect a token — I carried it to Step 5 as an open question instead, per the
skill's explicit instruction that owner-contradicting patterns are "the owner's call,
not yours."

### Type scale — worked example

Candidates for the general body/UI text role: `text-ui` (2644, doc47) vs `text-sm`
(801, Tailwind default) vs `text-body` (138, doc47). `text-ui` wins on both frequency
and "most recent source of truth" — **elected `ui` as the token** (0.9375rem/15px,
line-height 1.5rem). Tailwind-default classes (`text-xs`/`sm`/`base`/`lg`/`xl`/`2xl`/
`3xl`/`4xl`/`5xl`, 1941 combined occurrences) become migration targets, not tokens —
each needs a per-call-site judgment about which doc47 role it's standing in for
(caption vs. small vs. body vs. heading), which is exactly the kind of row-by-row work
Step 7's migration plan is for, not something to resolve here.

The `.nl`-line arbitrary pixel values (430 occurrences, concentrated in
`productos-nl`/`stock-nl` components) are a harder case: they don't map onto the doc47
scale at all (doc47 has no defined size below 12px/`text-micro`≈11px equivalent, but
the arbitrary data includes 8px/9px/10px used 68+3+1 times). They *do* roughly track
`nueva-linea.css`'s own `--nl-t-11` through `--nl-t-18` scale in value, but that scale
is never actually consumed as I found in Step 2. I did **not** elect either the doc47
scale or the `--nl-t-*` scale as *the* token for `.nl`-rooted views — that's a genuine
fork in the road (does `.nl` inherit doc47's type scale, or does someone finally wire
up its own long-declared-but-unused scale?) and I've carried it into Step 5 as an open
question rather than picking one.

### Radius — worked example (exact, as asked)

Role: **control** (buttons/inputs/selects). Candidates: `rounded-ctl` (10px, 1832) vs
`rounded-lg` (8px, 340, and explicitly mandated by the v1.1 doc's own "Reglas duras":
"Inputs siempre `rounded-lg` (8px). Nunca `rounded-boutique`") vs arbitrary `[10px]`
(32) vs `rounded-[var(--nl-r-ctl)]` (10px, subset of 15 `.nl` files). **Elected `ctl` /
10px** — it's both the overwhelming frequency leader (1832 vs. everything else
combined) and the value the codebase's most recent comment layer calls current. The
340 `rounded-lg` uses are not nothing (they represent a whole earlier documented
"hard rule" that was superseded, per the v1.1 doc, by doc47 without — as far as I can
tell from the code — a completed migration), but frequency + recency both point the
same way here, so this one I did elect outright.

Role: **card**. Candidates: `rounded-card` (14px, 984, doc47) vs `rounded-boutique`
(8px, 373, v1.1) + `rounded-lg`-as-card-context + `rounded-[8px]` (38) — a combined
8px-family cluster of roughly 751 occurrences. 984 is larger than 751, so by pure
frequency `card`/14px wins and I'm listing it as the elected token — **but** I'm
flagging this one as thinner than the control-radius case: v1.1's doc explicitly and
deliberately assigned 8px to cards with a two-paragraph design rationale (comparing
against Sanity Studio, Stripe, Shopify), i.e. this wasn't sloppy drift, it was a
considered decision that a *later* doc (doc47) apparently overrode without the
migration having caught up. 751 vs 984 is not a rounding error. I elected `card`/14px
per the rule as stated, but this is exactly the kind of near-tie the skill would want
an owner's eyes on before treating the 8px family purely as migration debt — noted
again in Step 5.

Role: **modal/drawer**. Candidates: `rounded-sheet` (20px, 12, doc47 — the fewest raw
occurrences of any named radius token in the whole harvest) vs `rounded-boutique-lg`
(12px, 68, v1.1, also with its own explicit "Modales SIEMPRE `rounded-boutique-lg`
(12px)" hard rule). Here frequency and "most recent doc" **disagree** outright: doc47
says 20px is current, but the codebase overwhelmingly still builds modals at 12px
(68 vs 12, a >5:1 ratio in the *opposite* direction of the other two roles). I did
**not** elect a token for this role — it's a straight contradiction between what the
most recent comment claims and what the code actually does at real scale, and per
Step 5's rule that's the owner's call, not mine.

Role: **pill/full**. `rounded-pill` (9999px, 447) vs `rounded-full` (9999px, 424) vs
`rounded-[999px]` (22). No value disagreement at all, just three spellings of the
identical number — **elected `pill`** (the semantic name) as token; the other two are
pure rename migrations, the cheapest fix in the whole plan.

### Colors — not worked in the same line-by-line depth as type/radius (per the task's
explicit ask, only type-scale and radius needed that precision), but the headline
decision: **elected `ink-muted`/`ink-secondary`/`ink-heading`/`ink`/`ink-disabled` and
the full `tokens.css` semantic set (`surface-*`, `line-*`, `chrome-*`, `tenant-*`,
`state-*`, `domain-*`) as the token set**, since it's both the largest semantic cluster
and the file's own declared source of truth, with `neutral-*` (2009) and `warm-*`
(2308) as migration targets — this is the skill's own example almost verbatim ("7
grays in the code is one `ink-muted` token plus six rows in the plan"), except here
it's two whole competing *scales* (not stray grays) that become migration rows, which
I noted in Step 7 as a materially bigger migration than the skill's own example implies.

## Step 5

**Instruction quoted:** "Backfill Decisions. Read each surface. A pattern the code
evidences consistently... becomes a dated entry... A pattern the code CONTRADICTS...
is the owner's call, not yours: record it as an open question with both variants and
their locations."

Same scale problem as Step 2: there are 157 view files and at least 15 files literally
named `*DetailView.vue`. "Read each surface" at the letter of the instruction would
mean opening on the order of 150+ files. I did not do that. What I actually did: I
picked the one recurring structural pattern the skill's own example describes almost
exactly ("the same back button in three views") and checked it against every file it
plausibly applies to, rather than reading every view for every possible pattern. That
is a deliberate, bounded substitution for "read each surface," and I'm stating the
substitution rather than presenting it as if I'd read all 157.

**What I checked:** `src/components/ui/DetailHeader.vue` — a shared component that
renders a back-link + title + badges/meta/actions slot, using the current token set
(`rounded-ctl`, `text-ink-muted`, `text-heading text-ink-heading`,
`focus-visible:ring-tenant`). I found all 15 files named `*DetailView.vue` and checked
each one for `DetailHeader` usage directly (not sampled):

- **Uses `DetailHeader`** (4 of 15): `ProveedorDetailView.vue`, `ShopCajaDetailView.vue`,
  `ShopEmpleadoDetailView.vue`, `ShopSucursalDetailView.vue`.
- **Does not** (11 of 15): `CrmClienteDetailView.vue`, `CrmEventoDetailView.vue`,
  `OrdenCompraDetailView.vue`, `properties/PropertyDetailView.vue`,
  `rentals/ContractDetailView.vue`, `rentals/RentalClientDetailView.vue`,
  `ShopClienteDetailView.vue`, `ShopOrderDetailView.vue`, `ShopProductDetailView.vue`,
  `ShopReclamoDetailView.vue`, `ShopTransferenciaDetailView.vue`.

This flips what I expected going in (I initially misread an earlier `grep -rl
DetailHeader src/views` count of 11 files as "11 of the 15 DetailViews use it" — it's
actually 11 files *total* across the whole `views/` tree, including three
Factura/NotaCredito *form* views that aren't named `*DetailView.vue` at all, and only
4 of the 15 actual DetailViews are among them). Checked one of the 11 holdouts,
`src/views/admin/ShopOrderDetailView.vue`, directly: it hand-rolls its own header with
a breadcrumb-style back link (`RouterLink to="/admin/shop-orders"` + separate "Volver"
link with `Icon name="arrowLeft"`) rather than `DetailHeader`'s simpler
`back-to`/`back-label` props — a structurally different pattern, not just a restyled
one.

**This is a contradiction, not a confirmed convention** — the majority (11/15, ~73%)
of detail views roll their own header, a minority (4/15) use the shared component, and
I did not pick a winner. Recorded as an open Decision:

> **Open question — detail-view header pattern.** 4 of 15 `*DetailView.vue` files use
> the shared `DetailHeader.vue` component (`ProveedorDetailView.vue`,
> `ShopCajaDetailView.vue`, `ShopEmpleadoDetailView.vue`, `ShopSucursalDetailView.vue`);
> 11 hand-roll their own back-link/title markup (`CrmClienteDetailView.vue`,
> `CrmEventoDetailView.vue`, `OrdenCompraDetailView.vue`, `properties/PropertyDetailView.vue`,
> `rentals/ContractDetailView.vue`, `rentals/RentalClientDetailView.vue`,
> `ShopClienteDetailView.vue`, `ShopOrderDetailView.vue`, `ShopProductDetailView.vue`,
> `ShopReclamoDetailView.vue`, `ShopTransferenciaDetailView.vue`). Owner needs to say
> whether `DetailHeader` is the target (and the 11 migrate to it) or whether it's the
> minority pattern that should be retired.

The second open question I recorded, already evidenced in Steps 2–4: the deprecated
`.btn`/raw-Tailwind-color pattern in the Inmobiliaria/properties module and
`AdminMediaLibrary.vue`/`OrderFiscalMLBlock.vue`/`ImageUploader.vue`/`NewsletterSettingsView.vue`
versus the tenant-accent/state-token pattern everywhere else — recorded as an open
question rather than assumed to be "obviously wrong and should just be migrated,"
since it's possible (I can't tell from here) that Inmobiliaria is an older module
intentionally left on the previous system pending a scheduled redesign, which would
make it a planned-legacy island rather than an accident.

Third: the modal/drawer radius reversal from Step 4 (`sheet`/20px documented-current
vs. `boutique-lg`/12px actually-dominant) is also carried here as an open question,
not resolved.

I found no pattern in this pass that was consistent enough across enough surfaces to
write as a settled, dated Decision without an open question attached — every
consistent-looking pattern I checked (DetailHeader) turned out to be a minority
pattern once I checked all matching files instead of a sample, which is itself worth
recording: assuming a shared component is "the convention" because it exists and looks
official is exactly the trap Step 5 is warning against.

## Step 6

**Instruction quoted:** "Write + gates. DESIGN.md in the spec format: frontmatter
tokens (quoted hex), prose sections in spec order, `## Decisions`. Then run
`design-md-gen` and `context-lint` from the Context-Engineering clone — the design
checks passing is the definition of 'the file is real', not an optional polish."

**I could not run this step's gate commands, and want to be exact about why.** The
task that sent me here fenced my scope to this repo plus the skill itself, and
explicitly barred reading this session's other repositories, "the skills repo
included." The skill names "the Context-Engineering clone" as where `design-md-gen`
and `context-lint` live — that is a different repository, entirely outside the scope
I was given for this run. Independent of the scope fence, I also checked whether
either tool exists reachable from inside the admin repo itself, in case they were
vendored locally: `grep -rn "design-md-gen\|context-lint" .` (excluding
`node_modules`) — zero hits; `package.json`'s `scripts` block has `dev`, `build`,
`test:smoke`, `preview`, `serve:dist`, `test`, `test:watch`, `qa:visual` — none of
these are the design-lint gate; `dev-tools/` contains exactly one file
(`arrow-audit.html`), unrelated. So even without the scope fence, there is no evidence
this repo has local access to those tools; they live in a separate clone I was not
given permission to look at.

**Consequence, stated plainly rather than glossed over:** the DESIGN.md draft below
(Step 7's deliverable) is unverified against the actual generator/lint gate. Per the
skill's own judgment note — "If the repo has no build to verify against, say so in
the plan — never silently skip a verification step" — I'm saying so here. I also could
not verify the exact frontmatter YAML schema the convention expects, since that's
defined in `Context-Engineering reference/design-md.md`, also out of scope. The
frontmatter/section structure below is my best-effort reconstruction from the skill's
own prose description ("frontmatter tokens (quoted hex), prose sections in spec order,
`## Decisions`") plus the shape of this repo's own `docs/admin-design-system-v1.0.md`/
`v1.1.md` (which cover, in order: typography, color/states, spacing, shadows, radius,
motion, icon strategy, modal strategy, primitive catalog) — not a verified match to
the actual spec.

## Step 7

**Instruction quoted:** "Migration plan + hand-off. Per-surface batches, effort-sized,
with exact find→replace rows per fix (mechanical to execute), and ZERO code edits in
this skill. Execution belongs to `designing-consistently`... State the metric: re-run
the harvest after each batch; the drift finding count must go down; done at zero
findings + lint PASS."

Zero code edits were made in this run — consistent both with this instruction and with
the read-only constraint I was given. Batches below are sized against the real counts
harvested in Step 2, not estimates.

**Batch 1 — gray scale consolidation (largest batch by volume).**
Find `neutral-{N}` → replace with the nearest `ink-*`/`surface-*`/`line-*` semantic
role per call site (2009 occurrences, 49 files). Find `warm-{N}` → same (2308
occurrences, 39 files). This is not a single mechanical find→replace — each call site
needs a semantic-role judgment (is this text, a border, a background?) — so "exact
find→replace rows" here means one row *per resolved semantic mapping*, e.g.:
`warm-500` (text context) → `text-ink-secondary`; `warm-200` (border context) →
`border-line-subtle`; `neutral-100` (bg context) → `bg-surface-2`; etc. — a mapping
table the owner should confirm before batch execution, since guessing wrong here
silently changes contrast ratios (`tokens.css` documents AA/AAA contrast numbers next
to several of the `ink-*`/state values that the raw scales don't carry). Effort: large
— 88 files touched between the two scales, no single mechanical regex covers it.

**Batch 2 — type-scale consolidation.**
Find `text-xs`/`text-sm`/`text-base`/`text-lg`/`text-xl`/`text-2xl`/`text-3xl`/
`text-4xl`/`text-5xl` (1941 occurrences) → map each to the nearest doc47 role
(`text-caption`/`text-small`/`text-body`/`text-title`/`text-heading`/`text-hero`) by
call-site judgment, same caveat as Batch 1. Separately, find the 430 arbitrary
`text-[Npx]` occurrences, concentrated in `.nl`-line files
(`ProductoExpandPanel.vue`, `IntegrationsMetaView.vue`, `DescuentosPanel.vue`,
`ShopStockView.vue`, `StoreSettingsView.vue`, `CuentaCorrientePanel.vue`,
`StockBranchExpansion.vue`, and others) → **blocked on the open question from Step 5**
(does `.nl` migrate to doc47's scale, or does someone finally wire up `--nl-t-*`?)
— do not batch this until that's answered, or the batch will need to be redone.

**Batch 3 — radius, control role (small, mechanical, safe to run first).**
Find `rounded-lg` where used as a control (button/input/select) → replace
`rounded-ctl` (340 call sites, but requires excluding the `rounded-lg` uses that are
legitimately non-control, e.g. thumbnail/avatar per v1.1 §5.1's own table — read each
hit, don't blanket-replace). Find `rounded-[10px]` → `rounded-ctl` (32, safe blanket
replace, same value). Find `rounded-[var(--nl-r-ctl)]` → `rounded-ctl` (subset of 15
files, safe, same value, removes a dependency on a CSS var only reachable via arbitrary
syntax). Effort: small.

**Batch 4 — radius, pill/full (cheapest fix in the whole plan).**
Find `rounded-full` → replace `rounded-pill` (424, blanket-safe, identical value).
Find `rounded-[999px]` → replace `rounded-pill` (22, blanket-safe). Effort: trivial.

**Batch 5 — radius, card and modal roles — BLOCKED on owner decision (Step 4/5).**
Do not batch `rounded-boutique`(373)/`rounded-lg`-as-card(subset of 340)/`rounded-[8px]`(38)
→ `rounded-card`, and do not batch `rounded-boutique-lg`(68) → `rounded-sheet`, until
the owner has actually picked a winner for each role — batching now risks re-doing 984
vs. 751 and 68 vs. 12 worth of surface area a second time if the owner picks the other
side.

**Batch 6 — motion duration reconciliation.**
Either change `tokens.css`'s `--motion-fast` from 120ms to 150ms (matching the
Tailwind `duration-fast` class and requiring 1226 call sites of `duration-120` to be
re-decided against a single owner-approved "fast" value), or change
`tailwind.config.js`'s `transitionDuration.fast` from 150ms to 120ms (matching the
dominant `duration-120` and requiring only the 261 `duration-fast` sites to be checked
for whether 120ms is actually acceptable there). The second direction touches far
fewer call sites (261 vs. 1226) — flagging that as the cheaper direction, but still an
owner call since it's a design decision (does the app's actual fast-transition feel
like 120ms or 150ms), not just an engineering cleanup.

**Batch 7 — dead code removal (zero risk, no visual change).**
Delete or actually wire up: `tokens.css`'s `--radius-*` (zero consumers, confirmed);
`src/assets/styles/main.css` (zero importers, confirmed — I did not read its content
since it's dead, so I can't say whether it's worth salvaging any rules from it before
deletion — that content check is a prerequisite the owner or a follow-up pass should
do before deleting, not a step I skipped carelessly). Effort: trivial, but "worth
salvaging first" is a real open question I'm not answering blind.

**Batch 8 — deprecated `.btn` class retirement — BLOCKED on the open question in
Step 5** (is Inmobiliaria/properties intentionally on the old system pending its own
redesign, or is it drift). 7 files, 41+15 occurrences. Do not batch until that's
answered.

**Metric, as the skill asks:** re-running the exact Step 2 grep commands after each
batch should show the corresponding count drop toward zero for that family (e.g.
`neutral-*` count should shrink batch-over-batch in Batch 1; `rounded-full` count
should hit exactly 0 after Batch 4). Zero findings + a passing `design-md-gen`/
`context-lint` run is "done" — and per Step 6, I have no verified way to run that lint
from inside this session's scope, so "done" for this repo, right now, can only be
judged by the harvest-count metric, not by a green gate. That gap should close before
anyone treats a future re-harvest's "zero findings" as equivalent to what the skill
means by it.

Execution of every batch above belongs to `designing-consistently`, per the skill —
I made zero code edits in this run, consistent with the read-only constraint I was
given independently of that handoff rule.

## The DESIGN.md I would have written

```markdown
---
name: Pegasuz Admin
tokens:
  color:
    canvas: '#F4F6F8'
    surface:
      default: '#FFFFFF'
      2: '#FAFBFC'
      sunken: '#EBEEF2'
      elevated: '#FFFFFF'
    border:
      subtle: '#E2E7ED'
      strong: '#8E9BAE'
    text:
      heading: '#131A28'
      primary: '#20293A'
      secondary: '#4A5568'
      muted: '#64748B'
      disabled: '#9AA6B8'
    chrome:
      default: '#181D28'
      hover: '#222937'
      border: '#2D3646'
      text: '#E8ECF3'
      text-dim: '#98A4B8'
    tenant:
      default: '#F85810'
      hover: '#DE470B'
      soft: '#FEEEE5'
      border: '#FBC9AC'
      contrast: '#FFFFFF'
      strong: '#C2410C'
    state:
      success: { default: '#16A34A', soft: '#EAF7EF', text: '#166534', border: '#BFE5CD' }
      error:   { default: '#DC2626', soft: '#FDECEC', text: '#991B1B', border: '#F4C7C7' }
      warning: { default: '#D97706', soft: '#FCF3E3', text: '#92400E', border: '#F1DCB4' }
      info:    { default: '#0284C7', soft: '#E9F5FC', text: '#075985', border: '#BCE0F3' }
      pending: { default: '#EA8A00', soft: '#FDF2E0', text: '#8F5400', border: '#F5DCAE' }
    domain:
      cash:      { default: '#059669', soft: '#E6F6F0', text: '#065F46', border: '#B4E5D3' }
      stock:     { default: '#0D9488', soft: '#E6F5F4', text: '#115E59', border: '#B0E0DC' }
      fiscal:    { default: '#7C3AED', soft: '#F3EFFD', text: '#5B21B6', border: '#DDD2F8' }
      ml:        { default: '#FFE600', soft: '#FFFBE0', text: '#6B5900', border: '#EBDB6B' }
      mp:        { default: '#009EE3', soft: '#E5F5FC', text: '#00618C', border: '#AEDFF4' }
      orders:    { default: '#2563EB', soft: '#EBF1FD', text: '#1E40AF', border: '#C5D6F8' }
      customers: { default: '#0E7490', soft: '#E7F4F8', text: '#155E75', border: '#B6DEE9' }
      purchases: { default: '#A16207', soft: '#F8F1E2', text: '#713F12', border: '#E5D3AC' }
      branches:  { default: '#A21CAF', soft: '#F9EDFB', text: '#701A75', border: '#EACBF0' }
      employees: { default: '#BE185D', soft: '#FCEBF2', text: '#831843', border: '#F2C4D8' }
  type:
    ui:       { size: '0.9375rem', line-height: '1.5rem' }
    table:    { size: '0.875rem',  line-height: '1.25rem' }
    micro:    { size: '0.6875rem', line-height: '0.875rem' }
    caption:  { size: '0.75rem',   line-height: '1rem' }
    small:    { size: '0.8125rem', line-height: '1.125rem' }
    body:     { size: '0.875rem',  line-height: '1.375rem' }
    body-lg:  { size: '1rem',      line-height: '1.625rem' }
    lead:     { size: '1.125rem',  line-height: '1.75rem' }
    subtitle: { size: '1.125rem',  line-height: '1.625rem' }
    title:    { size: '1.375rem',  line-height: '1.875rem' }
    heading:  { size: '1.75rem',   line-height: '2.25rem' }
    hero:     { size: '2.25rem',   line-height: '2.75rem' }
  radius:
    ctl: '10px'
    card: '14px'
    sheet: '20px'
    pill: '9999px'
  shadow:
    e1: 'var(--e1)'
    e2: 'var(--e2)'
    e3: 'var(--e3)'
    e4: 'var(--e4)'
  motion:
    fast: '120ms'
    base: '180ms'
    slow: '240ms'
modes:
  - name: emilopez
    selector: '[data-tenant="emilopez"]'
    values:
      tenant.default: '#9E1B32'
      tenant.hover: '#7F1527'
      tenant.soft: '#FBEAEC'
      tenant.border: '#EFB9C0'
      tenant.contrast: '#FFFFFF'
      tenant.strong: '#8A1729'
  - name: ecommerce-test
    selector: '[data-tenant="ecommerce-test"]'
    values:
      tenant.default: '#F06030'
      tenant.hover: '#D94E22'
      tenant.soft: '#FDECE4'
      tenant.border: '#F8C4AC'
      tenant.contrast: '#FFFFFF'
      tenant.strong: '#C2410C'
---

# Pegasuz Admin — Design System

Vue 3 + Pinia + Vite tenant admin. One build serves every tenant; the `tenant.*`
color group is overridden per tenant at the `[data-tenant]` selector (see `modes:`
above) and everything else is shared.

## Color

Canvas/surface/border/text/chrome tokens above are the single color source of truth,
matching `src/assets/css/tokens.css`, which the codebase's own comments already call
"ÚNICA fuente de verdad de color." Two legacy gray scales (`neutral-*`, Tailwind's
default, and `warm-*`, a v1.1 custom scale meant to replace it) remain in heavy
concurrent live use (2009 and 2308 occurrences respectively, across 49 and 39 files)
and are migration targets, not tokens — see Decisions and the migration plan. A third,
fully dead scale (`midnight`/`charcoal`/`silver`/`accent`, tailwind.config.js's own
"LEGACY — tolerado, no usar en código nuevo" block) has zero live usage and should be
deleted from the config outright once the deprecated `.btn`/`.card` block in
`main.css` that's their only remaining consumer is retired (see Decisions).

## Typography

The scale above (`ui`/`table`/`micro`/`caption`/`small`/`body`/`body-lg`/`lead`/
`subtitle`/`title`/`heading`/`hero`) is the elected token set. Tailwind's own default
scale (`text-xs` … `text-5xl`) remains live at real scale (1941 occurrences) and is a
migration target. `nueva-linea.css` declares its own scale (`--nl-t-11` …
`--nl-t-33`) that is never actually consumed anywhere — see Decisions for whether
`.nl`-rooted views should migrate to this scale or have their unwired one finally
adopted.

## Radius

`ctl` (10px, controls), `card` (14px, cards/panels), `sheet` (20px, modals/drawers),
`pill` (9999px, full-round) are the elected tokens. Two of these roles have
significant live competition that is NOT simply migration debt — see Decisions for
`card` vs. the 8px family and `sheet` vs. `boutique-lg`.

## Shadow / elevation

`e1`–`e4`, wired through `var(--e1)`…`var(--e4)` from the token file into
`tailwind.config.js`'s named `boxShadow` utilities. This family is consistent and not
drifted — no action needed here.

## Motion

`fast`/`base`/`slow` as declared in `tokens.css` (120/180/240ms). Note:
`tailwind.config.js`'s own `transitionDuration.fast` currently disagrees (150ms) with
this token — see Decisions.

## Components

No shared generic `Button.vue` primitive exists in `src/components/ui/`. Buttons are
built ad hoc per view/component (1902 `<button>` elements found across `.vue`
templates), with control height alone spread across 6 distinct values with no
majority. `DetailHeader.vue` is a shared header/back-link component used by a
minority (4/15) of detail views — see Decisions before treating it as "the"
convention.

## Decisions

### 2026-08-20 — Radius, control role

`rounded-ctl` (10px) is the control-radius token: buttons, inputs, selects. 1832 live
occurrences vs. 340 for the superseded `rounded-lg` (8px, the v1.1-era rule) and 32 for
raw `rounded-[10px]`. Migrate both to `rounded-ctl`.

### 2026-08-20 — Radius, pill/full role

`rounded-pill` (9999px) is the token; `rounded-full` and `rounded-[999px]` are pure
renames of the identical value (no visual change).

### Open — Radius, card role

`rounded-card` (14px, 984 uses) vs. an ~751-occurrence 8px family
(`rounded-boutique`/`rounded-lg`-as-card/`rounded-[8px]`) that was a *deliberate,
documented* v1.1 decision (Sanity/Stripe/Shopify comparison, not sloppy drift) later
apparently overridden by a newer doc without a completed migration. Needs an owner
call, not an assumption.

### Open — Radius, modal/drawer role

`rounded-sheet` (20px) is named current in the most recent source comment, but
`rounded-boutique-lg` (12px) is the one actually dominant in the code, 68 uses to 12 —
the opposite direction of every other radius role. Needs an owner call.

### Open — Type scale for `.nl`-rooted components

`nueva-linea.css` declares its own type scale (`--nl-t-11`…`--nl-t-33`) that has zero
live consumers; the components it's meant to serve hardcode 430 raw arbitrary pixel
values instead, across 17 distinct numbers that don't fully match the declared scale.
Needs an owner call: adopt the doc47 scale for `.nl` views, or finally wire up
`--nl-t-*`.

### Open — Motion "fast" duration

`tokens.css`'s `--motion-fast` (120ms) and `tailwind.config.js`'s `duration-fast`
(150ms) disagree. In practice the dominant live value is 120ms via the raw
`duration-120` class (1226 uses) — `duration-fast` the named class (261 uses) gives a
different number. Needs an owner call on which value is actually "fast" in this app.

### Open — Deprecated `.btn` class and the Inmobiliaria/properties module

`main.css`'s own comment marks `.btn`/`.btn-primary`/`.card`/`.input` "PROHIBIDO usar
en código nuevo," but 7 files still use them, several combined with raw untokenized
Tailwind reds/greens instead of `state-error`/`state-success`/`tenant-*`:
`AdminMediaLibrary.vue`, `OrderFiscalMLBlock.vue`, `ImageUploader.vue`,
`InmobiliariaDashboardView.vue`, `NewsletterSettingsView.vue`,
`properties/PropertiesListView.vue`, `properties/PropertyFormView.vue`. Could be
drift, or could be an intentionally-parked module awaiting its own redesign pass —
can't tell from the code alone. Needs an owner call.

### Open — Detail-view header pattern

4 of 15 `*DetailView.vue` files use the shared `DetailHeader.vue`
(`ProveedorDetailView.vue`, `ShopCajaDetailView.vue`, `ShopEmpleadoDetailView.vue`,
`ShopSucursalDetailView.vue`); 11 hand-roll their own back-link/title markup
(`CrmClienteDetailView.vue`, `CrmEventoDetailView.vue`, `OrdenCompraDetailView.vue`,
`properties/PropertyDetailView.vue`, `rentals/ContractDetailView.vue`,
`rentals/RentalClientDetailView.vue`, `ShopClienteDetailView.vue`,
`ShopOrderDetailView.vue`, `ShopProductDetailView.vue`, `ShopReclamoDetailView.vue`,
`ShopTransferenciaDetailView.vue`). Needs an owner call on which is the target.

### Not drift — tenant color overrides

`[data-tenant="emilopez"]` and `[data-tenant="ecommerce-test"]` in `tokens.css` are a
scoped reassignment of the same `tenant.*` variables for two named tenants —
intentional, documented in `modes:` above, not a finding.
```

## What I would have committed

- `DESIGN.md` (new file, repo root) — the content above.

Nothing else. Per Step 7 and the read-only constraint I was given, zero migration
batches were executed and zero other files were touched, created, or deleted. The
migration plan in Step 7 names every file/count that a real pass would still need to
touch; none of them were.

## Controller-verified addenda

<!-- Facts the controller verified directly after the run, kept strictly separate from the
     runner's words above. Nothing in this section is a re-interpretation of what the transcript
     means — that is PLAN step 4's job. -->

1. **The dead documentation pointers resolve one level up — controller-verified.** The runner
   reported (Step 1) that `Documentation/UX-UI-STANDARDS.md`, `Documentation/VISUAL_QA.md`,
   `Documentation/Ecommerce/ADMIN_UX_ROADMAP.md`, and the "doc 47" cited at
   `src/assets/css/tokens.css:4` — `Documentation/Ecommerce/modulo-tienda-facturacion-arca/47-FRONTEND-DESIGN-SYSTEM.md`
   — do not exist anywhere in the admin repo. The controller verified afterwards that all four DO
   exist, at the Pegasuz monorepo root, two directory levels above the admin app:
   `UX-UI-STANDARDS.md` (9757 bytes), `VISUAL_QA.md` (3775 bytes), `Ecommerce/ADMIN_UX_ROADMAP.md`
   (10,351 bytes), and `Ecommerce/modulo-tienda-facturacion-arca/47-FRONTEND-DESIGN-SYSTEM.md`
   (57,030 bytes). The runner's observation is accurate about its own scope; the pointers are
   monorepo-root-relative while the context and source files carrying them are app-level. Both
   halves hold at once — the runner was not wrong. This is the same pattern step 2's baseline
   recorded for the first two of those files, now observed a second time and independently, over
   two more pointers.

2. **The one pointer that resolves nowhere — controller-verified.** `src/assets/css/servicio.css`
   cites an absolute path on another machine's disk, `C:\Users\nicol\rcsistemas-design-stage\NODOS.md`.
   The controller verified it does not exist on this machine. Unlike the four above, this one is
   not a scope artifact.

3. **Zero writes — controller-verified.** `git status --short` in the Pegasuz checkout returned
   zero lines after the run, and HEAD was unchanged at `00ddbcbaf`. The read-only constraint held.
