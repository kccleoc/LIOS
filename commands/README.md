# LIOS Quick Commands

Self-contained prompts for invoking specific LIOS workflows without running the
full Health Check. Each file is paste-ready.

---

## How to Use

1. Open a new AI chat session
2. Paste `lios/v4.1.md` first (full initialization)
3. Wait for the AI to acknowledge LIOS v4.1
4. Paste your **live state file** and your snapshot
5. Paste the desired command file from this directory

Every command respects:
- Communication standards (`constitution/principles.md`)
- The eight rules, P1′–P8′ (`constitution/constitution.md`)
- The two operations, Top-up and Rebalance (`core/deployment.md`)

---

## v5.0 Command Set

| Command | Job | Replaces |
|---|---|---|
| [`regime-recalc.md`](./regime-recalc.md) | Score the five RI dimensions, return the band | `kelly-recalc` |
| [`health-check.md`](./health-check.md) | Full 15-section report | — |
| [`drift-check.md`](./drift-check.md) | Gap table only | old drift report |
| [`live-health-check.md`](./live-health-check.md) | Health Check against live data | — |
| [`snapshot.md`](./snapshot.md) | Generate a dated snapshot | — |
| [`portfolio-print.md`](./portfolio-print.md) | Formatted holdings | — |
| [`los.md`](./los.md) | Research an individual security | — |
| [`lfs.md`](./lfs.md) | Portfolio-fit evaluation | — |
| [`los-lfs.md`](./los-lfs.md) | Combined decision matrix | — |
| [`los-sleeve-b.md`](./los-sleeve-b.md) | Sleeve B LOS review | — |
| [`portfolio-rotation.md`](./portfolio-rotation.md) | The two legitimate rotation seams (B replacement, A view rotation) | old rotation report |
| [`replacement-candidates.md`](./replacement-candidates.md) | Replacement Index screen (Sleeve B) | — |

## Retired

| Command | Status | Replacement |
|---|---|---|
| `kelly-recalc.md` | Retired | `regime-recalc.md` |
| `sr-rank.md` | Retired (reserve repealed) | `core/deployment.md` Rebalance |
| `trigger-check.md` | Retired (trigger ladder repealed) | `core/deployment.md` Rebalance |
| `sleeve-d-review.md` | Retired (sleeve letters changed) | Sleeve B review via `los-sleeve-b.md` |
| `los-sleeve-d.md` | Retired (sleeve letters changed) | `los-sleeve-b.md` |
| `sleeve-c-review.md` | Retired (C is duration; rotation deleted) | `los-sleeve-b.md`, `replacement-candidates.md` |

Retired files are retained for historical reference and carry a banner. Do not
invoke them.

---

## Output discipline

- Conclusions first, dollars always, confidence percentage on every
  recommendation.
- Facts separated from assessments.
- A command that cannot produce a number from state must say so rather than
  estimate.
- No command may invent a fourth way to move money.
