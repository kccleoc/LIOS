# Deployment — Top-up and Rebalance (Part II)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. This file is part of the v7.0.0 framework; where it disagrees with Part I (the constitution), Part I governs.

Capital moves by exactly two operations, plus a time-boxed transition mandate when the structure itself is changing. There is no third path.

---

## Part II — Deployment (Top-up and Rebalance)

Capital moves by exactly **two operations**, plus a time-boxed transition mandate when the structure itself is changing.

| Operation | Job | Frequency | Sells? |
|---|---|---|---|
| **Top-up** | Allocate the monthly contribution and any pooled capital injection | Monthly | **No** |
| **Rebalance** | Correct a band or cap breach | Quarterly, or on a hard-cap breach | **Yes** |
| Transition mandate | Migrate to a new structure | Per window, sunsets, windowed | Yes |

There is no third path. No discretionary channel, no reserve, no override.

**Precedence.** When two rules could both act: (1) hard caps (P6′) — absolute; (2) Rebalance (P3′) — overrides Top-up in any month both would act, but only to the extent it actually executes a trade; (3) Top-up (P3′) — the default monthly action; (4) transition mandate (P3a) — bounded by the caps; cannot raise risky above P2′. A Rebalance trigger that executes no trade does **not** block Top-up. Without that distinction, a stuck sleeve freezes contributions forever.

**No borrowing, ever.** The account is margin-type so that unsettled sale proceeds can fund a same-session buy — not so the book can be levered. Gross exposure may not exceed NLV (`leverage ≤ 1.00×`). A buy leg is placed only after its funding sells have filled, and only up to settled cash plus those fills; a rolled or partial buy is capped at `min(remaining quantity, floor(available cash / limit price))`. Validation Check 12 aborts production on negative cash. **Buying power is not capital** — it is the broker's estimate of what could be borrowed, and nothing here borrows.

**Execution.** Limit orders only. **Venue-aware windows:** orders for positions with `listing_venue: LSE` are placed **10:00–11:30 ET**, before the LSE close (16:30 London = 11:30 ET); orders for positions with `listing_venue: US` are placed **10:00–15:30 ET**. The venue is an SSOT position attribute, not a ticker list in this text: a new line is traded in the window its attribute states. During the 1–2 weeks a year when US and UK clock changes misalign, the LSE close is 12:30 ET — the 11:30 ET bound is the conservative one and still applies. Sells at or above last trade − 0.25%; buys at or below last trade + 0.25%. A buy is sized on actual net proceeds. An unfilled leg rolls at the same quantity.

### Top-up

