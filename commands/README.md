# LIOS Quick Commands

Self-contained prompts for invoking specific LIOS workflows
without running the full Health Check. Each command file is
paste-ready.

---

## How to Use

1. Open a new AI chat session
2. Paste `lios/v1.5.md` first (full initialization — current version)
3. Wait for the AI to acknowledge LIOS v1.5
4. Paste your **live portfolio snapshot** (current holdings)
5. Paste the desired command file from this directory
6. AI produces the requested output

Every command respects:
- Communication standards (`01_identity_and_communication.md`)
- Locked Decisions (`07_locked_decisions.md`)
- Facts vs Assessments separation
- Confidence % on every recommendation
- Dollar amounts always

---

## Command Reference

### v1.3 — Core portfolio commands

| Command | When to use | Output |
|---|---|---|
| [`health-check`](./health-check.md) | Monthly or after major moves | Full 16-section report |
| [`kelly-recalc`](./kelly-recalc.md) | Before each monthly DCA | New Kelly Factor + dimension scores |
| [`drift-check`](./drift-check.md) | After any sleeve rebalance or quarterly | Per-sleeve drift, $ deltas, rebalance recs |
| [`trigger-check`](./trigger-check.md) | When SR deployment is being considered | Which SR triggers (if any) are firing |
| [`sleeve-d-review`](./sleeve-d-review.md) | Quarterly or after Sleeve D surgery proposal | Replacement Index per holding + candidates |
| [`sr-rank`](./sr-rank.md) | When a trigger fires | Ranked SR deployment opportunities |
| [`snapshot`](./snapshot.md) | When updating live portfolio data | Standardized snapshot format |

### v1.4 — Research Intelligence Layer (LOS) commands

| Command | When to use | Output |
|---|---|---|
| [`los`](./los.md) | Score a single company | Standard LOS research block |
| [`los-compare`](./los-compare.md) | Picking among candidates | Side-by-side LOS table + verdict |
| [`los-top`](./los-top.md) | Quick conviction check | Ranked holdings by LOS |
| [`los-watchlist`](./los-watchlist.md) | Manage pre-deployment queue | Watchlist table + actions |
| [`los-sleeve-b`](./los-sleeve-b.md) | Monthly Sleeve B review | LOS per B holding + cap-relative action |
| [`los-sleeve-d`](./los-sleeve-d.md) | Quarterly Sleeve D review | LOS + Replacement Index per D holding |
| [`los-candidates`](./los-candidates.md) | New idea research | Workflow-gated candidate evaluation |
| [`los-replace`](./los-replace.md) | Sleeve D surgery decision | Joint LOS + RI gate evaluation |
| [`portfolio-los`](./portfolio-los.md) | Full LOS dashboard | All holdings + summary + recommendations |
| [`portfolio-print`](./portfolio-print.md) | Export portfolio for AI ingest | Standardized clean Markdown snapshot with cash breakdown + avg cost + P/L |

### v1.5 — Portfolio Construction Intelligence (LFS) commands

| Command | When to use | Output |
|---|---|---|
| [`lfs`](./lfs.md) | Score a single company's LFS | Standard LFS research block with five components |
| [`los-lfs`](./los-lfs.md) | Full candidate evaluation | Combined LOS + LFS research + Decision Matrix |
| [`portfolio-fit`](./portfolio-fit.md) | Portfolio fit analysis | All holdings by LFS with fit commentary |
| [`portfolio-balance`](./portfolio-balance.md) | Construction assessment | Economic engine map + gaps + construction score |
| [`economic-engine-map`](./economic-engine-map.md) | Map by economic drivers | Engine concentration + gap analysis |
| [`best-diversifiers`](./best-diversifiers.md) | Find top diversifiers | Candidates ranked by diversification contribution |
| [`replacement-candidates`](./replacement-candidates.md) | Find replacement options | Candidates scored by LOS + LFS + RI |
| [`portfolio-rotation`](./portfolio-rotation.md) | Capital rotation analysis | Rotate from low-LFS to high-LFS names |

---

## Recommended Cadence

| Cadence | Commands |
|---|---|
| **Weekly (5 min)** | `drift-check` |
| **Monthly** | `kelly-recalc` → `health-check` → `los-sleeve-b` |
| **Quarterly** | `sleeve-d-review` → `los-sleeve-d` → full `health-check` → `portfolio-los` → `portfolio-fit` → `portfolio-balance` → `economic-engine-map` |
| **On event** (Fed, earnings shock, selloff) | `trigger-check` → `sr-rank` if firing |
| **On new idea** | `los` → `lfs` (if LOS ≥ 70) → `los-lfs` → `kelly-recalc` if LOS ≥ 80 |
| **On Sleeve D surgery proposal** | `los-replace` |
| **Any time** | `snapshot` to update live data |
| **Before new AI session** | `portfolio-print` to export canonical snapshot |

---

## Composition

Commands can be combined in a single session:

> Run `drift-check` first, then `kelly-recalc` with current
> macro data, then decide whether to invoke `health-check`
> for a full report or `trigger-check` for SR.

---

## Custom Commands

When a recurring need emerges that doesn't fit an existing command,
add it here as a new file. Follow the same format:
- **Purpose**
- **Inputs required** (snapshot, context, etc.)
- **Output spec** (what sections, what tables)
- **Respect locked decisions** (footer reminder)

---

## Related
- See `../lios/components/06_health_check_structure.md` — what full Health Check looks like
- See `../lios/components/07_locked_decisions.md` — what commands may NOT do
- See `../snapshots/` — current portfolio data for command input