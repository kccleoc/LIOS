# LIOS Glossary

> **Historical note.** Entries above the "v4.1 — Eight Rules and Two
> Operations" section were written for earlier versions and may reference
> L-numbers, Kelly, the Strategic Reserve, or Sleeve D. Those concepts are
> retired. Where an entry conflicts with the v4.1 or v5 sections, the later
> section wins.

Terms used across LIOS docs. Sorted alphabetically.
Each entry: term, definition, where it's used.

---

## B

### Buying-pause trigger
**Definition:** A cap hit on a Sleeve B name. Signals stop buying.
If momentum continues, escalate to trim trigger.
**Used in:** `02_capital_architecture.md` Sleeve B, `commands/health-check.md` §6

---

## C

### Cap (Sleeve B)
**Definition:** Maximum % of NLV a Sleeve B name may occupy. Caps are binding — both buying-pause AND trim triggers. The *form* (caps are binding) is constitutional per Locked Decision L3. The *parameter values* (specific tickers and their cap percentages) live in `state.yaml: sleeves.b.caps`.
**Used in:** Locked Decision L3, `02_capital_architecture.md` Sleeve B

### Candidate
**Definition:** In a Replacement Index evaluation, the proposed replacement for an incumbent Sleeve B holding.
**Used in:** `04_replacement_index.md`, `commands/sleeve-d-review.md`

### Confidence %
**Definition:** Required on every recommendation in LIOS outputs. Format: `Confidence: XX% — [one-line rationale]`.
**Used in:** Communication standards, `01_identity_and_communication.md`

### Candidate

## L (v1.5 — Portfolio Construction Intelligence)

### LFS — LIOS Fit Score
**Definition:** 0-100 score measuring portfolio fit — how much an investment improves the current LIOS portfolio. Portfolio-relative: same company may score differently as portfolio evolves. Five components: Diversification Value, Strategic Fit, Overlap, Conviction Compatibility, Portfolio Balance.
**Used in:** `09_portfolio_construction_intelligence.md`, all LFS commands, Health Check sections 5/6/8/10/11/15

### LFS Band
**Definition:** The five fixed interpretation tiers for LFS: Exceptional 90-100 / Strong Fit 80-89 / Useful 70-79 / Neutral 60-69 / Poor Fit <60.
**Used in:** Locked Decision L15-L18, `09_portfolio_construction_intelligence.md`

### LFS Preference Rule
**Definition:** Locked Decision L18. When LOS values are similar (within 5 points), LIOS should prefer the investment with the higher LFS. Official decision rule for capital allocation priority.
**Used in:** Locked Decision L18, `09_portfolio_construction_intelligence.md`, `commands/portfolio-rotation.md`

### LFS Decision Matrix
**Definition:** Joint evaluation of LOS + LFS: High/High = Highest priority, High/Low = Lower priority (excellent company, carry), Low/High = Do not buy (L17), Low/Low = Reject.
**Used in:** `09_portfolio_construction_intelligence.md`, `commands/los-lfs.md`

### LFS Components
**Definition:** The five scored dimensions of LFS:
1. Diversification Value — does the company introduce a new economic engine?
2. Strategic Fit — does this strengthen the long-term LIOS mission?
3. Overlap — how much similar exposure already exists?
4. Conviction Compatibility — can LIOS understand and monitor this business?
5. Portfolio Balance — does this improve overall portfolio construction?
**Used in:** `09_portfolio_construction_intelligence.md`, `commands/lfs.md`

### Economic Engine
**Definition:** Underlying economic driver of a business, distinct from sector label. Examples: AI Compute, AI Memory, AI Platforms, Industrial Automation, Consumer Spending, Digital Payments, Healthcare Innovation, Defense, Energy Infrastructure, Enterprise Software, Logistics, Utilities. LFS evaluates diversification by economic engine, not by sector.
**Used in:** `09_portfolio_construction_intelligence.md`, `commands/economic-engine-map.md`

---

## L (v1.4 — Research Intelligence Layer)

