# LIOS Quick Commands (v7.0.0)

Self-contained prompts for invoking specific LIOS workflows without running the
full Health Check. Each file is paste-ready.

---

## How to Use

1. Open a new AI chat session
2. Paste the current framework version's constitution
   (`../constitution/constitution.md`, Part I) first
3. Wait for the AI to acknowledge LIOS v7
4. Paste your **live state file** and your snapshot
5. Paste the desired command file from this directory

Every command respects:
- Communication standards (`constitution/principles.md`)
- The eight rules, P1′–P8′, plus P3a (`constitution/constitution.md`)
- The two operations, Top-up and Rebalance (`core/deployment.md`)
- The agent role (`core/agent-role.md`) — the agent never trades

---

## v7.0 Command Set

| Command | Job |
|---|---|
| [`health-check.md`](./health-check.md) | Full 14-section report |
| [`live-health-check.md`](./live-health-check.md) | Health Check against live data |
| [`drift-check.md`](./drift-check.md) | Gap table only |
| [`snapshot.md`](./snapshot.md) | Generate a dated snapshot |
| [`portfolio-print.md`](./portfolio-print.md) | Formatted holdings |
| [`regime-recalc.md`](./regime-recalc.md) | Score the five RI dimensions, return the band (dashboard) |
| [`sleeve-c-review.md`](./sleeve-c-review.md) | Sleeve C review: composition, duration, ammunition |

---

## Output discipline

- Conclusions first, dollars always, confidence percentage on every
  recommendation.
- Facts separated from assessments.
- A command that cannot produce a number from state must say so rather than
  estimate.
- No command may invent a fourth way to move money.
- The agent never trades. No command places, modifies, or cancels an order.
