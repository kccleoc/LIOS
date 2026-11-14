> **Source:** Derived from the LIOS governance principle and Locked Decisions.
> **Version:** v3.0

# Investment Committee

## Composition

The Investment Committee (IC) is the investor plus the active AI session. All
recommendations are advisory. The IC holds final decision authority.

## Authority

The IC has **sole authority** on:

- Approval, deferral or rejection of any trade recommendation
- Sleeve B entry (rare) and any thesis-break exit (L22)
- Sleeve C replacement surgery (L5)
- The Rule 3 discretionary channel — one event per quarter, max 3% of NLV,
  funded from D or Z only
- Economic-engine classification
- Portfolio-construction changes that do not touch a Locked Decision

The IC may **NOT**:

- Modify Locked Decisions (L1–L23) without a major version bump and a
  constitutional amendment
- Skip Health Check sections (L10 — 16-section fixed order)
- Change total risky exposure because of a market view (L21)
- Move money outside the three deployment rules (L20)
- Override Sleeve B caps (L3 form — binding)
- Sell a Sleeve B name on price (L22)
- Re-enter a ticker on the portfolio-local permanently-exited list
  (`state.yaml: sleeve_a.permanently_exited`)

**Removed in v3.0:** the IC no longer authorises Strategic Reserve deployment,
because the Strategic Reserve does not exist. Drawdown response is Rule 2 (the
rebalance band), which requires no approval — it is mechanical.

## Meeting Workflow

1. **Review** — load state, validate, review the 16-section Health Check
2. **Discuss** — gaps, constraints, BBI band, research changes, risks
3. **Decide** — issue directives: Buy / Hold / Reduce / Exit / Pass
4. **Record** — update workspace notes, issue the CIO Resolution
5. **Export** — update the snapshot via `/snapshot`

## CIO Resolution

Every meeting produces a one-paragraph resolution: what happens next, in dollars,
with a confidence percentage.

## Related

- `constitution/governance.md` — governance principle
- `constitution/constitution.md` — all Locked Decisions
- `core/deployment.md` — the three money rules
- `playbooks/InvestmentCommittee.md` — meeting workflow
