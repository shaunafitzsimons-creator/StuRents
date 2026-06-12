# Assumptions and Open Questions

> What I'm assuming, what I'd need answered before building, and what I'd escalate.

---

## Assumptions I'm making

These are decisions I've made to move forward. I'd validate each before committing to build.

| Assumption | Rationale |
|---|---|
| Ongoing monitoring applies only to the 100,000 tenants with active rent payments via ABC | ABC's legal obligation is tied to its role in facilitating payments. The other 100,000 are out of scope. |
| Checks should run monthly, aligned to the payment cycle | The legislation doesn't specify frequency. Monthly is the minimum defensible cadence given rent is monthly. Daily would be gold standard but at 10x–30x the cost. |
| The automated check threshold (0.70) remains unchanged for ongoing monitoring | No indication in the brief that the threshold changes. Consistent with current process. |
| If human review is unresolved at 5pm payout, funds are held by default | Safest legal position. Releasing funds while a review is pending creates compliance exposure. |
| ABC is responsible for sending the NCA notification directly | The legislation places the obligation on the party detecting the match. ABC detects it, so ABC notifies. Would confirm with legal. |
| Current address must be collected at tenancy start, not at the point of a match | Waiting until a match is confirmed to collect address introduces delay and potential tenant evasion. Better to collect upfront. |
| A confirmed false positive should trigger immediate payment unfreeze | The cost of keeping a non-sanctioned tenant's rent frozen is high (agent relationship, tenant hardship). Speed of resolution matters. |

---

## Questions for legal / compliance

1. What does "ongoing monitoring" mean in the legislation — is a frequency specified, or is it left to the implementing party to define?
2. Does the obligation apply to all tenants on the platform or only those where ABC facilitates rent payments?
3. What is the required timeframe for NCA notification after a confirmed match — immediate, within 24 hours, or another window?
4. What is the required format and channel for NCA notification — the brief says email, but is there a structured data requirement?
5. If funds have already been paid out to the letting agent at the point a match is confirmed, where does the freeze obligation sit — with ABC or the letting agent?
6. Is there a requirement to notify the tenant that a sanctions check has been run or that a match has been flagged? (GDPR / transparency obligation)
7. What is the liability exposure if the NCA email fails to send — is there a retry or backup notification requirement?
8. Does ABC need to maintain an audit log of all ongoing checks, and if so, for how long?

---

## Questions for engineering

1. Can the current sanctions check API be called programmatically on a schedule, or does it only support event-driven calls (i.e. triggered by a contract signing)?
2. What is the latency of the automated check — if we need to run 100,000 checks monthly, can the API handle that volume and in what timeframe?
3. Can a payment hold be applied to a specific tenant's funds in the client money account in real time, or does the current architecture only support batch blocking?
4. Is the 5pm payout a manual process, a scheduled job, or an external trigger? Can it be interrupted per-tenant rather than as an all-or-nothing batch?
5. What data do we currently store per tenant post-signing — specifically, do we retain the ID document and extracted data, or is it discarded after the initial check?
6. What would it take to add a current address field to the tenant onboarding data model?
7. Is there an existing email integration we can use for NCA notification, or does this require a new outbound service?

---

## Questions for operations

1. What is the current volume of human reviews per month under the existing one-time check model — i.e. what proportion of 200,000 checks hit the 0.70 threshold?
2. Is the manual review colleague's capacity sufficient to absorb ongoing monitoring reviews on top of the current workload?
3. What are the current operating hours for human review — does someone cover evenings and weekends, or only business hours? (Relevant given the 5pm payout window)
4. What is the current process for letting agents when a tenant is flagged — are they notified today, and if so, how?
5. Is there an existing escalation path if the manual reviewer is unavailable or uncertain — e.g. a compliance lead or external counsel?
