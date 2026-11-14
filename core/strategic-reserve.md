# Strategic Reserve — RETIRED in LIOS v4.1

**Status:** Superseded. Do not use.

---

## What this file was

`core/strategic-reserve.md` defined L6: a cash reserve funded only by external
capital, windfalls or exceptional realisations, deployed only on an enumerated
list of triggers, through a strict deployment hierarchy, with the Investment
Committee holding sole authority.

## Why it was removed

Three reasons, in order of weight:

1. **It was negative expected value as stated.** A reserve held to buy an
   equity dip is a bet that requires a large payoff ratio to beat simply being
   invested. For a typical trigger (the index falls 8%), the payoff ratio is
   around one-to-one, while the opportunity cost is the full equity premium.
   Kelly's criterion requires a payoff ratio above roughly 5.7:1 before such a
   reserve earns its place.

2. **The trigger was a hidden market-timing rule.** "Deploy when the index falls
   8%" fires after the decline, not before it, and gives one shot rather than a
   continuing mechanism.

3. **It duplicated the rebalance rule.** Once targets are set against NLV and a
   band exists, a drawdown automatically makes the defensive block overweight
   and the risky block underweight. The gap rule then sells D and Z into A, B
   and C. That is the same action, executed continuously rather than once.

## What replaced it

| Need | v3.0 home |
|---|---|
| Ballast | Sleeve D — duration |
| Liquidity | Sleeve Z — cash equivalents |
| Buying a dislocation | `core/deployment.md` Rule 2 (rebalance) and Rule 3 (discretionary) |
| Dry powder concept | Dissolved — it is a byproduct of drift, not a bucket |

**Dry powder should live in duration, not in bills.** If the purpose of the
reserve is to buy equities in a crash, hold it in the asset that appreciates in
that crash. A 0–1y Treasury instrument returns approximately zero while equities
fall; a duration-bearing bond fund rallies. Holding the reserve in bills means
arriving at the bottom with less capital than the plan assumed.

---

See also: `core/sleeves.md`, `core/deployment.md`.