### LVS — LIOS Value Score
**Definition:** 0-100 score measuring valuation attractiveness.
Inputs: Forward P/E, PEG, EV/EBITDA, FCF yield, historical valuation range, margin of safety.
Higher = better value.
**Used in:** `08_research_intelligence_layer.md`, Locked Decision L11

### LGS — LIOS Growth Score
**Definition:** 0-100 score measuring long-term compounding potential.
Inputs: Revenue/EPS/CF growth, estimate revisions, industry growth, TAM.
Higher = stronger future growth.
**Used in:** `08_research_intelligence_layer.md`, Locked Decision L11

### LQS — LIOS Quality Score
**Definition:** 0-100 score measuring business excellence.
Inputs: ROIC, ROE, margins, FCF, balance sheet, capital allocation, moat.
Higher = higher quality business.
**Used in:** `08_research_intelligence_layer.md`, Locked Decision L11

### LMS — LIOS Momentum Score
**Definition:** 0-100 score measuring market confirmation.
Inputs: Relative strength, trend, earnings surprises, estimate revisions, institutional accumulation, technical strength.
Higher = stronger momentum.
**Used in:** `08_research_intelligence_layer.md`, Locked Decision L11

### LOS — LIOS Overall Score
**Definition:** 0-100 composite of LVS/LGS/LQS/LMS using professional judgment.
Default weights: LQS 35 / LGS 25 / LVS 25 / LMS 15. Quality carries greatest influence.
**Used in:** `08_research_intelligence_layer.md`, all LOS commands, Health Check sections 5/6/8/11

### LOS Band
**Definition:** The five fixed interpretation tiers for LOS:
Elite 90-100 / Buy 80-89 / Watch 70-79 / Hold 60-69 / Pass <60.
**Used in:** Locked Decision L12, `08_research_intelligence_layer.md`

### LOS Default Weights
**Definition:** LQS 35% / LGS 25% / LVS 25% / LMS 15%. May evolve via documented v1.4.x patch. Per-score deviations allowed if disclosed.
**Used in:** Locked Decision L14, `08_research_intelligence_layer.md`

### LOS Gate (Sleeve B)
**Definition:** Pre-Replacement-Index check. Candidates with LOS < 70 are auto-Pass; 70-74 watch; ≥75 eligible.
**Used in:** `04_replacement_index.md` v1.4 section, `commands/los-sleeve-d.md`, `commands/los-replace.md`

---

## D

### DCA
**Definition:** Dollar-cost averaging. The monthly systematic deployment amount. **The Kelly Factor no longer governs it (retired v3.0)** — DCA is governed by the gap in the Top-up waterfall. The amount is a personal parameter in `state.yaml: monthly_dca`.
**Used in:** `core/deployment.md` Top-up

### Diversification Value
**Definition:** One of three Replacement Index metrics. 25% weight. Measures how much a candidate adds to the existing D portfolio (low correlation, theme coverage).
**Used in:** `04_replacement_index.md`

### Drift
**Definition:** % (or $) by which a sleeve has moved from its target.
Thresholds: <2% no action, 2-5% flag, >5% recommend rebalance.
**Used in:** `commands/drift-check.md`, `06_health_check_structure.md` §2

---

## F

### Facts vs Assessments
**Definition:** Required separation in every LIOS output.
Facts = observed/verifiable. Assessments = judgment calls with Confidence %.
**Used in:** Communication standards, all Health Check sections

### Frontier Score
**Definition:** One of three Replacement Index metrics. 40% weight. Measures positioning within frontier tech theme.
**Used in:** `04_replacement_index.md`, `commands/sleeve-d-review.md`

### Frontier Scoreboard
**Definition:** Health Check §11. All D names + candidates scored on the three metrics.
**Used in:** `06_health_check_structure.md` §11

### Funding Source
**Definition:** Where the capital for a sleeve deployment comes from. Strict whitelists per sleeve (e.g., D funded by external capital only).
**Used in:** Sleeve B satellites

---

## G

