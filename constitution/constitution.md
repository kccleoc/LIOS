# LIOS v7.0.0 — Constitution (P1′–P8′ + P3a)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. This file is the sole constitution. In any conflict between this document and any other file, this document governs. No other file grants an override, a discretionary channel, or an exception. `governance.md` is deleted and carries no authority.

---

## Preamble — what V7 is

V7 is a **rewrite, not a patch list**. V6 was reviewed hole by hole and the holes turned out to be structural: a second file granted the override the constitution forbids, the only exception mechanism had no frequency limit, the exit gate was self-scored, accepted breaches never expired, and the rules that actually bind the owner day to day lived outside the P-list with no change control. Patching those one at a time inside a broken plan would leave the contradictions in place. V7 is therefore written fresh, from the same design brief, keeping what works in V6 and deleting the rest.

**What V7 keeps from V6:** three sleeves (A/B/C) on one denominator (NLV); the monthly $1,500 DCA and the Top-up gap formula; the cap set (look-through 5%, sector 25%, US-situs below $60,000 with the $48,000 stop-adding line, B ceiling 10%, B name count 5, C duration 1–6y with any single holding ≤10y); the quarterly review on the last Saturday of February, May, August and November; the contribution-tilt glidepath; the owner places all trades.

**What V7 deletes:** every numeric score and the scoring layers; the `governance.md` override; the B wind-down at age 60; the hard-glidepath policy variant; the event-pipeline "official workflow"; the replacement ranking; the deleted sleeves and every other v4/v5 fossil (the reserve concepts, the old trigger rules, the second denominator, the old sleeve-count labels); the shadow rules — every binding rule is now either a P-rule or listed in the single amendment table, with its change control stated.

**What V7 adds:** a scope rule (the book is the IBKR portfolio; Mox is operating cash, outside the book, but its securities are listed in the SSOT with their US-situs total); mandate windows (one mandate per window; at most three mandates raised in any actual calendar year, counted by raising date; 90-day minimum interval); an expiry on every accepted breach and a cure deadline on every hard breach; a capital-injection rule and an explicit arrears repayment rate; a withdrawal rule; a dust rule that cannot fire on a defined block; validation checks that cover the constitutional caps; and an explicit agent role — advisor and checker, never trader, with reproducible outputs.

### Design brief (the owner's investment philosophy)

- The owner is in his 40s, with modest capital ($1,500/month contributions), building wealth over a 10–20 year horizon.
- Monthly DCA into mainly US and global equity ETFs. Bullish on AI and on the long-run growth of equities. The goal is to **follow the market trend with V6-like protection**. Beating the US indices is a **wish, not a requirement** — V7 contains no beat-the-benchmark target and no action is ever triggered by relative performance. A reporting-only benchmark comparison (portfolio vs a broad-market ETF, for information) is included in the quarterly review.
- The owner ignores market ups and downs and trusts the ETF's self-adjustment: losers leave the index, winners stay. Wealth accumulates by regular contribution.
- The owner follows tech news and has conviction on certain details, so he sometimes buys extra of a stock already held by an ETF into Sleeve B. Position size must be controlled. That is why B exists, why it is cap-only, and why its caps are hard.

### Handover — who implements LIOS now

The repo AI agent **no longer handles LIOS**. The owner has revoked that arrangement. V7 is drafted by Muse as the owner's advisor, and after the owner's approval Muse implements it directly — commit, push, tag v7.0.0 — in the private repo. The repo agent's old instruction files (`framework/AGENTS.md` as it stood in v6, the re-pin and push-framework flow) are **not** to be followed for LIOS. Any instruction in the repo that assumes the repo agent amends the framework is superseded by this paragraph.

### How to read this document

- **Part I (Constitution)** is the whole rulebook: eight P-rules plus the subordinate P3a. If a rule is not here or in the amendment table, it is not a rule.
- **Parts II–VII** are the working text that implements the constitution: deployment, sleeves, glidepath, method, validation, agent role. Where any of them disagrees with Part I, **Part I governs**.
- **Precedence:** hard caps (P6′) > Rebalance (P3′) > Top-up (P3′) > transition mandate (P3a). There is no other precedence rule anywhere in V7. (The same list is restated in Part II, where it is applied to the deployment operations; this paragraph is the statement, Part II is the application.)
- Dollars are stated in USD unless marked HKD. Every weight, cap, floor and ceiling is a percentage of NLV unless stated otherwise.

