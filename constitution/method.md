# Method — quarterly review, thesis breaks, reporting (Part V)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. This file is part of the v7.0.0 framework; where it disagrees with Part I (the constitution), Part I governs.

Method is how the constitution is worked. It changes no cap, target, tolerance, sleeve or operation. Anything in this part that would change one of those is a constitutional amendment, not a method edit.

---

## Part V — Method (quarterly review, thesis breaks, reporting)

Method is how the constitution is worked. It changes no cap, target, tolerance, sleeve or operation. Anything in this part that would change one of those is a constitutional amendment, not a method edit.

### Research — written theses, no scores

There is no scoring system in V7. **No numeric score governs any decision**, and no re-weighting of anything. The confidence percentage on a recommendation (P8′) is a reporting standard, not a score: it states how strong the evidence is, and it never triggers, sizes, or blocks an action — and it is not evidence for a thesis break or a replacement decision. The single exception is the Regime Index (Health Check section 3), which is a dashboard: five dimensions scored 0–100 and combined as `RI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq + 0.15·Technical + 0.10·Opportunity` (higher = more bearish / more hostile), with bands 0–35 Risk-on, 36–50 Constructive, 51–65 Cautious, 66–100 Defensive. Each dimension runs the same way — **higher means more hostile**: MarketVal (market valuation: 0 = cheap against history, 100 = expensive against history); PortfolioVal (the book's own valuation: 0 = cheap against its history, 100 = expensive); MacroLiq (macro liquidity: 0 = loose and ample, 100 = tight and restrictive); Technical (trend and breadth: 0 = strong uptrend, 100 = breakdown); Opportunity (0 = many attractive opportunities, 100 = few or none — a high Opportunity score means few opportunities, which is hostile). It has no mechanical effect and gates nothing, and it is not evidence for a thesis break or a replacement decision. It is updated monthly and after a material macro event; if a dimension cannot be scored from available data, say so — do not extrapolate. Each dimension carries a one-line rationale citing a specific data point. Research in V7 is **written theses**:

- **Every B name carries a written thesis** in the SSOT note for the position: what the business must do for the thesis to hold, the horizon, and the condition that would break it. Thesis and break-condition text is **append-only and versioned**: it is never edited in place — a change adds a new dated version, and the old versions stay in the note. A break is judged against the version in force at the last quarterly review before the research opened. A B name without a written thesis is incomplete and must be completed by the next quarterly review.
- **Every A holding carries a written view statement**: intended exposure, why the basket rather than a name, horizon, and the condition that invalidates it.
- Research is documented with **evidence cited by source** (filings, earnings releases, verified reporting) and dated. A claim with no cited evidence is an opinion, and an opinion does not break a thesis.
- New B names require an Investment Committee decision and a written thesis, within `names_max` and `position_cap_pct`.

### Thesis-break process (P5′)

1. **Research.** When a B name's thesis may have broken, the agent (or owner) writes the research: what changed, the evidence with sources and dates, and which part of the written thesis it contradicts. The thesis it is judged against is the version in force at the last quarterly review before the research opened (thesis text is append-only and versioned — see Research, above).
2. **Verification.** The research is checked against the cited sources before any decision is proposed — **by a session other than the one that wrote the research** (in practice a delegated review session), given only the cited sources and the locked thesis text. Verification means the evidence says what the research claims it says. The verifier's verdict is recorded in the SSOT note for the position. **The owner then reads and confirms the cited evidence, and his confirmation is recorded beside the verifier's verdict.** A thesis break with no recorded second-session verification is not a thesis break; a second session re-deriving the research is the verification step, not a fallback.
3. **Queue.** The thesis-break review is opened as a **dated item in the execution queue** (`execution/pending.yaml`) with its deadline. A review with no deadline date is not a review.
4. **Decision.** The exit is decided at the **quarterly review** by the Investment Committee: exit (thesis broken), hold (thesis intact, research recorded), or hold with a named re-check date. The decision and its evidence are recorded in the SSOT note for the position and in the IC record.
5. **No third outcome, no drift.** A thesis-break review left undecided past its deadline is reported as an open item at the next quarterly review. It is not silently dropped.

### Quarterly Review — fixed calendar and standing agenda

**Dates.** The Quarterly Review is held on the **last Saturday of February, May, August and November**. Under this rule the reviews are Saturday **27 February 2027**, Saturday **29 May 2027**, Saturday **28 August 2027** and Saturday **27 November 2027**, then the same rule every year.

**Marks.** The review is struck on **quarter-end closing prices** — 31 December, 31 March, 30 June or 30 September, whichever falls immediately before the review — and on the **latest reported earnings** for each holding. A review that marks the book at the prices of the week it is held tests the caps against a number nobody agreed to. The quarter-end prices are the formal record; but a hard-cap breach on current values at any time triggers P7′ immediately, and where the two tests disagree, the current-value test governs for breach purposes.

**Why a weekend.** By the last Saturday of those months the reporting season is complete for the portfolio's holdings, quarter-end prices are available, and the markets are shut, so no decision taken at the review is a decision taken at a price the review created.

**Standing agenda** — the same items, in this order, every quarter:

1. **Recompute every cap at quarter-end prices.** Sleeve B at or under 10% of NLV; look-through single-company at or under 5%; single-sector at or under 25% on GICS industry group; **true US-situs below the ceiling (book figure reported beside it)** and against the $48,000 stop-adding line; Sleeve C average effective duration between 1 and 6 years, with no single holding above 10 years and holdings above 6 years at or under 50% of C; the B name count against `names_max`; **re-mark the listed external holdings (Mox) on quarter-end prices** and recompute the true US-situs figure.
2. **Thesis-break review for every B name.** Each name's written thesis is re-read against the quarter's evidence. A thesis break is decided here, per the process above. This is also where the B name count is settled: the cap is forward-looking (it refuses the next addition), and a count already above the limit is resolved here, never by a forced sale. If the count is above `names_max`, each name is tested solely against its own written thesis (Part III): only a name whose own thesis is broken by cited, owner-verified evidence is exited. If all theses remain intact, the count is grandfathered as an accepted soft breach under P7′ with a re-decision date at the next review — never by forced or comparative sale.
3. **Review accepted breaches.** Every breach accepted on the record is re-read against its expiry date. An acceptance past its date without a cure or a fresh written decision has lapsed, and the breach is hard.
4. **Check the DCA Top-up tilt and correct a C overweight.** A risky-weight shortfall (C overweight) is corrected in two lines, in order. **First, the Top-up tilt:** new money goes to A, no sale. **Second, a C→A Rebalance sale** — only if the gap is too large to close by contributions within a reasonable time; the review decides which line applies and records why. A deviation within the 2.5pp tolerance is Top-up tilt only; beyond tolerance it is a review item under this agenda point. A C overweight is never corrected by moving money to B: B never receives Rebalance money — the only route into B is an IC A→B transfer from A's pre-existing holdings (Part III).
5. **Benchmark comparison (reporting only).** Portfolio total return vs the broad-market ETF total return over the quarter and since V7 adoption, for information. (The benchmark ETF is the broad-market ETF recorded in the SSOT's adoption record; until the owner confirms it, the comparison is not run.) No action is ever triggered by this comparison. Beating the index is a wish, not a requirement, and a quarter of lagging it changes nothing. **Benchmark performance is not evidence.** It is not evidence for a thesis break, is not a factor in candidate selection, and must not appear in an IC record's rationale. The comparison lives in this section only; it is never cited in a thesis-break research note, a replacement decision, or an IC record. A decision whose rationale cites benchmark performance is not a decision under Part V — the record is incomplete.
6. **Mandate window check.** Is a mandate open? Is the current window free? Is the next window's boundary (31 January, 31 July, 31 October) approaching? No mandate is raised at a review without this check.
7. **Re-read every A holding's view statement.** Each A holding's written view statement is re-read against the quarter's evidence: is the view still valid, has the fund's mandate, benchmark, index methodology, domicile or liquidity changed materially, has its expense ratio risen materially against peers? A holding whose view is invalidated is reviewed under P5′.

**Every decision is an Investment Committee record. No record, no funding.** The record names the candidate and the sleeve, the written thesis (for B) or view statement (for A) and the evidence, the dollar amount free of the ceiling and the look-through it creates, the alternative that was rejected, and the condition that would reverse the decision.

**Hard caps are never deferred to a review.** Look-through, single-sector, US-situs, the single-holding duration cap and the > 6y holdings 50% cap fire the moment they are breached (P7′); a review cannot schedule them. Only soft limits — sleeve bands, the risky band, the B ceiling, a B count already above `names_max` and the C average duration band — wait for this date.

**Scope.** This section fixes dates and an agenda. It changes no cap, target, tolerance, sleeve or operation, and it may not be used to introduce one: those live in Part I and move only by the amendment rule there.

**Pinned.** This section is the operative definition of "quarterly" in P3′ and P7′, so it is not free method text. Changing a date, the marks convention, or the order of the agenda is a **constitutional amendment — major version bump plus an ADR — not a CHANGELOG entry.**

### Health Check — 14 sections, fixed order

1. Portfolio Health
2. Gap and Drift
3. Regime Index (dashboard)
4. Constraint Check
5. Sleeve A Review
6. Sleeve B Review
7. Sleeve C Review
8. Top-up Plan
9. Rebalance Status
10. Look-Through Concentration
11. Satellite Review
12. Execution Calendar
13. Execution Plan
14. Risk Assessment & CIO Resolution

Full specification: `core/health-check.md` (rewritten for V7; the v6 spec's numbering is retained, its scoring references are removed). Section 5 (Sleeve A Review) is the A view-statement re-read of agenda item 7. Section 11 (Satellite Review) is the written-thesis list for B — each name, its thesis, its evidence, and its review status — explicitly not a score. The Regime Index in section 3 is a dashboard: it has no mechanical effect and gates nothing. Its formula and bands are defined in the Research section above; the Health Check spec carries the reporting format only.

### Communication standard

Conclusions first. Dollars always. Facts separated from assessments. Explicit directives. Confidence percentage on every recommendation. No fluff, no motivational language, no emoji.

### Investment Committee — record test and limits

The Investment Committee is the owner plus the active AI session. All recommendations are advisory. **The IC decides only the decisions this constitution assigns to it (thesis breaks, B funding, mandates, breach acceptances); everything else is arithmetic.** There is no override in V7.

A decision exists only if all six are recorded, dated, in the SSOT's history:

1. the candidate, and the sleeve it enters;
2. its written thesis (for B) or view statement (for A), and the evidence;
3. the dollar amount, the B ceiling headroom it consumes, and the look-through it creates;
4. the alternatives considered — at least two — compared head-to-head with the candidate on thesis and evidence, and why each was rejected (for a B replacement: the candidate against the incumbent it would replace and against leaving the money in A, compared on the structured dimensions in Part III — catalyst horizon, invalidation clarity, and look-through overlap with existing holdings);
5. the condition that would reverse it;
6. the source of the dollars, where the decision moves money — for an A→B funding transfer: which pre-existing A holdings the dollars come from, and the date of the most recent capital injection (so the 30-day bar can be tested); for a mandate or an injection deployment: the source of the injected money. Where the decision moves no money, this part is recorded as "no dollars moved".

Benchmark performance must not appear in any of the six parts: it is not evidence, not a factor in selection, and not part of the rationale.

No record, no funding. A verbal or chat-only approval is a deferral, not a decision. This is deliberately bureaucratic: it is the only control on the one sleeve where judgment replaces arithmetic.

The IC may **NOT**:

- Modify a P-rule (P1′–P8′, P3a) without a major version bump and a constitutional amendment
- Skip Health Check sections (P8′ — 14-section fixed order)
- Change total risky exposure because of a market view (P2′)
- Move money outside the two deployment operations (P3′)
- Override a framework output, with or without a rationale
- Override a Sleeve B cap (P6′)
- Sell a Sleeve B name on price (P5′)
- Re-enter a ticker on the portfolio-local permanently-exited list
- Raise a transition mandate outside a free window, or while another is processing (P3a)
- Cite benchmark performance as evidence or rationale for any decision
- Cite the Regime Index or the confidence percentage as evidence for a thesis break or a replacement decision (both are reporting, not evidence)

The list above is illustrative; the remit sentence is the rule.

---
