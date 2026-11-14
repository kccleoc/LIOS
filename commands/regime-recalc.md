# `/regime-recalc` — Recompute the Bull–Bear Index

## Purpose

Score the five BBI dimensions and return the band. Minutes, no full report.

Replaces `/kelly-recalc` (retired). The output is a **band**, not a factor, and
it does **not** produce a dollar deployment amount.

## When to use

- Every monthly cycle, before the DCA
- When a material macro event occurs
- After a Sleeve B thesis event (changes Portfolio Valuation)
- After Sleeve C surgery (changes Opportunity Set)

## Inputs

1. Live state file
2. Current market data (or note that the model will use the most recent)
3. Any specific concerns to surface

## Command

---

```
Recompute the LIOS v3.0 Bull-Bear Index.

State:
<paste state.yaml here>

Score the five BBI dimensions 0-100, higher = more bearish:
1. Market Valuation (30%)
2. Portfolio Valuation (25%)
3. Macro & Liquidity (20%)
4. Technical Conditions (15%)
5. Opportunity Set (10%)

For each: score + one-line rationale citing a specific data point.

Output:
- Weighted BBI (0-100)
- Band: Risk-On 0-35 | Constructive 36-50 | Cautious 51-65 | Defensive 66-100
- Band change vs prior cycle (and whether the one-step-per-quarter rule allows
  an allocation change this quarter)
- The A/B/C split implied by the band
- What that means for B and C CONTRIBUTION flow this month (not a dollar
  deployment figure)

Do NOT output a Kelly Factor. Do NOT change total risky exposure.
Confidence: XX% on the band.
```

---

## What you get back

| Dimension | Score | Rationale |
|---|---:|---|
| Market Valuation (30%) | XX | <one line> |
| Portfolio Valuation (25%) | XX | <one line> |
| Macro & Liquidity (20%) | XX | <one line> |
| Technical (15%) | XX | <one line> |
| Opportunity (10%) | XX | <one line> |
| **BBI** | **XX.X** | |
| **Band** | **<band>** | |
| Band change | none / one step | |
| B/C flow implication | starved / normal / favoured | |

Confidence: XX%

## Related

- `../core/regime-index.md` — full specification and prohibitions
- `health-check.md` §3 — dashboard placement
- `../scores/Regime.md` — quick reference
