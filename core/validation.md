# Validation (Part VI)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. This file is part of the v7.0.0 framework; where it disagrees with Part I (the constitution), Part I governs.

Every report generation runs these 30 checks against `portfolio/state/portfolio.yaml`: data checks 1–12 abort on failure; constitutional checks 13–30 report, never abort.

---

## Part VI — Validation

Every report generation runs these checks against `portfolio/state/portfolio.yaml`. The checks fall into two groups with different consequences:

- **Data checks (1–12)** test whether the state is internally consistent. **If any data check fails, generation is aborted.** Never generate a report from an unvalidated or failed state.
- **Constitutional checks (13–30)** test whether the book complies with the P-rules. **A constitutional check never aborts generation** — the report must be generatable precisely when a breach exists, so the breach can be reported. A failed constitutional check is reported in the report as a `BREACH` flag with dollars, and it starts the P7′ clock.

Every run records, in `logs/validation/`: timestamp, snapshot reference, NLV and portfolio total, reconciliation delta, validation status (PASSED / FAILED), and per-check results with details.

### Data checks (abort on failure)

**Check 1 · Position count.** The number of positions equals the count in the `positions` array, and matches the count recorded at the last validated report (stored in `logs/validation/`). A change in count must be explained by a recorded trade, mandate leg, or Rebalance. Failure: count differs from the last validated count with no recorded cause, or is zero.

**Check 2 · Share count.** Every position has a positive, numeric share count. Exception: a position carrying `status: planned` is a target, not a holding — skip this check for it. **`status: planned`** (used in Checks 2, 3, 8 and 9) means a planned position, not yet a holding: its market value is zero by definition, it is excluded from Check 9's sum, and it is automatically cleared on its first fill or at the next quarterly review. A position may only be `planned` before its first fill.

**Check 3 · Average cost.** Every position has a positive, numeric average cost. Exception: `status: planned`.

**Check 4 · Sleeve assignment.** Every position is assigned to a valid v7 sleeve (`a`, `b`, or `c`). Any other value — including `z` — is a file that was not migrated.

**Check 5 · Sleeve allocation and risky target.** Sleeve totals recomputed from `positions[]` equal the stored `computed.sleeve_*` (±$1.00). Recomputed NLV equals the stored `nlv` (±$1.00). A target equals `risky_target × NLV − B_actual_value` (dollars); C target equals `NLV × (1 − risky_target)` (the derived complement, used only to compute the gap) — both recomputed from holdings, never trusted from the stored block. This check is arithmetic only: a negative A target and a glidepath-band mismatch are constitutional conditions, tested by Check 28 — they never abort generation here.

**Check 6 · Sleeve risk-factor purity.** Each sleeve holds instruments consistent with its definition, by the instrument taxonomy in Part III. Failure: a cash-like instrument (duration < 1y) assigned to sleeve `a`; an individual equity assigned to sleeve `a` or `c`; a duration-bearing instrument (duration > 1y) assigned to sleeve `a` or `b`; a broad-index ETF assigned to sleeve `b`; **a sector/thematic ETF assigned to sleeve `b`**; **a narrow/single-theme ETP assigned to sleeve `a`**; **any equity-beta instrument (broad-index or thematic ETF, individual equity) assigned to sleeve `c`**; **any cash or cash-like instrument (duration < 1y) assigned to sleeve `b`**. Explicitly not a failure: cash and cash-like instruments (duration < 1y) held in sleeve `c`. Duration inside C is bounded by Checks 25–27, not prohibited here. Duration bullets apply only to positions that carry `duration_years` (cash/bills/bond ETF type); equity positions carry no `duration_years`.

**Check 7 · Regime Index (dashboard, optional).** *If* the state carries a Regime Index block, it is internally consistent (dimensions 0–100, weighted value matches its formula within 0.1, band matches the weighted value, no more than one band step versus the prior cycle). If the block is absent, report `Regime Index: not computed` and continue. If present and inconsistent, report it as a dashboard discrepancy — **never abort** report generation on a metric that governs no rule. A dashboard cannot be violated.

