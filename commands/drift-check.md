# `/drift-check` — Sleeve Drift Audit

## Purpose

Quantify how far each sleeve has drifted from its targets and
recommend rebalance actions if drift exceeds thresholds.

## When to use

- **Weekly** as a fast situational read
- After any sleeve rebalance
- After Sleeve D surgery
- Before any IC decision

## Inputs required

Live portfolio snapshot only.

## Command

---

```
Run a LIOS drift check on my portfolio.

Snapshot:
<paste live snapshot here>

Output a drift table per sleeve:
- Sleeve
- Target (%)
- Actual (%)
- Drift ($, always first)
- Drift (%)
- Action needed

Thresholds:
- < 2% from target → no action
- 2-5% → flag for next cycle
- > 5% → recommend rebalance or trim

For Sleeve A: per-holding drift (per `state.yaml: positions` filtered to sleeve: A) against A-base targets.
For Sleeve B: per-name check against `state.yaml: sleeve_b_caps`.
For Sleeve D: per-name check against D target + per-name caps if any.
For Sleeve C: actual % vs `state.yaml: sleeve_c.target_pct_of_abc` of (A+B+C) target.
For SR: balance + funded-by-source compliance check (verify
         it was NOT funded from DCA / Sleeve A sales / Kelly Reserve).

Rules:
- Dollar amounts always first, percentages second.
- Explicit directives (Rebalance / Hold / Trim) with Confidence %.
- Flag any Drift Check that would require modifying a Locked Decision.

Begin.
```

---

## What you get back

A drift table + rebalance directives. Fast — usually under 5 minutes.

## Example output shape

| Sleeve | Target | Actual | Drift ($) | Drift (%) | Action | Confidence |
|---|---|---|---|---|---|---|
| A (of A-base) | 100% | 100% | $0 | 0% | Hold | 99% |
| A: EXAMPLE-A1 | 50% | 51.2% | +$234 | +1.2% | Hold | 95% |
| A: EXAMPLE-A2 | 30% | 28.9% | −$213 | −1.1% | Hold | 95% |
| A: EXAMPLE-A3 | 20% | 19.9% | −$21 | −0.1% | Hold | 99% |
| B: EXAMPLE-B1 (cap) | ≤20% | 17.0% | OK | OK | Hold | 99% |
| C (of A+B+C) | ~5% | 4.9% | −$50 | −0.1% | Hold | 95% |
| D (of NLV) | 5% | 3.8% | −$2,419 | −1.2% | Hold | 90% |
| SR (existence check) | n/a | $X,XXX | n/a | n/a | Funded-by-source: ✅ | n/a |

## Related

- See `../lios/components/02_capital_architecture.md` — target definitions
- See `../lios/components/07_locked_decisions.md` — L2, L3 are the binding targets/caps