# Health Check — <DATE>

**Basis:** `portfolio/state/portfolio.yaml` as of <DATE>
**NLV:** $X · **Reconciliation Δ:** $X
**RI:** XX.X (<band>) · **Glidepath:** risky 80 · B ceiling 10 · C residual · A residual
**Risky now:** (A + B) / NLV = XX% — C is defensive, not risky

---

## 1. Portfolio Health

| Sleeve | Value | % NLV | Target | Δ pp |
|---|---:|---:|---:|---:|
| A conviction ETFs | | | residual | |
| B satellites | | | none — ceiling only | |
| C defensive (residual) | | | | |
| **NLV** | | 100 | 100 | |

Three-line headline: where we are, what changed, what matters next.

## 2. Gap & Drift

One table. Gap and drift are the same number. Sleeve B has no target: its line
compares against the ceiling, not a target.

| Sleeve | Value | Target $ | Gap $ | Gap pp | Band | Action |
|---|---:|---:|---:|---:|---:|---|

Σ gaps must be $0.

## 3. Regime Index (dashboard)

| Dimension | Weight | Score | Rationale |
|---|---:|---:|---|
| Market Valuation | 30% | | |
| Portfolio Valuation | 25% | | |
| Macro & Liquidity | 20% | | |
| Technical | 15% | | |
| Opportunity | 10% | | |
| **RI** | | | |
| **Band** | | | |

Dashboard only: the index reports; it does not size anything (P2′).

## 4. Constraint Check

| # | Constraint | Limit | Actual | Status |
|---|---|---:|---:|---|
| 1 | Look-through single-company | 5% NLV | | |
| 2 | Single-sector | 25% NLV | | |
| 3 | Sleeve B ceiling | 10% NLV | | |
| 4 | C duration — average | 1–6 years | | |
| 5 | C duration — single holding | ≤ 10 years | | |
| 6 | US-situs | below exemption | | |

Effective exposure = direct + fund look-through + index-embedded. A hard breach
is an immediate Rebalance (P7′); a soft breach beyond
`rebalance_tolerance_pct` goes to the quarterly review and is flagged in the
interim.

## 5. Sleeve A Review

Within-sleeve weight drift and the thematic cap
(`sleeves.a.thematic_max_pct`). Every thematic holding carries a written view
statement, horizon and invalidation condition. No cash-like instrument may
appear here.

## 6. Sleeve B Review

| Ticker | Value | % NLV | Ceiling status | LOS | Thesis-exit? | Verdict |
|---|---:|---:|---|---:|---|---|

B is cap-only: no target, no Top-up. The only sell trigger is a thesis break
(P5′). State explicitly whether any name is below LOS 60 — that opens a 30-day
deep-dive review.

## 7. Sleeve C Review — one sleeve, one measurement

Composition (cash / bills / bonds, in dollars and % of C — reported, never
targeted), the duration table for every holding, the weighted average against
the 1–6y band, and **money duration in dollars** with the cost of a 1pp move.
Ammunition available for Rebalance. C is not ranked: no rotation, no Replacement
Index — those judgments live in B. Say explicitly if the composition is a
barbell, even when the average is inside the band.

## 8. Top-up Plan

Show `NLV'`, `G_s` per sleeve, `ΣG⁺`, and the allocation. The target set is A,
C — B is excluded and never receives DCA. State whether the risky-cap
fallback applies; with one defensive sleeve it allocates to C.

| Sleeve | Allocation | Share of positive gap |
|---|---:|---:|

If Rebalance triggers **and executes a trade**, state that it absorbs the
contribution and Top-up is not run separately that month.

## 9. Rebalance Status

Not triggered / triggered and executed / triggered but no legal move (escalate
under P7′). Which sleeves or caps breached, by how much, and the direction of
any trade. Direction limits (P4′): C → A yes · B → A yes · rotation inside C is
not a transfer · A → B IC decision only · A → C prohibited.

## 10. Look-Through Concentration

Top single-company exposures measured through the wrapper: direct holding +
fund look-through + index-embedded, ranked against the 5% cap. Two funds
holding the same company are one exposure.

## 11. Satellite Scoreboard

Sleeve B names and candidates: LOS, Replacement Index, trend, action.

## 12. Execution Calendar

Upcoming earnings, macro events, index events, scheduled reviews.

## 13. Execution Plan

| # | Action | Amount | Sleeve | Confidence |
|---|---|---:|---|---:|

## 14. Risk Assessment & CIO Resolution

Drawdown ladder in dollars: ordinary bad quarter, bad quarter, bad year, severe
drawdown. Restate that the policy accepts them. Then one paragraph: directive,
dollars, confidence percentage.
