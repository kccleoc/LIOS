> **Source:** Derived from the LIOS governance principle and P1′–P8′.
> **Version:** v5

# Investment Committee

## Composition

The Investment Committee (IC) is the investor plus the active AI session. All
recommendations are advisory. The IC holds final decision authority.

**That is one human and whichever session happens to be open, so "an IC decision" needs a
written test, or it is just a word for "I decided".** Sleeve B is funded by IC decision and
receives no contribution, which is the only thing separating it from the discretionary sleeve
v4.1 retired. A decision exists only if all five are recorded, dated, in the SSOT's history:

1. the candidate, and the sleeve it enters;
2. its LOS and its Replacement Index against the weakest incumbent;
3. the dollar amount, the B ceiling headroom it consumes, and the look-through it creates;
4. the alternative that was rejected, and why;
5. the condition that would reverse it.

No record, no funding. A verbal or chat-only approval is a deferral, not a decision. This is
deliberately bureaucratic: it is the only control on the one sleeve where judgment replaces
arithmetic.

## Authority

The IC has **sole authority** on:

- Approval, deferral or rejection of any trade recommendation
- Sleeve B entry (rare) and any thesis-break exit (P5′)
- Sleeve B replacement decisions (Replacement Index method)
- The Investment Committee has no discretionary deployment channel. A drawdown
  response is Rebalance, which is mechanical and needs no approval
- Economic-engine classification
- Portfolio-construction changes that do not touch a P-rule

The IC may **NOT**:

- Modify a P-rule (P1′–P8′) without a major version bump and a
  constitutional amendment
- Skip Health Check sections (P8′ — 15-section fixed order)
- Change total risky exposure because of a market view (P2′)
- Move money outside the two deployment operations (P3′)
- Override the Sleeve B cap (P6′)
- Sell a Sleeve B name on price (P5′)
- Re-enter a ticker on the portfolio-local permanently-exited list
  (`state.yaml: sleeve_a.permanently_exited`)

**Removed in v3.0:** the IC no longer authorises Strategic Reserve deployment,
because the Strategic Reserve does not exist. Drawdown response is Rebalance (the
rebalance band), which requires no approval — it is mechanical.

## Meeting Workflow

1. **Review** — load state, validate, review the 15-section Health Check
2. **Discuss** — gaps, constraints, RI band, research changes, risks
3. **Decide** — issue directives: Buy / Hold / Reduce / Exit / Pass
4. **Record** — update workspace notes, issue the CIO Resolution
5. **Export** — update the snapshot via `/snapshot`

## CIO Resolution

Every meeting produces a one-paragraph resolution: what happens next, in dollars,
with a confidence percentage.

## Related

- `constitution/governance.md` — governance principle
- `constitution/constitution.md` — the eight Portfolio Rules (P1′–P8′)
- `constitution/method.md` — scoring, reports, workflow
- `core/deployment.md` — the two deployment operations
- `playbooks/InvestmentCommittee.md` — meeting workflow
