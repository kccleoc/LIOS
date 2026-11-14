# LIOS Quick Commands

Self-contained prompts for invoking specific LIOS workflows without running the
full Health Check. Each file is paste-ready.

---

## How to Use

1. Open a new AI chat session
2. Paste `lios/v3.0.md` first (full initialization)
3. Wait for the AI to acknowledge LIOS v3.0
4. Paste your **live state file** and your snapshot
5. Paste the desired command file from this directory

Every command respects:
- Communication standards (`constitution/principles.md`)
- Locked Decisions (`constitution/constitution.md`)
- The three deployment rules (`core/deployment.md`)

---

## v3.0 Command Set

| Command | Job | Replaces |
|---|---|---|
| [`regime-recalc.md`](./regime-recalc.md) | Score the five BBI dimensions, return the band | `kelly-recalc` |
| [`health-check.md`](./health-check.md) | Full 16-section report | — |
| [`drift-check.md`](./drift-check.md) | Gap table only | old drift report |
| [`live-health-check.md`](./live-health-check.md) | Health Check against live data | — |
| [`snapshot.md`](./snapshot.md) | Generate a dated snapshot | — |
| [`portfolio-print.md`](./portfolio-print.md) | Formatted holdings | — |
| [`los.md`](./los.md) | Research an individual security | — |
| [`lfs.md`](./lfs.md) | Portfolio-fit evaluation | — |
| [`los-lfs.md`](./los-lfs.md) | Combined decision matrix | — |
| [`portfolio-rotation.md`](./portfolio-rotation.md) | Sleeve C rotation review | — |
| [`replacement-candidates.md`](./replacement-candidates.md) | Replacement Index screen | — |

## Retired

| Command | Status | Replacement |
|---|---|---|
| `kelly-recalc.md` | Retired | `regime-recalc.md` |
| `sr-rank.md` | Retired (L6 repealed) | `core/deployment.md` Rule 2/3 |
| `trigger-check.md` | Retired (L6 repealed) | `core/deployment.md` Rule 2 |

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
