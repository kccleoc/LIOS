> **v5 conformance note.** The portfolio has three sleeves — A, B, C, and Z — measured
> against NLV. The Kelly Model and the Strategic Reserve are repealed; deployment is
> governed by P3′. LOS is a research filter, not a sizing tool. Where this file and
> `constitution/constitution.md` disagree, the constitution wins.

# 08 · Research Intelligence Layer (RIL) — LOS Framework

**Component version:** v6
**Status:** Method (`constitution/method.md`)
**Last updated:** 2026-11-15

---

## Purpose

The Research Intelligence Layer introduces a **standardized scoring
language** so every security — current holding, watchlist candidate,
Sleeve B prospect, or research idea — can be evaluated using the
same five-score framework before any capital allocation decision.

RIL does **not** replace the portfolio rules, deployment, sleeve definitions,
or the Replacement Index. It is the **upstream research filter**:

- **LOS decides WHICH assets deserve investment.**
- **P2′ and the deployment rules decide HOW MUCH capital is allocated.**
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

Bands are **fixed by the method** and change with a CHANGELOG entry. Moving a band
boundary is a method change, not a constitutional one.

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

### Example

```
Company: <TICKER>
LOS:    81
LVS:    82  — Forward P/E 18x vs 5-yr avg 22x; FCF yield ~5%
LGS:    72  — Modest revenue growth (~5%); expanding industrial/auto TAM
LQS:    95  — Best-in-class ROIC (~30%); fortress balance sheet; wide moat
LMS:    64  — Range-bound trend; mixed estimate revisions
Recommendation: BUY
Summary: Excellent business quality with attractive valuation,
         moderate growth outlook, and improving momentum.
Confidence: 78%
```

---

## Integration with Existing LIOS

### Deployment (P2′, P3′)
- The Kelly Model is repealed. Deployment has two operations: **Top-up** (monthly,
  gap-driven, never sells) and **Rebalance** (on breach). LOS sizes nothing.
- LOS decides **which** assets are candidates. It never decides how much.
- A LOS < 70 candidate does not receive capital.

### Sleeves (P1′, P2′, P6′)
- Sleeve targets and caps are binding, and every weight is a percentage of NLV.
- LOS ≥ 80 is required for a new Sleeve B name (IC discipline).
- Sleeve A ETFs are not LOS-scored as companies. A thematic ETF is judged by its
  written view statement, not by an aggregate of its constituents.
- Sleeve C is the defensive block — cash, bills and bonds — and receives no LOS.

### Health Check (P8′)
- The report structure is the fixed **14-section** order.
- LOS appears in section 6, the Sleeve B Review. Sleeve C's review is section 7 and
  carries no company scores.
- The Satellite Scoreboard is section 12.

### Replacement Index (Sleeve B)
- The Replacement Index governs Sleeve B replacement, subject to P6′ caps.
- LOS is **complementary**: a candidate must clear LOS 80 **and** pass Replacement
  Index composite superiority plus at least two of four qualitative conditions.
- Replacement is size-neutral: one out, one in, same dollars.
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
   Sleeve assignment and deployment (P2′, P3′)
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
| "LOS < 60 means automatically replace in Sleeve B" | LOS < 60 opens a 30-day review (P5′); replacement still requires the Replacement Index |
| "Calculate LOS using only LMS because momentum is hot" | Disclosure rule — must explain deviation if LMS dominates |
| "Skip LOS for ETFs in Sleeve A" | Correct — Sleeve A vehicles are not LOS-scored |
| "Generate LOS without rationale per component" | Violates the standard output format |

---

## Worked Example: Cross-Holding Comparison

Suppose a Health Check surfaces three candidates. Same LQS (~90)
across all three, but different other dimensions:

| Ticker | LVS | LGS | LQS | LMS | LOS | Verdict |
|---|---|---:|---:|---:|---:|---:|---|
| `<TICKER-A>` | 55 | 92 | 92 | 88 | 84 | **Buy** — premium quality + growth, valuation is the only friction |
| `<TICKER-B>` | 82 | 72 | 95 | 64 | 81 | **Buy** — quality compounder, fair valuation, sluggish tape |
| `<TICKER-C>` | 70 | 78 | 90 | 75 | 80 | **Buy** — quality + steady growth, modest multiple |
| `<TICKER-D>` | 30 | 75 | 60 | 70 | 58 | **Pass** — quality concerns + stretched valuation offset growth story |

All three "Buy" names are eligible for Sleeve B consideration, subject to the B
ceiling and IC approval. `<TICKER-D>` is flagged for a 30-day thesis review if
currently held (P5′) and for replacement scoring if not. None of this overrides the
Sleeve B ceiling, the position cap, or the look-through caps (P6′).

---

## Related

- See the current init prompt in `lios/` — full paste-ready prompt including RIL
- See `04_replacement_index.md` — LOS and Replacement Index joint workflow
- See `06_health_check_structure.md` — where LOS shows up in the Health Check
- See `07_locked_decisions.md` — historical record; L11–L14 are superseded
- See `commands/los*.md` — LOS-specific commands
- See `docs/glossary.md` — definitions of LVS, LGS, LQS, LMS, LOS