**Check 8 · Market value.** Every position has a valid, positive market value. Exception: `status: planned`.

**Check 9 · NLV reconciliation.** `nlv` equals `Σ(positions[].market_value) + cash.balance` (±$1.00). A position carrying `status: planned` has market value zero by definition and is excluded from this sum. `ibkr_nlv`, if present, is broker-reported and is **reference only**; every LIOS calculation uses `nlv`, never `ibkr_nlv`. There is one denominator.

**Check 10 · US-situs derivation.** The stored US-situs figures are derived, not asserted. The **book figure** must equal `Σ(positions where us_situs: true).market_value` plus `cash.balance` if `us_situs_includes_cash` is true (±$1.00). The **true figure** (`true_us_situs`) must equal the book figure plus the US-situs total of every listed external holding (±$1.00). **Brokerage cash is counted as US-situs** until the custodian confirms otherwise; if the custodian confirms an exemption, the flag is set false only with the custodian's answer recorded — the document attached, or its reference and date recorded in the SSOT history. A dated record alone, without the document or its reference, is not enough to flip the flag.

**Check 11 · Sleeve C duration data.** Every C position has a numeric `duration_years`. A missing or non-numeric duration is a data failure: the duration band cannot be computed from an incomplete state. The band itself is tested by constitutional Checks 25–27, not here.

**Check 12 · No borrowing.** `cash.balance ≥ 0` and `Σ(positions[].market_value) ≤ nlv` (±$1.00). A negative cash balance is a margin debit, whatever the broker calls it. Abort: "negative cash or gross exposure above NLV — the book is levered. Do not generate reports or place orders."

### Constitutional checks (report, never abort)

**Check 13 · Look-through single-company cap.** Every company's look-through exposure (direct + fund look-through + index-embedded) is at or under 5% of NLV. A value between 4.5% and 5% is reported as `AT CAP — no further addition`. **The check verifies the look-through inputs recorded in the SSOT** — fund holding weights and their dates, carried from the previous Health Check — not the report being generated, which does not exist yet. A check whose inputs are not recorded is not a check. **Recorded fund holding weights older than 120 days fail this check** — stale weights are not inputs.

**Check 14 · Single-sector cap.** Sector exposure on the GICS industry group basis is at or under 25% of NLV. Inputs verified as in Check 13 (recorded in the SSOT, not in the report being generated). **Recorded fund holding weights older than 120 days fail this check**, as in Check 13.

**Check 15 · US-situs ceiling and stop-adding line.** **True US-situs exposure** (`true_us_situs`, Check 10) is below the $60,000 ceiling. At or above the $48,000 stop-adding line, report `AT STOP LINE` and verify that no US-domiciled addition was made since crossing it. Above the ceiling is a hard breach (P7′). The check reports which figure it tested — the true figure, not the book figure — and the book figure is reported beside it.

**Check 16 · Risky band.** Risky weight `(A + B) / NLV` is within the glidepath target ± 2.5pp at the quarterly review (reported every cycle). **An undershoot beyond 3× tolerance (7.5pp) is hard** (P3′): it has a permitted cure (C → A). **An overshoot beyond 3× tolerance is reported-only**: it has no permitted cure (A → C is prohibited). **A deviation that survives two consecutive quarterly reviews is hard if it is an undershoot; an overshoot surviving two reviews is reported-only.** A shortfall is closed primarily by the Top-up tilt; a Rebalance into an underweight A also closes it where a breach trigger exists. During the 12-month age-step grace period, a risky-band deviation caused by the band change itself is reported, not hard (Part IV).

**Check 17 · Sleeve B ceiling.** B is at or under 10% of NLV. B between 10% and 12.5% (ceiling + the 2.5pp tolerance) is reported as `above ceiling, within tolerance — reported, not trimmed`. B above 12.5% at the quarterly review is a breach requiring a trim. A satellite that appreciates past the ceiling is reported every cycle.

**Check 18 · Sleeve B name count.** The count is at or under `names_max` (5). A proposed addition that would take the count above the limit is refused immediately — this is checked at the moment of proposal, not at the review.

