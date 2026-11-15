> **v5 conformance note.** The portfolio has four sleeves — A, B, C, and Z —
> measured against NLV. The Kelly Factor and Strategic Reserve are repealed.
> Where this file and `constitution/constitution.md` disagree, the constitution wins.


> **Source:** Extracted verbatim from LIOS v1.5. Method content, consolidated in constitution/method.md in v4.1. Describes the PCI execution layer — how LOS, LFS and sleeves integrate.

# Portfolio Construction Intelligence (Execution Engine) — LFS Framework

**Component version:** v1.5 (new module)
**Status:** Method (`constitution/method.md`)
**Last updated:** 2026-07-27

---

## Purpose

The Portfolio Construction Intelligence layer introduces **LFS — LIOS
Fit Score** — a metric that answers one question:

> **"How much does this investment improve the current LIOS portfolio?"**

Unlike LOS, LFS is **not** a company quality score. It is
**portfolio-relative**. The same company may receive different LFS
values as the portfolio evolves.

This layer transforms LIOS from a company-selection framework into
a complete institutional portfolio construction system.

---

## Hierarchy of LIOS (v1.5)

```
Mission
    ↓
LOS — Is the company exceptional?
    ↓
LFS — Does it improve the portfolio?
    ↓
Deployment rules — How much capital, and to which sleeve?
    ↓
Sleeves — Where does it live?
    ↓
Execution
```

LOS still decides **WHICH** assets deserve investment. LFS now
decides **PRIORITY** among investments that pass LOS.

---

## Relationship Between LOS and LFS

| Framework | Question | Nature |
|---|---|---|
| **LOS** | Is this an outstanding investment? | Absolute — evaluates the investment itself |
| **LFS** | Does owning this improve the portfolio? | Relative — evaluates against current portfolio |

Both are required. Neither replaces the other.

---

## LFS Method

### LFS Rule A — LOS is Absolute
LOS evaluates the investment itself. It is independent of portfolio
composition.

### LFS Rule B — LFS is Relative
LFS evaluates the investment against the current portfolio.
It changes whenever portfolio composition changes.

### LFS Rule C — LFS Cannot Rescue a Poor Investment
A weak LOS cannot become a Buy simply because it diversifies the
portfolio. LOS remains the primary investment gate.

### Rule 4 — LFS is Dynamic
LFS changes whenever portfolio composition changes — even if the
company itself has not changed.

### Rule 5 — Preference Rule (Official Decision Rule)
When LOS values are similar, LIOS should prefer the investment
with the higher LFS. This is now an official decision rule.

---

## The Five LFS Components

Each dimension is scored 0–100. The composite LFS is a
professional-judgment weighted blend.

### 1. Diversification Value
**Question:** Does the company introduce a new economic engine?

**Examples of economic engines:**
- Industrial Automation
- Healthcare Innovation
- Consumer Spending
- Digital Payments
- Energy Infrastructure
- Defense
- Enterprise Software
- AI Compute / AI Memory / AI Platforms
- Cloud Infrastructure
- Logistics
- Utilities

Higher diversification value → Higher LFS.

### 2. Strategic Fit
**Question:** Does this company strengthen the long-term LIOS mission?

**Sub-questions:**
- Supports durable compounding?
- Fits existing sleeve philosophy?
- Strengthens portfolio resilience?

### 3. Overlap
**Question:** How much exposure already exists?

| Scenario | LFS Impact |
|---|---|
| Fourth AI infrastructure company | Lower LFS |
| First industrial automation leader | Higher LFS |

### 4. Conviction Compatibility
**Question:** Can LIOS realistically understand and monitor this
business over the coming years?

Higher understanding → Higher LFS.

### 5. Portfolio Balance
**Question:** Does adding this position improve overall portfolio
construction?

**Examples of improvement:**
- Lower cyclicality
- Broader cash-flow drivers
- Reduced concentration
- New demand drivers
- Complementary earnings profile

---

## Economic Engine Principle

Diversification is **not** measured by sector labels alone. LFS
evaluates diversification by **underlying economic drivers**.

Adding another company driven by the **same** economic engine
should receive a **lower** LFS than adding a high-quality company
driven by a **new** economic engine.

**Example:**
- Adding a second AI compute company (same engine as your existing AI compute holding) → LFS 65
- Adding a first healthcare innovator (new engine) → LFS 88

---

## LFS Scale

| LFS Range | Label | Meaning |
|---|---|---|
| **90–100** | **Exceptional** | Adds meaningful diversification without sacrificing expected returns |
| **80–89** | **Strong Fit** | Improves balance while maintaining high quality |
| **70–79** | **Useful** | Adds some diversification but is not essential |
| **60–69** | **Neutral** | Does not materially improve the portfolio |
| **Below 60** | **Poor Fit** | Adds unnecessary overlap or weak strategic value |

---

## Investment Decision Matrix

LOS and LFS must be evaluated together:

| LOS | LFS | Decision |
|---|---|---|
| **High** | **High** | **Highest priority for new capital** — exceptional company that improves the portfolio |
| **High** | **Low** | **Excellent company, but lower portfolio priority** — consider for long-term watchlist; deploy elsewhere first |
| **Low** | **High** | **Do not buy.** Diversification alone is insufficient. LOS is the primary gate. |
| **Low** | **Low** | **Reject.** Neither the company nor its fit justifies investment. |

---

## Worked Example: LFS is Dynamic

### `<TICKER>` — Initial Evaluation

| Metric | Value |
|---|---|
| LOS | 84 (Buy) |
| LFS | 92 (Exceptional) |
| Rationale | Adds analog/mixed-signal semiconductor engine — highly complementary to digital AI compute positions |

