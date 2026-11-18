# Playbook: RegimeCheck

## Purpose
Score the Regime Index and record the band. This is a reporting workflow. It has
no effect on allocation.

## Prerequisites
- LIOS v4.1 acknowledged
- Live state file loaded
- Prior cycle's scores for comparison

## Steps

1. Score the five dimensions 0–100 (higher = more bearish). One-line rationale
   each, citing a specific data point.
2. Compute the weighted index: 0.30 / 0.25 / 0.20 / 0.15 / 0.10.
3. Assign the band: Risk-on 0–35 · Constructive 36–50 · Cautious 51–65 ·
   Defensive 66–100.
4. Note the change versus the prior cycle, for context.
5. **Stop.** The index does not inform Top-up, Rebalance, or any target.

## Prohibitions

- Never raise or lower total risky exposure on the reading (P2).
- Never shift a sleeve target.
- Never recommend a trade because of the band.
- Never present the band as a market call.

## Output format

```
Regime Index: XX.X (<band>)   prior: XX.X (<band>)   change: none
Market Valuation     XX — <rationale>
Portfolio Valuation  XX — <rationale>
Macro & Liquidity    XX — <rationale>
Technical            XX — <rationale>
Opportunity          XX — <rationale>

Mechanical effect on the portfolio: none.
```

## Related
- `../core/health-check.md` §3 — dashboard specification
- `../commands/regime-recalc.md`
- `HealthCheck.md` §3