**Terminology (owner's words).** DCA is the monthly contribution ($1,500) — the money, not the operation. Top-up is the operation that deploys money into a sleeve. It has two sources: an **internal Top-up** is money already inside the book, deployed by a Rebalance (the buy leg of a C→A move or an A→A rotation); an **external Top-up** is a capital injection — money from outside the book (a sale of an external holding, a bonus, a distribution) — deployed with the next monthly Top-up, pooled with the month's DCA. **An external Top-up does not enter via a Mandate path:** a mandate may not bring money from outside the book into it (P3a requirement 8c). Worked example: an employer bonus is a capital injection (an external Top-up), deployed with the next Top-up; it does not need a mandate. It is governed by the injection rules (source test, IC record above $5,000, 30-day A→B bar) and the caps — not by the mandate rules (windows, 90-day interval, max 3/year).

Runs monthly. Never sells. No discretion.

```
eligible_new_money = the month's DCA ($1,500)
                   + any arrears due that month ($750, if any)
                   + any eligible capital injection pooled with that month's Top-up

NLV' = NLV + eligible_new_money

T_A  = risky_target × NLV' − B_actual_value    ← A's dollar target: the risky target minus B, held constant
T_C  = (1 − risky_target) × NLV'
G_A  = T_A − A_value                           positive = underweight
G_C  = T_C − C_value
G+   = max(G, 0)

risky_now = (A + B) / NLV                      ← C is defensive, NOT risky

risky_cap = risky_target + rebalance_tolerance_pct   (80% + 2.5pp = 82.5%)

if risky_now >= risky_cap:
        allocate the whole eligible_new_money to C
else:
        allocate eligible_new_money × G+ / sum(G+)

If `T_A` is negative, B exceeds the whole risky budget: `G_A` is negative, A receives nothing from this Top-up, and the negative target is reported as an IC-level structural failure (Check 28) — it is not clamped to zero.
```

**Three things that must not be got wrong.**

1. **`risky_now = (A + B) / NLV`.** C is the defensive sleeve and is never counted as risky. Counting it trips the risky-cap branch permanently and routes every future contribution into the defensive block — a silent conversion of the portfolio, with no error and no flag.
2. **B is held constant, not excluded and not funded.** `T_A` deducts `B_actual_value`, so the Top-up neither funds B nor assumes B away: B's value is carried into A's target dollar-for-dollar. That is the mechanical expression of "B is never topped up."
3. **The risky-cap fallback targets C.** It is the only defensive sleeve, so the branch that used to starve one while feeding the other does not exist.

**Within A, an allocated dollar buys A's lines pro-rata to their within-A targets recorded in the SSOT** (the standing split: 50% / 30% / 20% across the two broad-index core lines and the thematic block — the per-line weights live in the SSOT, not in this text). Within the block, a dollar is allocated pro-rata to the block's existing composition (positions with `thematic_block: true`), matching C's rule — the block has one target, not three, and no line inside it is chosen by discretion. **A line whose purchase would breach a cap is skipped and its share re-allocated to the other A lines pro-rata.** Before that happens, test `constraints.us_situs_exposure` against its stop-adding line. At or above 80% of the ceiling ($48,000), A's share is directed only to A's **non-US-domiciled** lines; where A has none, that share goes to C. The gate binds **every** operation — Top-up (including any pooled capital injection), Rebalance, an IC decision, a mandate — and appreciation moves the figure too, so a breach is possible with no trade at all.

**Within C, the contribution buys pro-rata to C's existing composition** (cash, if C is empty). C has no target and no internal split, but an unallocated dollar is still a monthly decision for the owner. Pro-rata holds the duration average where it was and keeps the decision out of the loop.

**The fallback splits by gap, not by target weight.** Splitting by target weights looks tidy and feeds whichever sleeve is already overweight.

**A's gap cannot always be funded by the Top-up.** If `risky_now` is at the cap the whole contribution goes to C, and A stays underweight until either the cap falls or a Rebalance moves C → A. That is deliberate — but it means an underweight A while risk sits at the cap is a **Rebalance** condition, not a Top-up condition. Report it as one, or it presents as neglect.

**The DCA.** The monthly contribution is **$1,500**. It is reviewed each January under the step-up rule (Part IV). **It is never decreased. An increase requires a dated written record in the SSOT** stating the old amount, the new amount, the date, and the reason; a decrease is not permitted at all, not even by record. A one-line YAML edit with no record is not a change; it is an error, and the validation run must flag the SSOT as unrecorded. The DCA's discipline rests on the record, not on willpower.

**A skipped month is deferred, not cancelled.** The contribution is a fixed rate, and the plan has no discretion to reduce it. A month that is not executed becomes arrears, repaid on top of the normal contribution over the following months — **at $750 per month over two months** ($1,500 normal + $750 arrears, twice) — never as a single lump that changes the allocation arithmetic. Without this, one missed month drifts the whole book permanently, because the gap the Top-up was closing simply stops closing. Every skipped month opens a dated arrears item in the execution queue with its two repayment months named.

**Capital injection (windfall).** Money that arrives outside the monthly DCA — a sale of an external holding, a bonus, a distribution — is a **capital injection**. A capital injection is a **source of eligible external capital, not an operation of its own**: it is deployed **by the Top-up**, with the next monthly Top-up (one Top-up per month), **pooled with that month's contribution** (the $1,500 DCA plus any arrears due) in a single gap-formula allocation: allocated to A and C by positive gap, **capped at the A gap** (it may not push A above its target), with any remainder staying in C. It does not displace the month's $1,500 DCA. It does not go to B — B is never topped up. It is recorded in the SSOT with its source, date, and amount. An injection above $5,000 requires an IC record. **Source test:** an injection is money from **outside the book** — a sale of an external holding, a bonus, a distribution. The test is about the money's origin, not control: a sale is a deliberate act, and it still qualifies. **Eligible external capital expressly excludes Mox operating cash and any transfer from an operating or ready-cash account** (P1′: Mox cash stays in Mox and is not deployed into the book) — a transfer from such an account is not an injection and cannot enter the book under this rule. **Proceeds from selling Mox securities are eligible** — a sale of an external holding — which is the lawful path for the day-one US-situs cure (Part I, Adoption position). A transfer made for the purpose of buying the dip is a market-timing transfer, not an injection, and is prohibited by the same rule that prohibits deploying cash because the market looks cheap. There is no other way for new money to enter the book.

**How an injection is split inside A.** (1) The injection is deployed through the Top-up gap formula, and the allocation to A is capped at A's dollar gap — an injection cannot push A above its target. (2) Inside A, the money is split **pro-rata to the SSOT's A allocation weights** (the standing 50/30/20 split across the two broad-index core lines and the thematic block), **not in equal thirds** — equal thirds would overweight the thematic block. (3) A line whose purchase would breach a cap is skipped and its share re-allocated to the other A lines pro-rata — in particular, while the true US-situs figure is above the $48,000 stop line, no US-domiciled line may be bought and its share goes to the IE-domiciled lines.

**Withdrawal.** Money leaving the book is a **withdrawal** — an owner act governed by this rule, not an operation. It is not a Rebalance and not a mandate. It is recorded in the SSOT with its date, amount, reason, and destination. It is taken from **C first**, then from **A pro-rata to within-A targets**, and **never from B except on a thesis break**. A withdrawal that breaches a cap triggers P7′. A withdrawal above $5,000 requires an IC record; below that it is recorded only. A withdrawal is the owner's act — the IC record, where required, records the decision; it does not authorise the money to leave. The owner may always withdraw his own money, and this rule governs *how* it is taken, not *whether*.

### Rebalance

Runs on a hard-cap breach immediately, or at the quarterly review otherwise. This is the only routine operation that sells. The review's dates and standing agenda are fixed in Part V; nothing here depends on when the owner happens to remember to look.

**What the Rebalance covers.** Thesis-break exits, A→B funding transfers, and substitution swaps are executed as Rebalance actions — they are the Rebalance's selling/buying legs, executed at the quarterly review or on a hard-cap trigger. They are not a third operation: a thesis-break exit sells B and buys A (B → A); an A→B funding transfer moves money inside the risky block on an IC decision; a substitution swap replaces a B name with a fund (B → A). Exit proceeds go to A (never to C directly). Sale proceeds are never assigned to Sleeve B: cash from a B sale is deployed into A, or held as cash (Sleeve C by definition) until deployed — it is never used to buy a B name without an IC decision.

#### Trigger

**Hard — immediate:**
- look-through single-company exposure above cap
- single-sector exposure above cap
- US-situs above the ceiling
- a single C holding above its duration cap
- C holdings above 6 years exceeding 50% of C
- a B name above `position_cap_pct` — the per-name 5% test. It is hard on every report, not merely at a review: at the default settings a B name above 5% of NLV is also a look-through breach, so a soft reading would have the same fact firing immediately under one rule and waiting a quarter under another.

**Soft — quarterly review, beyond tolerance:**
- a sleeve outside its band
- C average effective duration outside 1–6 years
- Sleeve B above its ceiling beyond tolerance (above 12.5% of NLV)
- Sleeve B above `sleeves.b.names_max` — the **count**, not the addition. The next addition is refused when it is proposed (P6′); a count already above the limit is settled at the review, and this line never authorises a forced sale to make a count come out
- an A holding carrying a 0% target and still holding value (the dust rule) — except a member of a defined block (Part III)
- an age-band change, after the 12-month grace period

#### Direction limits (P4′)

| Sell → buy | Permitted |
|---|---|
| C → A | ✅ the crash response |
| B → A | ✅ fixes concentration |
| A → B | ✅ **IC decision only**, never automatic |
| inside C (any rotation) | ✅ not a transfer; cures the duration band |
| A → A (within-sleeve) | ✅ cures a look-through cap inside A, or exits an A holding on a P5′ trigger; proceeds to the most underweight A line |
| A → C | ❌ **prohibited** — that is de-risking |
| B → C | ❌ exit proceeds go to A, never to C directly |

#### Action

```
1  Identify hard-cap breaches, then soft breaches beyond tolerance
2  Sell the smallest set that cures the breach, subject to P4′
3  Buy underweight sleeves pro-rata to their remaining gaps
4  B is bought only under an explicit IC decision, within names_max and
   position_cap_pct — a decision that breaches either is not an IC decision,
   it is a cap breach. An A→B transfer may not be approved within 30 calendar
   days of a capital injection — the bar covers capital injections only, not the
   monthly DCA (a DCA lands every month, so the bar would prohibit A→B transfers
   permanently) — and its IC record states the source of the transferred dollars
   (Part III). The control for DCA-to-B is the IC decision plus the B caps.
5  C is never sold below the money needed to keep the risky block in band, and a sale
   inside C is pro-rata across its holdings — never shortest-first
6  Disposition any A holding carrying a 0% target outside a defined block (dust rule);
   proceeds to the most underweight A line
```

#### Trim selection

| Sleeve | Trim |
|---|---|
| **A** | the largest within-A holding, back to its within-A weight |
| **B** | the **largest** position — trims reduce concentration; a trim is permitted only at a Quarterly Review when B exceeds its ceiling beyond tolerance (P5′); a trim may not reduce any position below 1.0% of NLV — a reduction below that floor is an exit, not a trim |
| **C** | no intra-sleeve selection for sizing; sell pro-rata, then re-cure duration |

**A trim is not an exit.** Exits are governed by P5′.

**C is not ranked.** Bonds are not ranked against each other for weakness. That judgment lives in B, applied to companies.

### Transition mandate (P3a) — operating rules

The constitutional requirements are in P3a (Part I). The operating rules are:

- A mandate is recorded in the SSOT before any leg executes, with: id, window, window-year, raising date, sunset date (no more than 92 days after raising), completion criteria in dollars per sleeve, fallback, and status (`proposed` / `open` / `executed` / `expired` / `withdrawn`). A `withdrawn` mandate still consumes its window; an `expired` mandate's window is consumed and its failure reason is recorded.
- Legs execute in order. Each leg is sized on actual net proceeds of its funding sale. A leg that does not fill rolls at the same quantity.
- At sunset, any unmet gap is closed by Rebalance under P1′–P7′, not by extending the mandate.
- The mandate's status is updated on execution: `open` while legs are incomplete, `executed` when all legs are complete, `expired` if the sunset passes with legs incomplete. The sunset-check verification after completion is a dated note, not a status.
- No mandate may be raised in a window that already has one, and none while another is `proposed` or `open`. The validation run checks both (Part VI, Check 20).

### Prohibited

| Operation | Verdict |
|---|---|
| "Deploy cash because the market looks cheap" | ❌ no such operation |
| "Skip this month's contribution" | ❌ Top-up is mechanical |
| "Increase or decrease the contribution without a dated record" | ❌ P3′ — the DCA changes only by record |
| "Sell equities to raise cash for safety" | ❌ P4′ |
| "Top up the satellites, they're doing well" | ❌ P6′ — B is never topped up |
| "Sell a satellite on a price drop" | ❌ P5′ |
| "Move A → C because C is underweight" | ❌ P4′ |
| "Rebalance now because the gap is annoying" | ❌ soft limits wait for the quarterly review and the tolerance |
| "Deploy a windfall outside the capital-injection rule" | ❌ use the injection rule |
| "One more exception, just this quarter" | ❌ the transition mandate is the only exception, and it sunsets, and it is windowed |
| "Override a framework output with a rationale" | ❌ no override exists in V7 |
| "Withdraw from B because I need the cash" | ❌ P5′ — B is exited only on a thesis break; take it from C, then A |

---