**Check 19 · Permanently-exited list.** No position in any sleeve re-enters a ticker on the permanently-exited list (`permanently_exited`, portfolio-local, not sleeve-specific — it covers A and B tickers alike).

**Check 20 · Mandate status and windows.** No mandate was raised in a window that already has one (a `withdrawn` mandate still consumes its window). **No more than three mandates were raised in any actual calendar year**, counted by raising date in the SSOT mandate record. No mandate's sunset is more than 92 days after its raising. No mandate was raised within 90 days of the previous raising. No mandate was raised while another was `proposed` or `open`. Every open mandate has a sunset date in the future; every mandate past its sunset is `expired`, not `open`, with its failure reason recorded. **This check is detective, not preventive:** it detects a violation after the mandate is recorded; the refusal happens at proposal time, when the agent or owner refuses to record the mandate.

**Check 21 · DCA, arrears, injections, and withdrawals.** The monthly DCA is $1,500 or the amount in the latest dated SSOT record. **The DCA is never decreased: the check fails if the recorded amount is below the previous amount.** Every skipped month has a dated arrears item with its two repayment months named ($750 per month over two months), and **the two $750 repayments must appear in the Top-up history** — an arrears item with no repayments fails. The January DCA review item exists and is completed by 31 January. **Every capital injection and withdrawal in the SSOT history records its date, amount, and external source or destination.** Withdrawals draw from C before A and never from B (except on a thesis break). **Every injection and withdrawal above $5,000 has a dated IC record with all six parts (Part V).**

**Check 22 · Thesis-break reviews, IC records, and view statements.** Every open thesis-break review has a deadline date and is not past it without a decision, and every thesis-break review records a second-session verification verdict beside the owner's confirmation (Part V). Every B funding, every mandate, and every accepted breach has a dated IC record with all six parts (Part V) — including the source of dollars. **The check fails if any A→B IC record was approved within 30 calendar days of a capital injection**, or if any Sleeve A or Sleeve B holding lacks a non-empty written view statement or thesis. An accepted breach past its expiry date without a cure or a fresh written decision is reported as lapsed, and the breach is hard. A breach accepted twice without a cure is reported as `ACCEPTANCE EXHAUSTED` — the only remaining options are cure or constitutional amendment (P7′). **Any A→B funding, mandate raising, or capital injection recorded while a hard breach stands uncured and unescalated is reported as a breach of the P7′ block.**

**Check 23 · Thematic block cap.** The thematic block's share of Sleeve A is at or under 25% (`thematic_max_pct`, SSOT field `sleeves.a.thematic_max_pct`). The block's within-A target is 20% combined; within-block allocation is pro-rata to the block's existing composition.

**Check 24 · Scope.** Every investment account is either in the book or listed in the SSOT with its US-situs total. An account that is neither is a reported breach, not a label: the scope rule (P1′) is tested by this check, and an unlisted account starts the P7′ clock.

**Check 25 · Sleeve C single-holding duration cap (hard).** No C holding exceeds `duration_single_max_years` (10y). A holding above 10 years is a hard breach (P7′): cure within 10 business days by rotation inside C — replace the holding with a shorter-duration defensive instrument, or sell it and hold the proceeds as cash or bills inside C.

**Check 26 · Sleeve C average duration band (soft).** The value-weighted average effective duration of C is within `[duration_band.min_years, duration_band.max_years]` (1–6y). A value outside the band is a soft breach: reported every cycle, cured at the quarterly review by rotation inside C. The average is a style guard, tested as its own unit — not against a percentage-point tolerance, because years are not percentage points.

**Check 27 · Sleeve C long-holding value cap (hard).** Holdings with duration above 6 years do not exceed **50% of C's value**. Above 50% is a hard breach (P6′ cap): cure within 10 business days by rotation inside C. **Each C holding's `duration_years` must cite a recorded fact-sheet source (ISIN + date). A holding whose source is unrecorded is reported as UNCONFIRMED INPUT, and the 50% result computed from it is reported as unconfirmed** — the check does not assert a duration it cannot verify. (Implementation gate: the fact-sheet duration of the C holding above 6 years must be confirmed — ISIN + date — and recorded in the SSOT before tagging v7.0.0.)

