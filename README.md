# fuc-community

<!-- fuc-observer:progress:start -->
## Progress

![route frames](badges/route_frames.svg)
![first failure subsystems](badges/first_failure_subsystems.svg)
![layer vm](badges/layer_vm.svg)
![regression tests](badges/regression_tests.svg)

![FUC progress](progress.svg)

| Figure | Value | As of |
|---|---|---|
| Route frames native | 3226 / 3232 (99.81 %) | 2026-10-29 |
| Effect programs statically eligible | 277 / 443 (62.53 %) | 2026-10-29 |
| Unknown closure | 41 / 128 (32.03 %) | 2026-10-29 |
| Effect VM semantic support | 441 / 443 (99.55 %) | 2026-10-29 |
| Catalog support | 432 / 443 (97.52 %) | 2026-10-29 |
| Regression tests passing | 379 / 379 (100.00 %) | 2026-10-29 |
| Frozen milestones | 76 | 2026-10-29 |

### How to read the panels

Each panel is a treemap: every large box is a group whose **area is proportional to how many items it holds**, and
every small square is one item (or a fixed number of items, noted as "one cell = N"), coloured by its state:

| Colour | Meaning |
|---|---|
| 🟩 green | supported / native / pass |
| 🟨 yellow | ready or static only (not yet proven at run time) |
| 🟧 orange | fallback / needs review |
| 🟥 red | blocked / fail |
| 🟪 purple | unknown or not classified by the project's data |
| ⬜ grey | not evaluated |

The percentage in each label is the share of green squares in that box. The chips at the top summarise the panels.

**1. Route frames: native vs fallback.** Each box is a measured game route (a fixed sequence the project runs, e.g.
GSH, GCU, GGA). Squares are frames from the newest run of that route: green frames were drawn entirely by the native
renderer, orange frames fell back to the legacy path. This is the most direct measure of run-time progress. The squares
show proportions, not the order of the frames.

**2. Effect programs by semantic layer.** Every box holds the same effect programs of the game, measured against a
different layer of the port (VM semantics, resource catalog, element registers R01, K01/K02 persistence, late binding
LB01, end-of-pass texture rule EOPT, local lights LT01, and static eligibility). The layers are independent, so they are
never added together; a program is only fully native when every layer it needs is green. Purple usually means that a
layer was never evaluated for that program, not that it failed. Per program and layer, the newest project census that
reports it is used.

**3. Effect programs by first-failure subsystem.** The same programs, grouped by the subsystem that blocks each one
first (NONE = no static blocker left). Green squares are programs that are statically eligible for the native path.
The biggest non-green box is the subsystem whose fix would unlock the most programs.

**Table.** Headline figures exactly as the project records them (catalog completeness, VM semantic support, closed
unknowns, regression tests, frozen milestones). When a figure appears both here and in a panel, the table is
authoritative: some milestones publish totals without updating every program.

_Updated 2026-10-29T23:28:44Z. Aggregate figures only (counts and percentages recorded by the project's own artifacts), exported by FUC Observer. No game data, file names or paths are published. The per-program layer panel composes the newest project census per program; where a milestone published only totals, the table above is authoritative. Raw data: [progress.json](progress.json)._
<!-- fuc-observer:progress:end -->