### Governance Principle
**Definition:** "Rules are permanent. Decisions are dynamic." Constitutional layer of LIOS.
**Used in:** Locked Decision L8, `05_governance_principle.md`

---

## H

### Health Check
**Definition:** The full portfolio report. Canonical output format. Always the same sections, always this order — **14 sections since v5** (was 16 in v4.1). See the v5 section below.
**Used in:** `core/health-check.md`, `commands/health-check.md`

---

## I

### IC (Investment Committee)
**Definition:** Lc + active AI session. Has sole authority on SR deployment, may approve/reject any trade recommendation, may NOT modify Locked Decisions without major version bump.
**Used in:** `05_governance_principle.md`, `02_capital_architecture.md` SR

### Incumbent
**Definition:** In a Replacement Index evaluation, the current Sleeve B holding being evaluated.
**Used in:** `04_replacement_index.md`, `commands/sleeve-d-review.md`

---

## K

### Kelly Factor
**Definition:** **Retired (v3.0).** Historical entry. Was: 0.00–1.00 fraction determining what portion of the monthly DCA gets deployed. From Kelly Model v2.0.
**Used in:** `core/regime-index.md`, `commands/kelly-recalc.md`

### Kelly Model v2.0
**Definition:** **Retired (v3.0).** Historical entry. Was: five-dimension weighted scoring producing Kelly Factor.
Dimensions: Market Valuation 30% / Portfolio Valuation 25% / Macro & Liquidity 20% / Technical 15% / Opportunity Set 10%.
**Used in:** Locked Decision L4

### Kelly Reserve
**Definition:** **Retired (v3.0).** Historical entry. Was: accumulated undeployed portion of monthly DCA. Lives in Sleeve C. Deploys on Kelly Factor increase or trigger.
**Used in:** `02_capital_architecture.md` Sleeve C, `03_kelly_model_v2.md`

---

## L

