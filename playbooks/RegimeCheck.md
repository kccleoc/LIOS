# Playbook: RegimeCheck

## Purpose
Score the Bull–Bear Index and translate the band into a B/C contribution
posture. This is a gauge workflow, not a deployment workflow.

## Prerequisites
- LIOS v3.0 acknowledged
- Live state file loaded
- Prior cycle's BBI scores and band available for comparison

## Steps

1. Score the five dimensions 0–100 (higher = more bearish). One-line rationale
   each, citing a specific data point.
2. Compute the weighted index: 0.30 / 0.25 / 0.20 / 0.15 / 0.10.
3. Assign the band: Risk-On 0–35 · Constructive 36–50 · Cautious 51–65 ·
   Defensive 66–100.
4. Compare with last cycle. If the band changed, state whether the
   one-step-per-quarter rule permits an allocation change this quarter.
5. Output the implied A/B/C split — but **do not change total risky exposure**.
6. State the contribution implication: are B and C starved, normal, or favoured
   this month?
7. Do not output a dollar deployment amount. That belongs to the Health Check
   Deployment Plan (Rule 1).

## Prohibitions

- Never raise or lower total risky exposure. (L21)
- Never recommend a sale. The index is implemented by starving flow. (L21)
- Never present the band as a market call. It adjusts active risk only.

## Output format

```
BBI: XX.X (<band>)   prior: XX.X (<band>)   band change: none|one step
Market Valuation XX — <rationale>
Portfolio Valuation XX — <rationale>
Macro & Liquidity XX — <rationale>
Technical XX — <rationale>
Opportunity XX — <rationale>
B/C contribution posture: starved | normal | favoured
Confidence: XX%
```

## Related
- `../core/regime-index.md`
- `../commands/regime-recalc.md`
- `HealthCheck.md` §3
