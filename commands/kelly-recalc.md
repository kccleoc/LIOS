# `/kelly-recalc` — RETIRED in LIOS v4.1

**Status:** Superseded. Do not use.

The Kelly Factor no longer exists. It was never the Kelly criterion: it computed
no edge, no payoff ratio and no bankroll term, and it sized a flow rather than
wealth.

Use instead:

| Need | Command |
|---|---|
| Conditions gauge | `/regime-recalc` — Regime Index, 0–100 |
| This month's allocation | `/health-check` §10 — Top-up Plan (gap-driven) |

See `../core/regime-index.md` and `../core/deployment.md`.