### Locked Decision
**Definition:** A rule that may NOT be modified without major version bump (v3.0+) and constitutional amendment. See `07_locked_decisions.md` for full list (L1–L18, L7 gapped — was removed in v3.0 as portfolio-local, not a framework invariant). v3.0 introduces the distinction between **Locked Decision form** (the rule template, public/framework) and **Locked Decision parameters** (the bound values, private to each portfolio's `state.yaml`).
**Used in:** Throughout LIOS, especially `05_governance_principle.md`

---

## M

### Macro & Liquidity
**Definition:** Kelly Model v2.0 dimension #3, 20% weight. Rates, credit spreads, Fed posture, USD, liquidity conditions.
**Used in:** `core/regime-index.md`

### Market Valuation
**Definition:** Kelly Model v2.0 dimension #1, 30% weight (highest). Broad equity valuations — P/E, CAPE, equity risk premium, breadth.
**Used in:** `core/regime-index.md`

### Market Valuation

## N

### NLV (Net Liquidation Value)
**Definition:** Total portfolio value. Gross Position Value + Cash. Always expressed in dollars, never just %.
**Used in:** Throughout LIOS

### NLV (Net Liquidation Value)

## O

### Opportunity Set
**Definition:** Kelly Model v2.0 dimension #5, 10% weight. Quality of buying opportunities available *now*.
**Used in:** `core/regime-index.md`

---

## P

### Pilot (Stage 2)
**Definition:** Small starter position (typically ≤ 0.5% NLV) used to confirm a thesis before sizing up. Applies to Sleeve B satellites.
**Used in:** Sleeve B satellites, `04_replacement_index.md`

### Portfolio Valuation
**Definition:** Kelly Model v2.0 dimension #2, 25% weight. Valuation of Lc's specific holdings vs own history, growth durability.
**Used in:** `core/regime-index.md`

---

## R

### Replacement Index
**Definition:** Three-metric weighted score governing satellite surgery in **Sleeve B**. In v4.1 it governed the old satellite sleeve; in v5 it governs B.
Frontier 40% / Momentum 35% / Diversification 25%. Replacement requires composite superiority plus at least two of four qualitative conditions, and is size-neutral (one out, one in, same dollars).
**Used in:** `constitution/method.md`, `core/sleeves.md`

### RIL (Research Intelligence Layer)
**Definition:** The v1.4 module that introduces LVS/LGS/LQS/LMS/LOS scoring. Universal research framework. See `08_research_intelligence_layer.md`.
**Used in:** All v1.4 docs, all LOS commands

---

## S

### Sleeve
**Definition:** A capital layer in LIOS. Since v5, three sleeves: **A, B, C**. Every weight, cap and floor is a percentage of NLV. See the v5 section below for the current definitions.
**Used in:** `core/sleeves.md`

### Snapshot
**Definition:** Date-stamped portfolio state. The canonical input for all LIOS commands.
**Used in:** `snapshots/`, `commands/snapshot.md`

### SR (Strategic Reserve)
**Definition:** **Retired (v3.0).** Historical entry. Was: undeployed capital awaiting superior expected returns. NOT a sleeve. Funded only by external capital, exceptional realization, windfalls.
**Used in:** Locked Decision L6, `02_capital_architecture.md`

### Staging
**Definition:** Stage 1 Watchlist → Stage 2 Pilot → Stage 3 Full position. New D names move through stages sequentially.
**Used in:** Sleeve B satellites, `04_replacement_index.md`

---

## T

### Technical Conditions
**Definition:** Kelly Model v2.0 dimension #4, 15% weight. Trend, momentum, breadth, volatility regime, drawdown depth.
**Used in:** `core/regime-index.md`

### Thesis Momentum
**Definition:** One of three Replacement Index metrics. 35% weight. Is the investment thesis strengthening or weakening?
**Used in:** `04_replacement_index.md`

### Trigger
**Definition:** **Retired (v3.0).** Historical entry. Was: SR deployment condition (e.g., S&P correction >8-10%, Nasdaq >10%, earnings confirm stronger thesis, Kelly Factor increase).
**Used in:** `02_capital_architecture.md` SR, `commands/trigger-check.md`

### Trim trigger
**Definition:** A cap hit on a Sleeve B name combined with continued momentum. Signals reduce position to cap.
**Used in:** `02_capital_architecture.md` Sleeve B

---

## V

## W

### Watchlist (Stage 1)
**Definition:** Watchlist stage 1. Thesis and initial score logged. No capital deployed. A watchlist entry is not a holding and does not consume Sleeve B ceiling headroom.
**Used in:** Sleeve B satellites, `04_replacement_index.md`

### Weighted Score
**Definition:** **Retired.** The Kelly dimensions and Kelly Factor were repealed in v3.0 and do not exist in v5. The five-dimension weighted score survives only as the **Regime Index**, a dashboard with no mechanical effect.
**Used in:** `core/regime-index.md`

---

## Symbols

### Confidence: XX%
**Definition:** Required format. Number 0-100 + one-line rationale.

### Facts vs Assessments
**Definition:** Required separation. **Facts** = observed/verifiable. **Assessments** = judgment.

### L1–L10
**Definition:** Locked Decisions carried from v1.3. L7 removed in v3.0 (was portfolio-local, not framework invariant); L-number gapped per the no-renumber rule. See `07_locked_decisions.md`.

### L11–L14
**Definition:** Locked Decisions introduced in v1.4 (Research Intelligence Layer).
L11 — RIL canonical. L12 — LOS bands fixed. L13 — LOS additive (not substitutive). L14 — Default weighting orientation.
See `07_locked_decisions.md`, `08_research_intelligence_layer.md`.

### L15–L18
**Definition:** Locked Decisions introduced in v1.5 (Portfolio Construction Intelligence).
L15 — LFS canonical (portfolio fit required). L16 — LOS absolute, LFS relative.
L17 — LFS cannot rescue poor investment. L18 — LFS preference rule.
See `07_locked_decisions.md`, `09_portfolio_construction_intelligence.md`.

---

## Related

- See `lios/components/` for concept docs that reference these terms
- See `lios/v4.1.md` for the canonical definitions in init-prompt form

---

## v3.0 — Open-Source Bifurcation (new terms)

### Framework repo
**Definition:** The public `kccleoc/LIOS` repository. Contains constitution, core, lios/v2.x.md, commands, templates, synthetic examples, and docs. Never contains personal portfolio data. The `examples/` directory provides a fully runnable synthetic demo.
**Used in:** All OSS-facing docs; grilling into v3.0

### Personal data repo
**Definition:** A cloner's private repository (Leo's is `kccleoc/LIOS-mine`). Contains `portfolio/state/portfolio.yaml`, snapshots, reports, research, and a **vendored pinned** copy of the public framework under `framework/` with `framework/VERSION` recording the pinned tag.
**Used in:** Private LIOS operations