**Check 28 · Negative A target and glidepath band match (moved from Check 5).** A negative A target (B above the whole risky budget) is reported as an IC-level structural failure, not clamped to zero. The risky target matches the glidepath band for the investor's age (non-overlapping bands: 45–49, 50–54, 55–59, 60–64, 65+). This is a constitutional check: it reports, never aborts — an age-band mismatch at a birthday must not abort the report that starts the grace period.

**Check 29 · Validation freshness.** No Top-up is valid without a PASSED validation run within 35 days. The latest run in `logs/validation/` must be PASSED and no more than 35 days old; otherwise the Top-up is reported as not valid and must wait for a fresh PASSED run. With no run there is no identification and no clock — this check closes that hole.

**Check 30 · Trade ledger.** Every sale is recorded in the trade ledger with its P4′ direction (C→A, B→A, inside C, A→A, A→B) and its operation (Rebalance, mandate leg). **An untagged sale is reported as BREACH.** Any sale tagged A→C is reported as BREACH — the direction is prohibited. Every sale tagged as a trim states the post-trim position size; a post-trim size below 1.0% of NLV is reported as BREACH (the P5′ trim floor). The ledger is what makes the A→C ban and the trim floor testable.

### Agent instructions

Before generating any report:

1. Load `portfolio/state/portfolio.yaml` and confirm `framework/VERSION` is v7.0.0.
2. Run all 30 checks in order.
3. If any **data** check fails → abort, record in `logs/validation/`, report the failure.
4. If any **constitutional** check fails → record the `BREACH` flag with dollars, start the P7′ clock, and proceed — the report is the instrument that surfaces the breach.
5. If all pass → record, proceed.

Never generate a report from an unvalidated state. Never generate a report from a stale derived document (P8′).

---

## Control map — defined, implemented, tested, failed

For every important control, this map shows where it is defined, where it is implemented, how compliance is tested, and what happens when the test fails. A control that cannot be traced through all four columns is not a control — it is a wish. Where no automated check tests a control, the row says so and names the manual control instead.

