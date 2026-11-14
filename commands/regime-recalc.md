# `/regime-recalc` — Recompute the Regime Index

## Purpose

Score the five dimensions and return the band. Minutes, no full report.

**The output is a reading, not an instruction.** The index sizes nothing, shifts
no target, and gates no operation.

## When to use

- Every monthly cycle, for the record
- When a material macro event occurs

## Inputs

1. Live state file
2. Current market data (or note that the model will use the most recent)

## Command

---

```
Recompute the LIOS v4.1 Regime Index.

State:
<paste state.yaml here>

Score the five dimensions 0-100, higher = more bearish:
1. Market Valuation (30%)
2. Portfolio Valuation (25%)
3. Macro & Liquidity (20%)
4. Technical Conditions (15%)
5. Opportunity Set (10%)

For each: score + one-line rationale citing a specific data point.

Output:
- Weighted index (0-100)
- Band: Risk-on 0-35 | Constructive 36-50 | Cautious 51-65 | Defensive 66-100
- Change versus the prior cycle, for context only

Do NOT output a Kelly Factor.
Do NOT propose an allocation change.
Do NOT change total risky exposure.
The index has no mechanical effect (P2).

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
| **Regime Index** | **XX.X** | |
| **Band** | **<band>** | |
| Change vs prior | none / one band | |

Confidence: XX%

## Related

- `../core/regime-index.md` — full specification and prohibitions
- `../scores/Regime.md` — quick reference
- `health-check.md` §3 — dashboard placement
