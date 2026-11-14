> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


> Quick reference — full methodology at core/research-layer.md

# LOS — LIOS Overall Score

## The Five Scores

| Code | Score | Measures | Typical inputs |
|---|---|---|---|
| **LVS** | LIOS Value Score | Valuation attractiveness | Forward P/E, PEG, EV/EBITDA, FCF Yield, historical valuation, margin of safety |
| **LGS** | LIOS Growth Score | Long-term compounding potential | Revenue growth, EPS growth, cash flow growth, estimate revisions, industry growth, TAM |
| **LQS** | LIOS Quality Score | Business excellence | ROIC, ROE, gross margin, operating margin, FCF, balance sheet, capital allocation, moat |
| **LMS** | LIOS Momentum Score | Market confirmation | Relative strength, trend, earnings surprises, estimate revisions, institutional accumulation, technical strength |
| **LOS** | LIOS Overall Score | Composite attractiveness | Weighted blend of LVS/LGS/LQS/LMS with professional judgment |

All four component scores are 0-100. Higher = better on that dimension.

## Default Weights

| Component | Default Weight |
|---|---|
| LQS | 35% |
| LGS | 25% |
| LVS | 25% |
| LMS | 15% |

Weighting deviations must be disclosed per score with one-line rationale.

## LOS Interpretation Bands

| LOS Band | Label | Action |
|---|---|---|
| **90–100** | **Elite** | Highest conviction. Core compounder. Aggressively accumulate during normal pullbacks. Rarely sell on valuation alone. |
| **80–89** | **Buy** | Accumulate. Suitable for regular DCA. Eligible for Sleeve A or Sleeve B. |
| **70–79** | **Watch** | Build watchlist. Buy on meaningful correction. Starter position acceptable. |
| **60–69** | **Hold** | Existing holders monitor. New purchases require a strong catalyst. |
| **Below 60** | **Pass** | Avoid. Candidate for replacement if currently owned. |

## Standard Research Output Format

```
Company: <TICKER>
LOS:    <NN>  — <band>
LVS:    <NN>  — <one-line rationale>
LGS:    <NN>  — <one-line rationale>
LQS:    <NN>  — <one-line rationale>
LMS:    <NN>  — <one-line rationale>
LFS:    <NN>  — <band>
Decision Matrix: <LOS band> + <LFS band> → <decision>
Recommendation: <BUY / HOLD / REDUCE / EXIT / PASS / WATCH>
Summary: <one sentence>
Confidence: <XX>%
```

## Candidate Research Workflow

```
Discover Candidate → Generate LOS → LOS < 70 → STOP
                                    → LOS 70-79 → WATCHLIST
                                    → LOS 80+ → DEEP RESEARCH → Kelly → Execution
```

Core: core/research-layer.md | Playbook: playbooks/ResearchWorkflow.md
