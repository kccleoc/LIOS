> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


> **Source:** Extracted verbatim from LIOS v1.5. L11-L14 are Locked Decisions.

# Research Intelligence Layer (RIL) — LOS Framework

**Component version:** v1.4 (still active in v1.5)
**Status:** Locked Decision (L11–L14)
**Last updated:** 2026-07-25

---

## Purpose

The Research Intelligence Layer introduces a **standardized scoring
language** so every security — current holding, watchlist candidate,
Sleeve D prospect, or research idea — can be evaluated using the
same five-score framework before any capital allocation decision.

RIL does **not** replace Kelly, the Sleeves, SR logic, or the
Replacement Index. It is the **upstream research filter**:

- **LOS decides WHICH assets deserve investment.**
- **Kelly decides HOW MUCH capital to deploy.**
- **Sleeves decide WHERE the capital lives.**

Without RIL, every "is this a good investment?" question becomes
a vibes-based conversation. With RIL, the same question has a
structured answer that can be compared across holdings, candidates,
and time.

---

## The Five Scores

### LVS — LIOS Value Score
**Measures:** Valuation attractiveness.
**Typical inputs:**
- Forward P/E (vs own history, vs peers, vs market)
- PEG ratio
- EV/EBITDA
- Free Cash Flow Yield
- Historical valuation range (where current sits vs 5-yr band)
- Margin of Safety vs intrinsic estimate

**Score:** 0–100. Higher = better value.

---

### LGS — LIOS Growth Score
**Measures:** Long-term compounding potential.
**Typical inputs:**
- Revenue growth (trailing + forward consensus)
- EPS growth (trailing + forward consensus)
- Cash flow growth
- Analyst estimate revisions (positive/negative drift)
- Industry growth rate
- TAM expansion trajectory

**Score:** 0–100. Higher = stronger future growth.

---

### LQS — LIOS Quality Score
**Measures:** Business excellence.
**Typical inputs:**
- ROIC (Return on Invested Capital)
- ROE
- Gross Margin
- Operating Margin
- Free Cash Flow generation
- Balance Sheet strength (debt/equity, interest coverage)
- Capital allocation track record (buybacks, M&A, dividend)
- Competitive Moat (brand, network effects, switching costs, scale)

**Score:** 0–100. Higher = higher quality business.

---

### LMS — LIOS Momentum Score
**Measures:** Market confirmation.
**Typical inputs:**
- Relative Strength (vs market, vs sector)
- Trend (price above key moving averages)
- Earnings surprises (recent beats/misses)
- Estimate revisions (direction + magnitude)
- Institutional accumulation (13F flow, ownership changes)
- Technical strength (RSI regime, breakout patterns)

**Score:** 0–100. Higher = stronger momentum.

---

### LOS — LIOS Overall Score
**Measures:** Composite investment attractiveness.
**Derivation:** Weighted blend of LVS / LGS / LQS / LMS with
**professional judgment**. Quality should generally carry the
greatest influence, followed by Growth, Value, then Momentum.

**Default starting weights:**

| Component | Default Weight | Rationale |
|---|---|---|
| LQS | 35% | Quality compounds; weak businesses destroy value regardless of price |
| LGS | 25% | Long-term returns depend on durable growth |
| LVS | 25% | Valuation discipline prevents permanent loss |
| LMS | 15% | Useful as confirmation, weak as standalone predictor |

**Score:** 0–100. Higher = more attractive overall.

**Discipline rule:** Any deviation from default weights must be
disclosed in the score output with one-line rationale. Examples:
- "Quality-only deep-value play: LQS 50 / LVS 40 / LGS 10 / LMS 0"
- "Momentum-rotation tilt: LQS 30 / LGS 20 / LVS 15 / LMS 35"

**Why judgment, not a rigid formula:** A 100-bps mechanical
weighting would be deterministic but brittle. The framing —
"Quality generally carries the greatest influence" — preserves
discipline while allowing context (e.g., a momentum regime, a
deep-value opportunity, a turnaround thesis).

---

## LOS Interpretation Bands

| LOS | Label | Action |
|---|---|---|
| **90–100** | **Elite** | Highest conviction. Core compounder. Aggressively accumulate during normal pullbacks. Rarely sell on valuation alone. |
| **80–89** | **Buy** | Accumulate. Suitable for regular DCA. Eligible for Sleeve A or Sleeve B. |
| **70–79** | **Watch** | Build watchlist. Buy on meaningful correction. Starter position acceptable. |
| **60–69** | **Hold** | Existing holders monitor. New purchases require a strong catalyst. |
| **Below 60** | **Pass** | Avoid. Candidate for replacement if currently owned. |

Bands are **fixed** (Locked Decision L12). Modifying band
boundaries requires a major version bump.

---

## Standard Research Output Format

Every company research begins with this exact block:

```
Company: <TICKER>
LOS:    <NN>
LVS:    <NN>  — <one-line rationale>
LGS:    <NN>  — <one-line rationale>
LQS:    <NN>  — <one-line rationale>
LMS:    <NN>  — <one-line rationale>
Recommendation: <BUY / HOLD / REDUCE / EXIT / PASS / WATCH>
Summary: <one sentence>
Confidence: <XX>%
```