### LIOS-archive
**Definition:** The frozen private repository preserving Leo's full original LIOS history from v1.3 through v2.1.2 — including v1.x historical prompts, portfolio snapshots from every era, all research, and the contaminated git commit graph. Renamed from `kccleoc/LIOS` to `kccleoc/LIOS-archive` during the v3.0 bifurcation. The old URL redirects silently (GitHub rename). Never modified after the v3.0 split.
**Used in:** Historical reference only

### Vendored pin
**Definition:** The consumption model where the public framework is *copied* (not submoduled, not subtreed) into the personal data repo at a named git tag and recorded in `framework/VERSION`. Updates via `bin/pull-framework.sh <tag>`. Overrides to vendored files are permitted but never silent — each carries a `<!-- PRIVATE-FORK -->` marker and a `+leo.N` version suffix.
**Used in:** ADR-003 §B; all cross-repo version management

### Private fork override
**Definition:** An edit to a vendored framework file in the personal data repo, marked with `<!-- PRIVATE-FORK: overrides <base-tag> per ADR-003 → ... -->`. Reflected in `framework/VERSION` as `<base-tag>+leo.N`. When `bin/pull-framework.sh` detects an upstream change to a flagged file, it *preserves* the local override (does not overwrite) and exits nonzero — forcing manual reconciliation.
**Used in:** ADR-003 §A; `bin/pull-framework.sh` (13b behavior)

### Locked Decision form
**Definition:** The rule template that is constitutional and framework-invariant. Examples: "Sleeve B per-position NLV caps are binding (buying pause + trim)" (L3 form), "Sleeve A has permanent target allocation" (L2 form). Amendment requires a major version bump. Exists in the public framework repo for all cloners.
**Used in:** ADR-003 §C; `constitution/constitution.md` L2/L3 reframes

### Locked Decision parameters
**Definition:** The bound values referenced by a Locked Decision form — the portfolio-local content that varies per user. Examples: the specific ticker+cap pairs that populate L3's form (e.g., `<TICKER>` at 20% NLV, etc.), the specific ETF tickers and weights that populate L2's form. Live in `state.yaml` under the v3.0 bifurcation.
**Used in:** ADR-003 §C; `state.yaml: sleeve_a.allocation`, `state.yaml: sleeves.b.caps`, `state.yaml: sleeve_a.permanently_exited`
---

# v3.0 — Additions and Supersessions

The entries above were written for v2.x. Where they conflict with the
definitions below, **these win**.

### Regime Index (RI)
**Definition:** 0–100 conditions gauge, higher = more bearish. Five weighted dimensions (Market Valuation 30 / Portfolio Valuation 25 / Macro & Liquidity 20 / Technical 15 / Opportunity 10). Its only mechanical effect is to shift the A/B/C split inside a fixed risky budget. Supersedes Kelly Factor.
**Used in:** `core/regime-index.md`, `scores/Regime.md`, `commands/regime-recalc.md`

### Band (RI)
**Definition:** One of four regimes: Risk-On 0–35 · Constructive 36–50 · Cautious 51–65 · Defensive 66–100. Total risky exposure is identical in all four.
**Used in:** `core/regime-index.md`