### `<TICKER>` — After Becoming a Large Position

| Metric | Value |
|---|---|
| LOS | 84 (Buy) — company unchanged |
| LFS | 78 (Useful/Strong Fit) |
| Rationale | Portfolio now has significant semiconductor concentration. Additional exposure has diminishing diversification benefit. |

**Company has not changed. Portfolio has.**
LFS accurately reflects the evolving portfolio fit.

---

## Standard Research Output Format (LFS-Augmented)

Every company researched under LIOS v1.5+ includes LFS alongside LOS:

```
Company: <TICKER>
LOS:    <NN>  — <band>
LVS:    <NN>  — <one-line rationale>
LGS:    <NN>  — <one-line rationale>
LQS:    <NN>  — <one-line rationale>
LMS:    <NN>  — <one-line rationale>
LFS:    <NN>  — <band>
  Diversification Value: <NN> — <rationale>
  Strategic Fit:         <NN> — <rationale>
  Overlap:               <NN> — <rationale>
  Conviction:            <NN> — <rationale>
  Portfolio Balance:     <NN> — <rationale>
Decision Matrix: <LOS band> + <LFS band> → <Buy/Hold/Pass/Watch>
Recommendation: <BUY / HOLD / REDUCE / EXIT / PASS / WATCH>
Summary: <one sentence>
Confidence: <XX>%
```

---

## New Standard Workflow (v1.5)

```
Idea
  ↓
Generate LOS
  ↓
LOS below threshold ──→ Reject
  ↓
LOS acceptable
  ↓
Generate LFS
  ↓
Evaluate Decision Matrix (LOS + LFS)
  ↓
Top-up (gap-driven)
  ↓
Sleeve assignment
  ↓
Execution
```

---

## Integration with Existing LIOS

### LOS (v1.4 — unchanged)
- LOS remains the upstream research filter.
- LFS is evaluated **after** LOS passes threshold.
- A candidate with LOS < 70 does not advance to LFS evaluation.
- The research-layer method is unaffected.

### Deployment (v4.1 — replaces Kelly Factor)
- The Kelly Model and its Factor are repealed. Deployment is governed by
  `core/deployment.md`: **Top-up** (monthly, gap-driven, never sells) and
  **Rebalance** (on band or cap breach, sells, direction-limited by P4′).
- LFS informs **priority ranking** among LOS-qualified candidates. It does not
  size the contribution — the gap does.
- When multiple LOS-qualified candidates compete for capital, higher LFS receives
  capital first.

### Sleeves (v5)
- Sleeve targets and caps follow P2′ and P6′.
- Sleeve assignment rules are untouched.
- LFS may influence **which sleeve** when LOS values are similar.
- Sleeve A (broad-index ETFs) does not receive LFS scoring.

### Replacement Index (Sleeve B)
- The Replacement Index method governs Sleeve B replacements, subject to P6′ caps.
- LFS is an **additional input** when comparing candidates with similar Replacement Indices.
- LFS does not override Replacement Index superiority.

### Health Check
- P8′ requires the 15-section fixed order.
- Sleeve C is the duration sleeve. The satellite review is Sleeve B in section 6,
  where each holding shows LFS beside LOS.
- New subsection in §15 (Risk Assessment) for portfolio construction intelligence.

---

## Anti-Patterns (will be rejected)

| Trigger | Why it's not a valid LFS use |
|---|---|
| "LFS 92, so buy it despite LOS 58" | P6′ — LFS cannot rescue a poor investment. LOS is the primary gate. |
| "LFS is low so sell a great company" | Strong LOS + low LFS = excellent company, lower portfolio priority. Not a sell signal. |
| "Use LFS instead of the Replacement Index" | The Replacement Index governs Sleeve B replacements. LFS is additive. |
| "The sector is different so LFS is automatically high" | Economic Engine Principle — sector labels alone do not determine diversification. |
| "Calculate LFS once and it's permanent" | LFS is dynamic. Re-score after portfolio changes. |
| "Replace LOS with LFS" | They measure different things. Both are required. |

---

## When To Re-Score LFS

LFS values are dynamic. Re-score after:

- Any new position added (any sleeve)
- Any material position increase (> 20% size change)
- Any position exit
- Significant price move that changes relative weights
- After any LOS re-score (if LOS changes, LFS may need re-evaluation)
- Any time Lc asks "does this portfolio make sense?"

---

## New Standard Commands (v1.5)

| Command | When to use | Output |
|---|---|---|
| `/lfs` | Score a single company's LFS | Standard LFS research block |
| `/los-lfs` | Combined LOS + LFS evaluation | Full decision matrix output |
| `/portfolio-fit` | Full portfolio-fit analysis | All holdings by LFS with fit commentary |
| `/portfolio-balance` | Portfolio construction assessment | Balance gaps, overlaps, recommendations |
| `/economic-engine-map` | Map holdings by economic engine | Engine map + concentration analysis |
| `/best-diversifiers` | Identify top diversifiers | Ranked by diversification contribution |
| `/replacement-candidates` | Find best replacement candidates | Candidates scored by LOS + LFS + RI |
| `/portfolio-rotation` | Capital rotation opportunities | Rotate from low-LFS to high-LFS names |

---

## Related

- See `lios/v1.5.md` — full paste-ready init prompt including LFS
- See `constitution/method.md` — LFS method
- See `06_health_check_structure.md` — where LFS appears in Health Check
- See `commands/lfs.md`, `commands/los-lfs.md`, etc. — v1.5 command files
- See `08_research_intelligence_layer.md` — LOS framework (complementary)
- See `docs/glossary.md` — LFS definition
