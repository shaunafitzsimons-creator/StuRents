# Success Metrics

> How we define success before we ship, and how we know it worked after launch.

---

## Principle

For a compliance feature, success has two layers that must both be true:
1. **Legal compliance** — the system does what the legislation requires, every time
2. **Operational health** — the system doesn't break the business while doing it

A feature that is legally compliant but generates 500 false positives a month, freezes legitimate rent payments, and overwhelms the manual reviewer is not a success. Both layers matter.

---

## Compliance metrics

These are non-negotiable. If any of these fail, the product has failed regardless of everything else.

| Metric | Target | How measured |
|---|---|---|
| % of active rent tenants receiving monthly sanctions check | 100% | Audit log — checks run vs. tenants on rent payments |
| % of confirmed matches where payment hold was applied before next payout | 100% | Payment log cross-referenced with match timestamps |
| % of confirmed matches where NCA was notified | 100% | NCA notification log — sent and delivery confirmed |
| % of NCA notifications sent within defined timeframe of confirmed match | 100% | Timestamp delta: match confirmed → email sent |
| Zero instances of funds released for a tenant with 'match' status | 0 breaches | Payment audit log — manual review post-launch |

---

## Operational metrics

These tell us whether the system is working without breaking the team or damaging agent relationships.

| Metric | Target | How measured |
|---|---|---|
| False positive rate (score ≥ 0.70, resolved as 'no match') | Baseline TBD from R1 data; aim to reduce in R2 | Review queue outcomes |
| Manual review resolution time | 95% resolved within 4 hours of flag | Timestamp: flagged → resolved |
| Payment holds released same-day (false positives resolved before 5pm) | >80% | Hold applied vs. hold released timestamps |
| Inbound support contacts from letting agents related to payment holds | Tracked; aim for <5% of hold events generating a support contact | Support ticket tagging |
| NCA notification delivery failure rate | <1%; zero unresolved failures | Notification log — failed sends with no manual resolution |

---

## Business metrics

These tell us whether the feature is commercially sustainable.

| Metric | Target | How measured |
|---|---|---|
| Cost of ongoing monitoring as % of check revenue | Tracked monthly; inform R2 pricing decision | Finance — check costs vs. revenue |
| Letting agent churn attributable to monitoring-related friction | 0 | Customer success — exit interview tagging |
| Time to full R1 rollout (all 100,000 tenants on monitoring) | Within 4 weeks of release | Deployment log |

---

## How we'd know it worked after launch

**Week 1–2 — Watch closely**
- Monitor the first monthly check run end-to-end: did all 100,000 checks fire? Did any payment holds apply incorrectly? Did any NCA emails fail?
- Daily check on the manual review queue — is volume manageable? Are reviews resolving within SLA?
- Any letting agent contacts about unexpected payment holds?

**Month 1 — Establish baselines**
- Flag rate from the first full monthly run — this is the number we didn't have before launch
- Average review resolution time
- False positive rate
- Support contact volume
- These baselines feed directly into the R2 scope and the pricing decision

**Month 3 — R1 retrospective**
- Are all compliance metrics at 100%?
- What's the operational load — is one reviewer sufficient or do we need to add capacity?
- Is the false positive rate acceptable or does it need to be addressed in R2 sooner than planned?
- Are letting agents aware of and comfortable with the monitoring process?

**The question that tells us R1 genuinely worked:**
> "If a regulator audited our sanctions monitoring process today, could we demonstrate that every active rent tenant was checked monthly, every match was caught, every NCA notification was sent, and every payment hold was correctly applied?"

If the answer is yes, R1 has done its job.