### Gap (G)
**Definition:** `G_s = w_s × (NLV + DCA) − V_s`. Positive = underweight. The only input to the DCA waterfall other than target weights. Drift and gap are the same number in v3.0.
**Used in:** `core/deployment.md`

### Standstill term
**Definition:** `w_s × DCA`. The portion of a contribution required simply to hold a sleeve's weight, because the target grows with NLV. Without it, a gap-driven rule converges to a permanent positive gap and never closes.
**Used in:** `core/deployment.md` Top-up

### Deployment Rules 1–3
**Definition:** The only three operations that move capital. (1) DCA waterfall — monthly, never sells. (2) Rebalance band — on breach, sells. (3) Discretionary channel — ≤1 per quarter, max 3% NLV, funded from D or Z. P3. **Superseded in v4.1: two operations only — the discretionary channel is deleted.**
**Used in:** `core/deployment.md`

### Rebalance band
**Definition:** Per-sleeve band, `clamp(0.5 × target, 2pp, 8pp)`. A breach triggers Rebalance. Replaces the v2.x trigger ladder.
**Used in:** `core/deployment.md` Rebalance

### Contribution tilt
**Definition:** Default glidepath policy. Age-band targets govern **new money only**; no equities are sold to de-risk; equity weight may exceed target by 10–20pp by retirement. The alternative is a hard glidepath.
**Used in:** `core/glidepath.md`

### Effective exposure
**Definition:** A holding's direct weight plus its embedded weight in the index sleeves. A 7% direct position in a name that is 6.5% of a 65% core sleeve is about 11% effective. Single-name caps apply to this number, not to the direct weight.
**Used in:** `core/sleeves.md`, P6

### US-situs exposure
**Definition:** Assets subject to US estate tax for a non-US investor — US-domiciled ETFs and shares of US corporations. Irish-domiciled UCITS do not count. Tracked against a hard ceiling. P6.
**Used in:** `constitution/constitution.md` P6, `state.yaml: constraints.us_situs_*`

### Sleeve Z (Cash Equivalents)
**Definition:** Duration ≈ 0 sleeve, always last. T-bills, money-market funds, 0–1y Treasury ETFs, bank cash. Never reported as a bond sleeve or a hedge. Defined by risk factor, not issuer.
**Used in:** `core/sleeves.md`

### Sleeve D (Duration)
**Definition:** **Deleted in v5.0.0.** The duration role is absorbed by **Sleeve C**; the satellite role moved to **Sleeve B** (see v5 section below). Historical definition: bonds with duration > 3 years, hedged to the liability currency. The only sleeve whose purpose is to appreciate when equities fall.
**Used in:** `constitution/constitution.md` ("What replaced what")

### Sleeve C (Satellite)
**Definition:** **Superseded in v5** — the satellite role moved to **Sleeve B**, and Sleeve C now holds duration bonds (see v5 section below). Historical definition: discretionary rotation sleeve, winner-weighted by design, no equal-weight rule. Laggards are cut, not averaged down. Hard ceiling; above it, trim the weakest name. Sleeve C surgery was governed by the Replacement Index.
**Used in:** `core/sleeves.md`

## Superseded terms

| Term | Status | Replacement |
|---|---|---|
| Kelly Factor | **Deleted** | Regime Index band |
| Kelly Model v2.0 | **Repealed (L4)** | `core/regime-index.md` |
| Kelly Reserve | **Deleted** | Sleeve C |
| Strategic Reserve / SR | **Repealed (L6)** | Sleeve D drift + rebalance band |
| Trigger ladder | **Deleted** | Rebalance band |
| Free tactical (TC) | **Deleted** | Sleeve C + discretionary channel |
| `ABC` denominator | **Deleted** | NLV |
| DCA "governed by Kelly Factor" | **Superseded** | DCA governed by the gap (Top-up) |

---

# v4.1 — Eight Rules and Two Operations

Supersedes the L-number layer entirely.