### Files deleted in V7

`framework/constitution/governance.md` (deleted — its override powers are the S1 contradiction; its surviving principle, "rules are permanent, decisions are dynamic", is carried as one sentence in Part I). `framework/core/research-layer.md` (deleted — the scoring framework is gone). `framework/core/replacement-index.md` (deleted — the replacement ranking is gone). `framework/core/event-pipeline.md` (deleted — it contradicted P5′ and the validation count). `framework/core/regime-index.md` (deleted — the dashboard is defined in the Health Check spec, `core/health-check.md`, which is rewritten for V7). All derived documents generated under v6 or earlier (`portfolio/current/*`, `reports/drift.md` and any file carrying a superseded rule) are archived at adoption and must not be acted on (see P8′).

---

## Part I — Constitution (P1′–P8′)

**This file is the sole constitution.** In any conflict between this document and any other file, this document governs. No other file grants an override, a discretionary channel, or an exception. `governance.md` is deleted and carries no authority.

**Rules are permanent. Decisions are dynamic.** A rule changes only by the amendment table below. A decision (a trade, a trim, a thesis-break call) is re-made every cycle on current evidence.

Eight rules. That is the whole constitution.

### P1′ · Three sleeves, one denominator — and the scope of the book

The portfolio is partitioned into three sleeves:

| Sleeve | Holds | Risk role |
|---|---|---|
| **A** | ETFs — broad-index and sector/thematic | Systematic and thematic beta |
| **B** | Satellites — individual companies and narrow bets | Idiosyncratic alpha, **cap-only** |
| **C** | Everything defensive — cash, money market, bills, bond ETFs of any duration | Ballast and ammunition, **residual** |

Every weight, cap, floor and ceiling is a percentage of **NLV**. No second denominator exists.

**Narrow bets.** A narrow bet is a single-company exposure or a single-theme/single-issuer instrument — anything whose risk is concentrated in one company or one narrow theme, as opposed to a diversified basket. A narrow bet belongs in Sleeve B, never in Sleeve A.

**A + B is the risky block. C is the defensive block, and it is the residual:** whatever risky does not take. One target, one arithmetic.

**Scope — what "the portfolio" is.** The book is the owner's **IBKR investment portfolio**, recorded in the SSOT (`portfolio/state/portfolio.yaml`). Two things are outside the book, and both are recorded, not assumed:

- **Mox is operating cash, not part of the book.** The owner keeps his ready cash for contingencies in Mox (the balance is recorded in the SSOT's scope record). It stays in Mox, it is not deployed into the book, and it is not counted in any cap. Any **securities held in Mox are outside the book but must be listed in the SSOT with their US-situs total**, so that every cap is tested against the owner's true exposure, not only the book's. The listed Mox total and the true US-situs figure are recorded in the SSOT (see Adoption position below); they are live figures, not constitution text.
- **The emergency fund is outside the book** (`outside_portfolio`). C is not an emergency fund.

**The true US-situs figure.** The SSOT carries two figures: the book figure (derived by Check 10 from SSOT positions and cash) and `true_us_situs` (book figure + the US-situs total of every listed external holding). Check 15 tests the **true** figure against the $60,000 ceiling and the $48,000 stop-adding line, and reports which figure it tested. The listed external holdings are re-marked at each quarterly review on quarter-end prices, alongside the cap recomputation.

**Adoption position.** At V7 adoption the true figure is above the $60,000 ceiling (the book figure, the true figure, the Mox total and the Mox holdings are recorded in the SSOT). That is a **hard breach on the true figure on day one of V7**, and it is acknowledged here rather than left unrecorded. **The P7′ identification clock starts at the first validation run after V7 adoption, not at the draft date.** The adoption breach acceptance must be recorded within 10 business days of adoption, with a cure or re-decision date of **2026-12-15** (following the 1 December Mox rate check already scheduled). The named cure is the December Mox decision recorded in the SSOT: sell the Mox US-domiciled holding first and deploy the proceeds into IE-domiciled lines. The proceeds enter the book as a capital injection under Part II — a sale of an external holding is eligible; Mox operating cash is not — deployed by the next monthly Top-up. If the December decision does not cure it, the breach is accepted on the record under P7′ with a re-decision date at the 2027-02-27 quarterly review. The book figure alone is below the ceiling; the ceiling is tested on the true figure, so the book figure passing does not cure the breach.

**This constitution contains no live portfolio data.** No dollar figures, no tickers, no position sizes, no balances. Live figures live in the SSOT; updating one is an SSOT record, not a P-rule edit, and the leak gate cannot trip on this text.

No other account, holding, or cash balance is outside the book. A new investment account, or a new holding outside the SSOT, must be listed in the SSOT (with its US-situs total) or brought into the book within 30 days of its creation. A book whose scope is unstated is non-compliant.

**Within A,** "conviction" means a documented, reviewable view expressed through a basket — not confidence in one company. A sector or thematic ETF is a legitimate A holding provided it carries a written view statement, a horizon, and an invalidation condition.

**C is defined by risk role, not by duration.** Cash, bills and bonds all belong in C, because none of them is equity beta and all of them settle in a day. A cash-like instrument may not sit inside A. A duration-bearing instrument may not sit inside B.

### P2′ · Targets come from the glidepath; risky weight is set by age

Sleeve targets are read from the age band in the glidepath (Part IV).

**Total risky weight is set by age — not by opinion, not by a market view, not by valuation.** Risky weight means `(A + B) / NLV`.

An investor who wants to change the equity level changes the glidepath band by constitutional amendment (major version bump + ADR), with the reason recorded in the ADR. Nothing else may move it.

Sleeve B carries a **ceiling, not a target**. It is excluded from the glidepath's target arithmetic: the glidepath sets A + B, and B's ceiling constrains how much of that total may sit in satellites. B's ceiling is **10% of NLV at every age band, for life** — there is no wind-down. Protection with age comes from the risky weight falling, not from liquidating B.

**C is the residual**, `C = NLV − A − B`. There is no separate defensive target. C's target in the Top-up formula is the derived complement `(1 − risky_target) × NLV'`, used only to compute the gap (Part II); C has no independently-set target and receives whatever the Top-up does not allocate to A.

### P3′ · Money moves two ways only

| Operation | Job | Frequency | Sells? |
|---|---|---|---|
| **Top-up** | Allocate the monthly contribution and any pooled capital injection (Part II) | Monthly | **No** |
| **Rebalance** | Correct a band or cap breach | Quarterly, or on a hard-cap breach | **Yes** |

There is no third operation. No discretionary channel, no reserve, **no override**. No file in V7 grants an override, and no rationale — however well written — creates one.

**The AI agent is advisory only. It never places, modifies, or cancels an order; execution is the owner's act alone.** (See Part VII.)

**Soft limits** — sleeve bands and the Sleeve B ceiling — are reviewed quarterly and rebalanced only when the deviation exceeds the tolerance in the SSOT (`rebalance_tolerance_pct`, 2.5pp). A deviation inside the tolerance is reported, not traded.

**Hard caps** — look-through single-company, single-sector, US-situs, and a single defensive holding above its duration cap — fire immediately as a Rebalance trigger (P7′).

**Escalation is time-bounded.** A deviation beyond 3× tolerance is hard — for an *undershoot* of risky weight, which has a permitted cure (C → A) and so triggers a real Rebalance. An *overshoot* beyond 3× tolerance is reported-only: it has no permitted cure (A → C is prohibited), so it is reported and closed by Top-up, never by a forced sale. A deviation that survives two consecutive quarterly reviews is hard if it is an undershoot; an overshoot surviving two reviews is reported-only. The two directions are not equal, and the asymmetry is deliberate: no rule in V7 pretends a cure exists where no permitted direction does.

**A mandate that expires without curing its breach does not lapse silently.** On the sunset date every uncured breach becomes hard, and the Rebalance that follows must execute a named action. Where no permitted direction exists, the Investment Committee must decide — in writing, within 10 business days — between amending the constitution and accepting the breach on the record. There is no third outcome, and "wait" is not one. **An accepted breach is a deferral, not a pardon** (see P7′ for its expiry).

A time-boxed **transition mandate** may exist (P3a) to migrate the portfolio from one structure to another. It has a sunset date, completion criteria, a fallback, cap compliance throughout, a **window**, a window-year cap, and a minimum interval (see P3a). It is the only exception in V7, and it is windowed.

### P3a · Transition mandate (subordinate to P3′)

A time-boxed structural migration is permitted, with **eight** requirements:

1. **A sunset date, no more than 92 days after raising.** Hard, with no extension. After it, P1′–P8′ apply unchanged. (The 92-day maximum matches the v5→v6 mandate's actual span — adopted 2026-11-15, sunset 2027-02-15 — and the owner may choose another figure.)
2. **Completion criteria.** Per-sleeve target ranges, in dollars.
3. **A fallback.** At sunset, any unmet gap is closed by Rebalance.
4. **Cap compliance throughout.** The mandate cannot raise total risky weight above the P2′ ceiling, and cannot breach any P6′ cap.
5. **A window, a window-year, and the actual-calendar-year cap.** Mandate windows are bounded by **31 January, 31 July, and 31 October**. The windows are 1 February–31 July, 1 August–31 October, and 1 November–31 January. **Between any two boundaries only one mandate may be raised** — at most one mandate per window. A second mandate raised in the same window is **refused when proposed**. Each window belongs to a **window-year**: the calendar year of its opening date — the 1 November–31 January window belongs to the window-year of its 1 November opening; the 1 February–31 July and 1 August–31 October windows belong to the window-year of their opening. A mandate raised in January belongs to the previous year's window: a January 2027 mandate sits in window-year 2026's window. **Window-year is used only to assign a mandate to its window; it is not the annual cap.** The annual cap is counted by **actual calendar year of raising**: **no more than three mandates may be raised in any actual calendar year**, counted by raising date in the SSOT mandate record. A fourth raising in the same actual calendar year is **refused when proposed**, even if its window is free. **Counting starts at V7 adoption, as a one-time adoption exemption, not a general principle:** mandates raised before V7 adoption do not count toward either count — including the v5→v6 mandate, whose raising date is recorded in the SSOT as adopted 2026-11-15 and which was executed 2026-11-16. **The first window under V7 is the window containing the adoption date** — in the expected case (adoption before 31 January 2027) that is 1 November 2026–31 January 2027, belonging to window-year 2026 — so the rule does not break if adoption falls after 31 January 2027.
6. **No overlap.** No new mandate may be raised while the previous mandate is still **processing**. "Processing" means the mandate's status is `proposed` or `open` and its legs are not yet complete. A mandate whose legs are complete is `executed`; a mandate past its sunset with legs incomplete is `expired` — its window is consumed, and the reason for the failure is recorded in the SSOT; an expired mandate does not free its window. A refused or abandoned proposal is recorded as `withdrawn` — it still consumes its window, so a refusal cannot be erased by deleting the record. The sunset-check verification after completion does not count as processing. The status vocabulary is exactly `proposed`, `open`, `executed`, `expired`, `withdrawn` — no free text, no other values.
7. **A minimum interval.** At least **90 days** must pass between raisings, so a mandate raised on 31 January and another on 1 February — in different windows — is still refused. The 90-day interval is shorter than the shortest window and prevents boundary-adjacent back-to-back mandates. *(Advisor-added rule, not an owner decision: the owner may strike it. The window rule above is the owner's rule; the interval is friction the advisor added to make the window rule hold at the boundary.)*

8. **What a mandate may not do.** A mandate may not (a) exit a Sleeve B name except on a P5′ thesis break, (b) sell A to buy C, or (c) bring any money from outside the book into it. A mandate is a migration of money already in the book, between sleeves and lines the constitution permits — it is not a general override, and it cannot do what P4′ and P5′ forbid.

**Raising** a mandate means recording it in the SSOT (`transition_mandate`) with its sunset date, completion criteria, fallback, and window, and the owner's approval. A mandate that is not recorded in the SSOT does not exist.

**What a mandate is for (and is not for).** A mandate is a time-boxed structural migration: it changes the portfolio's **structure**, not the money in it or the risk level. It is for: (1) the v5→v6 migration (November 2026, the real precedent: the sleeve count went from four to three, the Z sleeve merged into C, plus the direct-semiconductor-name → semiconductor-basket same-exposure swap; sunset 2027-02-15); (2) rotating the thematic BLOCK — changing the set of themes held (for example selling two block members and buying a different theme) — which requires a mandate, not a Rebalance (exiting a single thematic ETF on a P5′ trigger is a Rebalance, A→A); (3) a wholesale US→IE migration — moving all US-domiciled lines to IE equivalents at once (a single same-exposure swap is a Rebalance; a wholesale migration is a mandate); (4) a future framework-version migration (for example v7→v8) that changes the sleeve structure. A mandate is NOT for: bringing money in (an employer bonus, Mox sale proceeds — those are capital injections, external Top-ups, governed by the injection rules); taking money out (that is the withdrawal rule, Part II); selling A to buy C (barred by requirement 8(b)); or exiting a B name (thesis break only, requirement 8(a)). **The December Mox consolidation is a capital injection under this framework, not a mandate**: the Mox holding is sold, the proceeds enter the book as an external Top-up, and they are deployed by the next monthly Top-up.

**Worked examples (windows, window-year, and the actual-calendar-year cap).** (a) A mandate raised in December 2026 sits in the 1 November 2026–31 January 2027 window, window-year 2026; it is the first mandate under V7. The v5→v6 mandate (adopted 2026-11-15, in the same window) does not count, by the one-time adoption exemption. (b) A mandate raised in January 2027 sits in the same window and is refused — the window already has one. (c) The actual-calendar-year cap is independent of the windows: mandates raised on 2 January, 2 April, 1 August and 1 November 2027 are each in a different window, and the 2 January raising sits in window-year 2026's window — but all four fall in actual calendar year 2027, so the 1 November 2027 raising is the fourth in that calendar year and is refused by the actual-calendar-year counter, even though its window is free. (d) A mandate raised on 31 January 2027 and another proposed on 1 February 2027 are in different windows, but the 90-day interval refuses the second. (e) A mandate raised on 15 December 2026 blocks any raising until 15 March 2027 under the 90-day interval — including the 1 February 2027 window's opening — which is the advisor-added friction of rule 7, not the owner's window rule. (f) The v5→v6 mandate ran adopted 2026-11-15 to sunset 2027-02-15: exactly 92 days, the maximum rule 1 now allows.

**Substitution clause.** Replacing direct single-name exposure with a fund holding the same exposure — a capital-neutral swap — is **not** an exit under P5′. It is a Rebalance action under P4′ (B → A), subject to the look-through caps, and it does not require a transition mandate. The mandate path remains available for it, but is not required. Wherever it is executed, the swap must be stated in dollars, hold risky weight constant, and pass the look-through caps on the post-swap book — and it must satisfy P5′'s definition of "same exposure" (post-swap look-through exposure to the exited company's GICS industry group at least the pre-swap direct exposure, in dollars, recorded in the SSOT).

### P4′ · Rebalance may not de-risk the core

Rebalance may sell:

| Direction | Permitted? |
|---|---|
| Defensive → risky (C → A) | ✅ The crash response |
| Risky → risky (B → A) | ✅ Fixes concentration |
| Rotation **inside** C | ✅ Not a transfer — the duration band is cured here |
| Sale **inside** C, pro-rata | ✅ Preserves the band; shortest-first would spike the average |
| Rotation **inside** A | ✅ Cures a look-through cap inside A, or exits an A holding on a P5′ trigger (view invalidated, fund mandate/domicile/liquidity change, expense ratio); proceeds to the most underweight A line |
| **A → C** | ❌ **Prohibited** — that is de-risking |
| **B → C** | ❌ Exit proceeds go to A, never to C directly (Part II) |

Selling core equity to buy bonds or cash is de-risking, and de-risking is what the glidepath decides — not a rebalance. **There is no policy that lifts this restriction.** V6's "hard glidepath" variant is deleted: the glidepath is contribution-tilt, always, and A → C is always prohibited.

**A → B is permitted only as an Investment Committee decision** under P6′, never as an automatic Rebalance output, because B has no target and therefore no gap to fund.

### P5′ · Exits follow the instrument, not the price — and there is no score

**Sleeve A (ETF) holdings.** An A holding is not sold because a constituent company's thesis breaks, because its price falls, or because a valuation conclusion is drawn about one name inside it. A single-company event inside a fund triggers a **look-through review** of concentration, not a sale of the fund.

An A holding is reviewed — and resized or exited — when:

1. the written view statement that justified it is invalidated;
2. a look-through cap in P6′ is breached and cannot be cured by trimming another A holding;
3. the fund's mandate, benchmark, index methodology, domicile, or liquidity changes materially; or
4. the fund's expense ratio rises materially against peers.

**Sleeve B (satellite) holdings.** A B holding is exited — taken to zero — **only when its thesis breaks**. Price decline, drawdown, valuation and macro conditions are never exit triggers.

**There is no score in V7.** Every numeric score, the scoring layers and the replacement ranking are deleted. A thesis break is established by **documented research and verification**: the research states what changed, cites the evidence (filings, earnings, verified reporting), and is recorded in the SSOT note for the position. Verification is by a session other than the one that wrote the research (Part V). The exit is decided at the **quarterly review**, opened as a dated item in the execution queue (`execution/pending.yaml`) with its deadline. A thesis break with no recorded research is not a thesis break; a review with no deadline date is not a review. The owner's own words govern here: a thesis break needs research and verification first, and that research is itself the scoring.

**A weight trim is not an exit.** Trimming is risk control and is permitted only at a Quarterly Review when Sleeve B exceeds its ceiling beyond tolerance (Part II); voluntary mid-quarter trims are prohibited.

**A trim has a floor.** A Sleeve B trim may not reduce any position below **1.0% of NLV**; any reduction below 1.0% of NLV is an exit and requires a documented thesis break (Part V). A trim that would take a position below the floor is refused; the position is either trimmed to the floor or exited on a thesis break — there is no third outcome. Without this floor, a "trim" can shrink a position to a stub and exit it in all but name, bypassing the thesis-break rule.

**Replacing direct single-name exposure with a fund holding the same exposure is not an exit.** The factor weight is unchanged; only the wrapper changes. This is the operation this system exists to permit, and it is a **Rebalance action under P4′ (B → A), subject to the look-through caps** — it does not require a transition mandate (see the P3a substitution clause). **"Same exposure" is defined:** the post-swap look-through exposure to the exited company's GICS industry group must be at least the pre-swap direct exposure to that company, in dollars, recorded in the SSOT with both figures. Below that, the swap is an exit and needs a thesis break. Precedent check: the canonical November 2026 swap of a direct semiconductor name into a semiconductor basket qualifies under this definition — the fund is 100% in the exited company's industry group, so the post-swap industry-group exposure is at least the pre-swap direct exposure. A broad fund holding only a small weight in the company does not qualify: its industry-group exposure on the swapped dollars is a fraction of the pre-swap direct exposure.

**A P5′ exit adds the ticker to the permanently-exited list automatically**, recorded in the SSOT with the exit date and reason. The list is append-only; removal requires a constitutional amendment (major version bump + ADR), not a dated record.

### P6′ · Caps outrank every rule; concentration is measured through the wrapper

| Constraint | Default |
|---|---|
| **Look-through single-company exposure** (direct + fund look-through + index-embedded) | 5% of NLV |
| **Single-sector exposure** (including sector weight inside broad-index funds) | 25% of NLV |
| Sleeve B ceiling | 10% of NLV, at every age |
| Sleeve B name count (`names_max`) | 5 names |
| Sleeve B single name (`position_cap_pct`) | 5% of NLV |
| Sleeve C — **average** effective duration | 1–6 years |
| Sleeve C — **any single holding** | ≤ 10 years |
| Sleeve C — **holdings with duration above 6 years** | ≤ 50% of C's value |
| Thematic block share of Sleeve A | 25% of A, hard — `sleeves.a.thematic_max_pct` |
| US-situs assets (non-US investor) | below the jurisdiction's estate-tax exemption ($60,000) |

When a cap and any other rule disagree, **the cap wins**. A target that violates a cap is invalid and must be re-solved.

**Look-through is mandatory.** A fund position is not a single exposure for cap purposes: it is the sum of its underlying holdings. Two funds that both hold the same company are one exposure. Broad-index funds are included — index-embedded weight counts.

**"Sector" is measured on a declared basis, not an assumed one.** The 25% cap is computed on **GICS industry group** — the level at which a thematic book actually concentrates — and the basis is recorded in the SSOT (`constraints.sector_basis`). A cap whose basis is not recorded cannot be computed, and a book that cannot be measured is treated as non-compliant. Broad GICS *sector* exposure (eleven sectors) is reported as a **dashboard metric, not a cap**. Changing `constraints.sector_basis` is a constitutional change — a version bump and an ADR — because a cap whose basis can be redefined can be satisfied without trading.

**The stop-adding line binds every operation, not one trade.** When US-situs exposure is at or above 80% of the ceiling ($48,000), the A share of a contribution is directed only to A's non-US-domiciled lines; where A has none to buy, that share goes to C. This binds the Top-up (including any pooled capital injection), the Rebalance, an IC decision, and a transition mandate. Position appreciation moves it too, so a breach is possible with no trade at all: at or above the ceiling it is a hard breach with a named cure — rotate a US-domiciled line into the non-US instrument holding the same exposure, inside the same sleeve. A capital-neutral swap between two US-situs instruments changes nothing and is not restricted. **Brokerage cash is counted as US-situs** until the custodian confirms otherwise; if the custodian confirms an exemption, the SSOT flag is set false only with the custodian's answer recorded — the document attached, or its reference and date recorded in the SSOT history (see Part VI, Check 10).

**Duration is the one control inside C.** Average effective duration of C must sit between **1 and 6 years**, and no single holding may exceed **10 years**. The average is a style guard, tested at the quarterly review as its own unit — not against a percentage-point tolerance, because years are not percentage points. The single-holding cap is a **hard breach** (Check 25, P7′ 10-day cure, by rotation inside C). Holdings with duration above 6 years may not exceed **50% of C's value** — the average alone would pass a barbell that is really a duration bet; exceeding 50% is a **hard breach** (Check 27, P7′ 10-day cure, by rotation inside C). The average band is a **soft limit** (Check 26): a value outside 1–6y is reported every cycle and cured at the quarterly review by rotation inside C. Measure from fact-sheet effective duration, weighted by market value, and report **money duration in dollars** beside the average: an average alone hides how much is actually at stake.

The lower bound exists so the sleeve is not a parked cash balance earning nothing; the upper bound exists so a labelling decision cannot become a duration bet.

**Caps are point tests with a measurement buffer.** Look-through and sector weights are summed by hand from published fund holdings, which lag and truncate. Assets are therefore maintained at least **0.5pp inside** a cap; a measured value between cap − 0.5pp and the cap is reported as `AT CAP — no further addition`, not as passing with room.

**`names_max` is a forward cap, and it is a forward refusal.** No *new* name may take the count above the limit; existing names are not force-sold to satisfy a count. Selling a dust position to reach five is compliance theatre, and the risk a name count guards against — sprawl — is stopped by refusing the next addition. The count is reported every cycle. The count comes down by decision at the quarterly review — which name, on its thesis — never by selling the smallest position to make arithmetic work.

**Sleeve B ceiling is soft.** It enforces at the quarterly Rebalance review within the tolerance band: B between 10% and 12.5% (ceiling + the 2.5pp tolerance) is reported, not trimmed; B above 12.5% at the review is trimmed back to the ceiling. Between reviews B may exceed the ceiling, and this is reported every cycle rather than silently tolerated.

**Sleeve B receives no Top-up.** Ever. Satellites are funded only by an Investment Committee decision from Sleeve A under P6′, and never by the monthly contribution.

### P7′ · A cap breach is itself a Rebalance trigger — with a deadline

No separate mechanism is needed.

- **Hard cap breach** — look-through single-company, single-sector, US-situs, a single C holding above 10 years, C holdings above 6 years exceeding 50% of C, a risky-weight undershoot beyond 3× tolerance, or a risky-weight undershoot surviving two consecutive quarterly reviews — triggers Rebalance immediately. A risky-weight overshoot beyond 3× tolerance, or surviving two consecutive quarterly reviews, is reported-only: it has no permitted cure (A → C is prohibited) and triggers no Rebalance.
- **Soft limit breach** — sleeve band beyond tolerance, Sleeve B ceiling beyond tolerance, a B count already above `names_max`, or C average effective duration outside 1–6 years — triggers Rebalance at the next quarterly review, and is reported as a flag in the interim.

**A hard breach must be cured or escalated within 10 business days of identification.** Cured means the trade is **filled** — a placed-but-unfilled order does not cure a breach. Escalated means the Investment Committee decides in writing, within those 10 business days, between amending the constitution and accepting the breach on the record. **An uncured, unescalated hard breach blocks A→B funding, mandate raising, and capital injections until it is cured** — no new money moves and no new mandate is raised while a hard breach stands past its deadline without a decision (tested by Check 22). An amendment takes effect 30 days after its ADR is recorded (amendment table), so it cannot cure a breach on the day it is recorded. **Identification is the validation run that flags the breach** (Part VI). Reports are generated at least monthly, with the Top-up, and immediately on any known hard-cap event — so a breach is identified at most a month after it occurs, and at once when it is known. The clock runs from **identification**, not from the owner's decision that the breach is incurable. A breach the owner simply does not cure still starts the clock. "Triggers Rebalance immediately" is immediate flagging; the 10-day deadline is the cure.

**An accepted breach expires.** An acceptance must name a **cure date or a re-decision date no later than the next quarterly review**. If that date passes without a cure or a fresh written decision, the acceptance **lapses and the breach is hard**. Acceptance is a deferral to a named date, not a permanent pardon. The same breach may not be accepted twice without a cure in between, and **after two acceptances without a cure, the only remaining options are cure or constitutional amendment (major version bump + ADR) — a third acceptance is not available**. An expired acceptance that is not cured is hard, and the breach is **flagged in every report and every Top-up until it is cured or the constitution is amended**. Every acceptance is recorded in the SSOT with its expiry date.

If a breach cannot be cured within the P4′ limits, it is escalated to the Investment Committee as a structural problem, not silently tolerated: a written decision within 10 business days, between amending the constitution and accepting the breach on the record. **Reporting is not an outcome.**

### P8′ · Reports state dollars, confidence, and fixed order

Every report:

- leads with the conclusion
- states dollar amounts, not percentages alone
- separates facts from assessments
- carries a confidence percentage on each recommendation
- follows the **14-section** Health Check order when producing a full review (Part V)

**A derived document is authoritative only for the framework version it was generated under.** A document generated under an older version is stale and must not be acted on. At a version adoption, superseded derived documents are archived or deleted — never left standing with "stands" or "fires" language. The SSOT is the only source of portfolio data; a derived document is never edited by hand.

---

## Amendment table — the single table

There is **one** amendment table, and it is this one. No other file contains an amendment table, a version-bump table, or a change-control rule. `framework/AGENTS.md` and every other agent instruction file refer to this table and do not restate it.

| Change | Requirement |
|---|---|
| Any P-rule (P1′–P8′, P3a) | **Major version bump + ADR + CHANGELOG entry** |
| Any other binding rule in framework text (`constitution/`, `core/`, `commands/`, `AGENTS.md`, scripts that enforce a rule) | **Major version bump + ADR + CHANGELOG entry** |
| Method text that changes no rule, no date, no threshold, and no deadline (explanatory prose, report templates, research workflow steps, glossary) | CHANGELOG entry only |
| The quarterly review calendar, marks convention, or agenda order (Part V, "Pinned") | **Major version bump + ADR + CHANGELOG entry** (it defines what P3′ and P7′ mean by "quarterly") |
| A parameter in the SSOT that implements a P-rule (cap values, tolerance, targets, glidepath bands, sector basis) | Only as part of a rule change above — a cap value cannot be changed as a "parameter" |
| Portfolio-local facts in the SSOT (positions, shares, cost bases, market values, notes, external-account listings) | No bump; a dated record in the SSOT history |
| The DCA amount | An increase requires a dated written record in the SSOT (Part II); a decrease is not permitted at all — "never decreased" is the rule |
| Derived documents | Regenerated from the SSOT; never edited by hand |
| Typo fixes, version stamps, non-rule wording | CHANGELOG entry only |
| Effect of a P-rule amendment | Takes effect **30 days after its ADR is recorded** (cooling-off). The V7 adoption itself is not an amendment and is not delayed by this rule. |

**What counts as a "binding rule":** a sentence that constrains what the owner or the agent may do, a cap, a target, a trigger, a deadline, a window, or a procedure that moves money. Everything else is non-rule text. If a change is arguably a rule, it is a rule.

There is no soft-amendment pathway.

---
