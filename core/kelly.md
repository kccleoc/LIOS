# Kelly Model v2.0 — RETIRED in LIOS v4.1

**Status:** Superseded. Do not use.

---

## What this file was

`core/kelly.md` defined the Kelly Model v2.0: five 0–100 dimensions, a weighted
score, and a six-tier mapping to a **Kelly Factor** in the range 0.00–1.00. That
factor multiplied the monthly DCA.

The file was removed in v3.0 because the model was not the Kelly criterion.
It computed no edge, no payoff ratio, and no bankroll term. It applied a
fraction of a *flow* rather than a fraction of *wealth*, and it ignored the
distance between the portfolio and its target.

A Kelly Factor of 0.25 on a book of roughly 200,000 with a monthly contribution
of 1,500 deployed 375 — about 1/139th of what a genuine 0.25 Kelly fraction of
the same book would imply. The name was doing no mathematical work.

## What replaced it

| Need | v3.0 home |
|---|---|
| Conditions gauge | `core/regime-index.md` — Regime Index (0–100) |
| "How much do I deploy?" | `core/deployment.md` — Rule 1, gap-driven |
| "When do I sell?" | `core/deployment.md` — Rule 2, band breach |
| "How big should a position be?" | `core/sleeves.md` — caps and constraints |

The five dimensions, their weights, and their 0–100 scoring survived unchanged.
Only the output and its job changed.

## Why not compute real Kelly

A genuine Kelly fraction for a permanent equity allocation is
`f* = (μ − r) / σ²` (continuous) or `w* = (1/γ)·Σ⁻¹(μ − r)` (Merton
multi-asset). Both are **linear in μ**, and μ for a broad equity index at
current valuations is not estimable to better than a range of roughly
−2% to +3% excess. That range moves `f*` across a span of 0 to 125% of wealth.

Kelly does not remove the need for a forecast; it makes the forecast's
consequences explicit and linear. That is useful as a diagnostic and unusable as
a monthly controller — which is exactly why v3.0 keeps only the ordinal gauge
and lets the gap and the glidepath do the sizing.

---

See also: `core/regime-index.md`, `core/deployment.md`, `core/glidepath.md`.