### P1–P8 (Portfolio Rules)
**Definition:** The eight constitutional rules as written in v4.1. P1 five sleeves one denominator · P2 targets from the glidepath, risky weight by age · P3 two operations only · P4 Rebalance direction limits · P5 Sleeve B exits only on thesis break, via a 30-day review · P6 caps outrank every rule · P7 a cap breach is a Rebalance trigger · P8 reporting. Superseded in v5 by **P1′–P8′** (three sleeves); see the v5 section below. Changing any of them requires a major version bump plus an ADR.
**Used in:** `constitution/constitution.md`

### Method
**Definition:** Everything in `constitution/method.md` — LOS/LFS weights, band thresholds, Replacement Index, report templates, communication style, glossary. Editable with a CHANGELOG entry, no version bump. The test: if it cannot be broken in a way that damages the portfolio, it is method.
**Used in:** `constitution/method.md`

### Top-up
**Definition:** The monthly operation. Allocates the contribution pro-rata to each sleeve's gap, `G_s = w_s × (NLV + DCA) − V_s` — computed over **A and C only** (B has no target and is never topped up). Never sells. Falls back to routing 100% to **C** when total risky weight is at the age-band cap. v5: risky weight is `(A + B) / NLV`, and A's target is the residual `risky_target − B_actual`.
**Used in:** `core/deployment.md` (P3′)

### Rebalance
**Definition:** The on-breach operation. Triggered **immediately** by a hard-cap breach; soft breaches (sleeve band or B ceiling beyond tolerance) wait for the quarterly review. Sells overweight and buys underweight, subject to P4′'s direction limits. This is the crash response.
**Used in:** `core/deployment.md` (P3′, P7′)

### Per-sleeve band
**Definition:** `clamp(0.5 × target, 2pp, 8pp)`. A 70% sleeve gets ±8pp; a 4% sleeve gets ±2pp. A single absolute band could not serve both.
**Used in:** `core/deployment.md`

### Transition mandate
**Definition:** A time-boxed structural migration. Requires a sunset date, completion criteria, a fallback that closes unmet gaps by Rebalance, and continuous cap compliance. Cannot raise risky weight above the P2 ceiling.
**Used in:** `core/deployment.md`

### Contribution tilt
**Definition:** The default glidepath policy. Targets govern new money only; equity is not sold to de-risk. Contrast with a hard glidepath, where targets are binding and Rebalance sells equity to reach them. A "glidepath" that cannot reduce equity weight is a contribution tilt, and should be named one.
**Used in:** `core/glidepath.md`

### Regime Index (RI)
**Definition:** 0–100 conditions gauge, higher = more bearish. Dashboard only. It sizes nothing, shifts no target, gates no operation. Its mechanical effect was removed in v4.1.
**Used in:** `core/regime-index.md`

### 30-day review (P5)
**Definition:** What LOS < 60 triggers on a Sleeve B holding. Must resolve to either a documented re-score ≥ 60 citing new evidence, or an exit. No third outcome, no automatic extension.
**Used in:** `constitution/constitution.md` P5′

---

# v5.0.0 — Four Sleeves (A / B / C)

Supersedes the v4.1 five-sleeve definitions above. Where an entry above
conflicts with these, these win.

### Sleeve A (Conviction ETFs)
**Definition:** ETFs — broad-index **and** sector/thematic. Every holding carries a **view statement**: intended exposure, why a basket rather than a name, horizon, invalidation condition. A constituent's thesis break triggers a look-through review, not a sale of the fund.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P1′

### Sleeve B (Satellites, cap-only)
**Definition:** Individual companies and narrow bets. No target weight, no equal-weight rule, **no Top-up — ever**. Ceiling 10% of NLV, enforced at the quarterly review within the tolerance. Funded only by an Investment Committee decision from Sleeve A. Exits only on thesis break (P5′). The Replacement Index governs surgery here.
**Used in:** `core/sleeves.md`, `constitution/method.md`

