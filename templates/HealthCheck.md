# Health Check — <DATE>

**Basis:** `portfolio/state/portfolio.yaml` as of <DATE>
**NLV:** $X · **Reconciliation Δ:** $X
**RI:** XX.X (<band>) · **Glidepath:** risky 80 · B ceiling 10 · C 15 · Z 5 · A residual
**Risky now:** (A + B) / NLV = XX% — C is bonds, not risky

---

## 1. Portfolio Health

| Sleeve | Value | % NLV | Target | Δ pp |
|---|---:|---:|---:|---:|
| A conviction ETFs | | | residual | |
| B satellites | | | none — ceiling only | |
| C duration | | | | |
| Z cash equivalents | | | | |
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
| 4 | Duration (C) | 15% NLV | | |
| 5 | US-situs | below exemption | | |
| 6 | Sleeve Z floor | 3% NLV | | |

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

## 7. Sleeve C Review

Duration, currency hedge, ammunition available for Rebalance. C is not ranked:
no rotation, no Replacement Index — those judgments live in B.

## 8. Sleeve Z Review

Floor 3%. Duration ≈ 0 only. Below the floor fires an immediate Rebalance
(P7′); above the band inside the tolerance is reported, not traded.

## 9. Top-up Plan

Show `NLV'`, `G_s` per sleeve, `ΣG⁺`, and the allocation. The target set is A,
C and Z — B is excluded and never receives DCA. State whether the risky-cap
fallback applies; it allocates to C and Z.

| Sleeve | Allocation | Share of positive gap |
|---|---:|---:|

If Rebalance triggers **and executes a trade**, state that it absorbs the
contribution and Top-up is not run separately that month.

## 10. Rebalance Status

Not triggered / triggered and executed / triggered but no legal move (escalate
under P7′). Which sleeves or caps breached, by how much, and the direction of
any trade. Direction limits (P4′): C/Z → A,B yes · Z → C yes · B → A yes ·
A → B IC decision only · A → C/Z prohibited.

## 11. Look-Through Concentration

Top single-company exposures measured through the wrapper: direct holding +
fund look-through + index-embedded, ranked against the 5% cap. Two funds
holding the same company are one exposure.

## 12. Satellite Scoreboard

Sleeve B names and candidates: LOS, Replacement Index, trend, action.

## 13. Execution Calendar

Upcoming earnings, macro events, index events, scheduled reviews.

## 14. Execution Plan

| # | Action | Amount | Sleeve | Confidence |
|---|---|---:|---|---:|

## 15. Risk Assessment & CIO Resolution

Drawdown ladder in dollars: ordinary bad quarter, bad quarter, bad year, severe
drawdown. Restate that the policy accepts them. Then one paragraph: directive,
dollars, confidence percentage.