### Example (Texas Instruments)

```
Company: TXN
LOS:    81
LVS:    82  — Forward P/E 18x vs 5-yr avg 22x; FCF yield ~5%
LGS:    72  — Modest revenue growth (~5%); expanding industrial/auto TAM
LQS:    95  — Best-in-class ROIC (~30%); fortress balance sheet; wide moat in analog
LMS:    64  — Range-bound trend; mixed estimate revisions
Recommendation: BUY
Summary: Excellent business quality with attractive valuation,
         moderate growth outlook, and improving momentum.
Confidence: 78%
```

---

## Integration with Existing LIOS

### Kelly
- Kelly v2.0 (L4) unchanged. Kelly still decides **how much** capital to deploy.
- LOS decides **which** assets are even candidates for Kelly-allocated dollars.
- A LOS < 70 candidate does not enter Kelly sizing decisions.

### Sleeves
- Sleeve targets and caps (L2, L3) unchanged.
- LOS ≥ 80 required for new Sleeve B additions (per IC discipline; not a hard rule but expected).
- Sleeve A (broad-index ETFs) does not receive LOS scoring (not applicable to index vehicles).

### Strategic Reserve
- SR funding/deployment rules (L6) unchanged.
- SR Opportunity Ranking (Health Check §10) should now also display LOS beside each candidate.

### Health Check
- L10 (16-section fixed order) unchanged.
- **Each holding in sections 5, 6, 8 now shows LOS beside it.**
- Sleeve D Review (§8) shows both Replacement Index AND LOS per holding.

### Replacement Index (Sleeve D)
- L5 unchanged. Replacement Index still gates all Sleeve D surgery.
- LOS is **complementary**: a candidate must clear LOS threshold (typically ≥ 75) **AND** pass Replacement Index composite superiority + ≥2 of 4 qualitative conditions.
- See `04_replacement_index.md` for the joint workflow.

---

## Candidate Research Workflow

```
Discover Candidate
       ↓
  Generate LOS  (using commands/los.md)
       ↓
   LOS < 70 ──→ STOP (do not pursue)
       ↓
 LOS 70-79 ──→ WATCHLIST (commands/los-watchlist.md)
       ↓
  LOS 80+  ──→ DEEP RESEARCH (manual due diligence)
       ↓
   Kelly Allocation (if Sleeve A/B eligible, after Kelly recalc)
       ↓
   Execution
```

LOS < 70 → does not enter the watchlist. No exceptions without explicit IC override.

---

## When To Re-Score

LOS values are **dynamic**, not permanent labels. Re-score after:

- Quarterly earnings (incumbent OR candidate)
- Material guidance changes
- Price moves > 20% in either direction
- M&A events
- Major competitive / regulatory shifts
- Any time Lc asks "is this still a good holding?"

A drop from LOS 85 → LOS 72 should trigger a Watchlist re-evaluation,
not an automatic exit.

---

## Anti-Patterns (will be rejected)

| Trigger | Why it's not a valid LOS use |
|---|---|
| "It has a high LOS so buy it regardless of valuation" | LOS embeds LVS — if LMS-driven, the high LOS is fragile |
| "It dropped to LOS 65 so sell it" | Hold band (60-69) is monitor-only. Need a stronger catalyst to exit. |
| "LOS < 60 means automatically replace in Sleeve D" | LOS < 60 flags as Pass; replacement still requires Replacement Index |
| "Calculate LOS using only LMS because momentum is hot" | Disclosure rule — must explain deviation if LMS dominates |
| "Skip LOS for ETFs in Sleeve A" | Correct — Sleeve A vehicles are not LOS-scored |
| "Generate LOS without rationale per component" | Violates the standard output format |

---

## Worked Example: Cross-Holding Comparison

Suppose a Health Check surfaces three candidates. Same LQS (~90)
across all three, but different other dimensions:

| Ticker | LVS | LGS | LQS | LMS | LOS | Verdict |
|---|---:|---:|---:|---:|---:|---|
| EXAMPLE-X1 | 55 | 92 | 92 | 88 | 84 | **Buy** — premium quality + growth, valuation is the only friction |
| TXN | 82 | 72 | 95 | 64 | 81 | **Buy** — quality compounder, fair valuation, sluggish tape |
| INTU | 70 | 78 | 90 | 75 | 80 | **Buy** — quality + steady growth, modest multiple |
| TSLA | 30 | 75 | 60 | 70 | 58 | **Pass** — quality concerns + stretched valuation offset growth story |

All three "Buy" names eligible for Kelly allocation. TSLA flagged
for replacement if currently owned. None of this overrides the
Sleeve B per-position cap rules (see `state.yaml: sleeve_b_caps` — caps are binding per L3 form).

---

## Related

- See `lios/v1.4.md` — full paste-ready init prompt including RIL
- See `04_replacement_index.md` — LOS + Replacement Index joint workflow
- See `06_health_check_structure.md` — where LOS shows up in Health Check
- See `07_locked_decisions.md` — L11–L14 are the new LOS locked decisions
- See `commands/los*.md` — 9 LOS-specific commands
- See `docs/glossary.md` — definitions of LVS, LGS, LQS, LMS, LOS