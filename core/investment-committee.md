> **Source:** Derived from LIOS v1.5 governance principle and locked decisions.

# Investment Committee

## Composition

The Investment Committee (IC) consists of Lc (Leo) + the active AI session.
All recommendations produced by LIOS are advisory. The IC has final decision authority.

## Authority

The IC has **sole authority** on:
- Strategic Reserve (SR) deployment (L6)
- Approval, deferral, or rejection of any trade recommendation
- Sleeve D replacement surgery approval
- New economic engine classification
- Portfolio construction changes

The IC may **NOT**:
- Modify Locked Decisions (L1-L18) without a major version bump (v2.0+) and constitutional amendment
- Skip Health Check sections (L10 — 16-section fixed order)
- Deploy SR outside enumerated triggers (L6)
- Override Sleeve B caps (L3 form — binding)
- Re-enter a ticker on the portfolio-local permanently-exited list (formerly L7; lives in `state.yaml: sleeve_a.permanently_exited`)

## Meeting Workflow

1. **Review** — Load current snapshot, review portfolio state
2. **Discuss** — Evaluate watchlist, triggers, drift, risks
3. **Decide** — Issue directives: Buy / Hold / Reduce / Exit / Pass
4. **Record** — Update workspace/committee-notes.md, issue CIO Resolution
5. **Export** — Update portfolio snapshot via /portfolio-print

## CIO Resolution

Every IC meeting produces a one-paragraph CIO Resolution:
"What should Lc do next?" — specific, actionable, with Confidence %.

## Related

- constitution/governance.md — governance principle
- constitution/constitution.md — all Locked Decisions
- playbooks/InvestmentCommittee.md — IC meeting workflow playbook
- workspace/committee-notes.md — current meeting notes