| Control | Defined | Implemented | Tested by | On failure |
|---|---|---|---|---|
| No override | P3′, Part I (sole constitution) | Deletion of governance.md; no file grants an override | Hostile review (Part VII) — no validation check tests this; the control is structural | The conflicting file is deleted/archived; the constitution governs |
| Agent never trades | P3′, Part VII | Owner places all orders; agent has no order capability | None — manual control: the owner places all orders | Any agent-placed order is outside the framework and must be reversed by the owner |
| Mandate windows + actual-calendar-year cap + 90-day interval + 92-day sunset | P3a rules 1, 5–8 | SSOT mandate record (windows, window-year, raising dates, last raising date, sunset) | Check 20 (detective) + refusal at proposal time | Refused when proposed; if recorded anyway, Check 20 flags it and the mandate is void |
| US-situs ceiling ($60,000) / stop line ($48,000) | P1′ scope, P6′ | Check 10 (both figures), Check 15 (true figure) | Check 15, quarterly agenda item 1 | Hard breach → P7′ 10-day cure or escalate; stop line → A buys non-US lines only |
| Look-through 5% / sector 25% | P6′ | Checks 13/14, inputs recorded in the SSOT | Checks 13/14 (weights older than 120 days fail) | Hard breach → P7′ 10-day cure or escalate |
| Risky band + 3× undershoot / two-quarter persistence | P3′, Part IV | Check 16 | Check 16, quarterly agenda item 4 | Undershoot beyond 3× tolerance or surviving two reviews: hard → P7′; overshoot: reported-only |
| B ceiling 10% (+ 2.5pp tolerance) | P3′, P6′ | Check 17 | Check 17, quarterly agenda item 1 | 10–12.5%: reported, not trimmed; above 12.5% at review: trim required |
| B name count 5 (forward refusal) | P6′ | Check 18 | Check 18 at proposal; agenda item 2 at review | Next addition refused immediately; count above limit settled at review by grandfathering as an accepted soft breach with a re-decision date (owner-confirmed); never by forced or comparative sale |
| B trim floor 1.0% NLV | P5′, Part II trim rules | Rebalance trim selection; trade ledger records post-trim size | Check 30 (trade ledger) | A trim below the floor is reported as BREACH; the position is trimmed to the floor or exited on a thesis break |
| A→C ban | P4′ | Trade ledger tags every sale with its direction | Check 30 (any A→C-tagged sale is BREACH) | Reported as BREACH; the sale is outside the framework |
| Thesis break (no score) | P5′, Part V process | Written research + second-session verification + owner confirmation + dated queue item | Check 22 (deadlines, IC records, second-session verdict) | A thesis break with no recorded research or no second-session verification is not a thesis break; an undecided review past its deadline is an open item at the next review |
| Benchmark exclusion (reporting only) | Part V agenda item 5, IC record test | Benchmark comparison lives in its own reporting section only | None — manual control: the owner reads the IC record. Check 22 verifies the record exists and is complete, not its wording | A record citing benchmark performance is incomplete; the decision does not exist |
| DCA $1,500 + never-decreased rule | Part II, amendment table | SSOT DCA change-record field | Check 21 (never decreased; arrears repayments verified) | A decrease is not permitted at all; a one-line YAML edit with no record is an error; the validation run flags the SSOT as unrecorded |
| Capital injection | Part II | Pooled with next monthly Top-up; IC record above $5,000; source test (outside the book; Mox operating cash excluded, Mox securities proceeds eligible) | Check 21 (records, source, six-part IC record above $5,000); SSOT record | A transfer made to buy the dip is not an injection — prohibited; a transfer from Mox operating cash or any operating/ready-cash account is not an injection; an injection above $5,000 without an IC record is unrecorded |
| Withdrawal | Part II | SSOT record; C first, then A pro-rata; IC record above $5,000 | Check 21 (records, withdrawal order, six-part IC record above $5,000); P7′ if a cap is breached | A withdrawal that breaches a cap triggers P7′ |
| Accepted-breach expiry | P7′ | SSOT acceptance record with expiry date | Check 22 | Acceptance lapses → breach is hard; after two acceptances without a cure, only cure or amendment remain; flagged in every report and Top-up until cured or amended |
| A→B funding (30-day / source rule) | Part III | IC record states source of dollars (sixth part); 30-day bar after a capital injection | Check 22 (30-day test; six-part record) | A transfer within 30 days of an injection is refused; an IC record without the source is incomplete |
| Thematic block cap 25% of A | P6′, Part III | Check 23; SSOT field `sleeves.a.thematic_max_pct` | Check 23 | Hard breach → P7′ 10-day cure or escalate |
| Scope (every account in book or listed) | P1′ | SSOT external-account listing | Check 24 | An unlisted account is a reported breach; starts the P7′ clock |
| C duration band 1–6y / single ≤ 10y / > 6y ≤ 50% of C | P6′ | Checks 25–27 (Check 11 verifies duration data only) | Checks 25–27, quarterly agenda item 1 | Single-holding > 10y or > 6y share > 50%: hard → P7′ 10-day cure by rotation inside C; average outside 1–6y: soft, cured at the quarterly review by rotation inside C; unconfirmed duration input reported as such (Check 27) |
| No borrowing (leverage ≤ 1.00×) | Part II | Check 12 (data check, aborts) | Check 12 | Report generation aborts; no orders placed |
| Validation freshness | P7′ | Check 29 | Check 29 | A Top-up without a PASSED run within 35 days is not valid; it waits for a fresh PASSED run |
| Trade ledger | P4′, Part VI | Check 30 | Check 30 | An untagged sale, an A→C sale, or a trim below the floor is reported as BREACH |
| Data integrity | Part VI data checks 1–12 | Checks 1–12 | Validation run | Generation aborts; failure recorded in `logs/validation/` |

---