### Sleeve C (Defensive)
**Definition:** Cash, money market, T-bills, 0–1y funds and bond ETFs of any duration. **Residual target** — `C = NLV − A − B`; no stored target, no floor, no ceiling. Ballast and ammunition: the designated source for Rebalance into an underweight risky sleeve. **One control: average effective duration 1–6 years, no single holding above 10.** Composition (cash vs bonds) is reported, never targeted — the v5 split into a bond sleeve and a cash sleeve was deleted in v6 because it generated defects without changing behaviour. Bought, not rotated: no ranking, no Replacement Index.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P1′, P6′

### Sleeve D — deleted
**Definition:** Removed in v5.0.0. The duration role moved to **Sleeve C**; the satellite rotation role moved to **Sleeve B**. No live instruction may reference a Sleeve D.
**Used in:** `constitution/constitution.md` ("What replaced what")

### Sleeve Z — deleted in v6
**Definition:** Removed in v6.0.0. Its contents (cash, bills, money market) merged into **Sleeve C**, which is now the whole defensive block. The floor and ceiling went with it: a 5% sleeve with a 2.5pp tolerance is ±50% of itself, which is a suggestion rather than a limit. No live instruction may reference a Sleeve Z.

### Duration band
**Definition:** The single control inside Sleeve C. Average effective duration of 1–6 years, and no single holding above 10. Measured as a value-weighted average of fact-sheet effective duration; the average is a quarterly style guard, the single-holding cap is a hard breach. Report **money duration in dollars** with it — a 1pp rate move costs money duration × 1%. A barbell of bills and one long bond can satisfy the average while behaving in two directions, which is why the single-holding cap exists.
**Used in:** `constitution/constitution.md` P6′, `core/sleeves.md`, `core/validation.md` Check 11, Health Check §7

### Look-through exposure
**Definition:** Exposure to a single company measured through every wrapper: direct holdings plus each fund's underlying weight in that company plus index-embedded weight. Two funds holding the same company are one exposure. Capped at 5% of NLV; single-sector exposure (25% of NLV) is measured the same way, including sector weight inside broad-index funds.
**Used in:** `constitution/constitution.md` P6′, `core/sleeves.md`, Health Check §11

### Sector basis
**Definition:** The taxonomy a sector cap is computed on, recorded in `state.yaml: constraints.sector_basis` because "sector" is otherwise undefined and the same book is compliant or in hard breach depending on the reading. Default `gics_industry_group`. Broad GICS sector exposure is a dashboard metric, not a cap. A cap with no recorded basis is treated as non-compliant, not as passing.
**Used in:** `constitution/constitution.md` P6′, Health Check §11

### Cap-only
**Definition:** A sleeve governed by a ceiling rather than a target weight. Applies to Sleeve B: no target, no glidepath share, no Top-up. The ceiling — 10% of NLV — binds at the quarterly review within the tolerance; between reviews a winner may run, with the overage reported every cycle.
**Used in:** `core/deployment.md`, `core/sleeves.md`

### Rebalance tolerance
**Definition:** `state.yaml: rebalance_tolerance_pct` (default 2.5pp). Soft-limit deviations inside the tolerance are reported, not traded; beyond it, the sleeve rebalances at the next quarterly review. Hard-cap breaches ignore the tolerance and trigger Rebalance immediately (P7′).
**Used in:** `core/deployment.md`, `constitution/constitution.md` P3′, P7′

### View statement
**Definition:** The written justification required for every Sleeve A holding: intended exposure, why a basket rather than a single name, the horizon, and the condition that invalidates it. A thematic ETF is entered and reviewed as a view, not as a position in a company.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P1′

### 14-section Health Check
**Definition:** The v5 report order: Portfolio Health · Gap & Drift · Regime Index (dashboard) · Constraint Check · Sleeve A/B/C Reviews · Top-up Plan · Rebalance Status · Look-Through Concentration · Satellite Scoreboard · Execution Calendar · Execution Plan · Risk Assessment & CIO Resolution. Replaces the 16-section v4.1 order.
**Used in:** `constitution/method.md`, `core/health-check.md`
