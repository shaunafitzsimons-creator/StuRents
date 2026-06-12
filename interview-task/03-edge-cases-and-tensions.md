# Edge Cases and Tensions

> Where the current system has gaps, and where the new requirement creates complexity.

---

## Timing tensions

**The 9am–5pm payment window**
Rent lands in ABC's client money account at 9am. Payout to letting agents is at 5pm. If a sanctions match is confirmed at any point in that window, the legislation requires funds to remain where they are. That means:
- A match confirmed at 4:58pm must still block the 5pm payout
- The system needs a real-time hold trigger, not a batch check
- If human review is in progress at 5pm with no resolution, the default must be: hold, not release

**The monthly rent cycle creates 12 exposure windows per year**
Under monthly card payments, there are 12 moments per year where funds are in transit through ABC's client money account. Each one is a potential freeze point. The system must be ready to intercept at any of these moments, not just once.

**Retrospective match on funds already paid out**
If a check runs and a match is confirmed after the 5pm payout has already been processed, the funds are no longer in ABC's account — they're with the letting agent. The legislation says funds must remain in "whatever bank account they are in at the point the match is determined." This needs a clear definition: is ABC responsible for clawback, or does the obligation transfer to the letting agent? This is a legal question, not a product question — but the product needs to handle both outcomes.

---

## Data quality issues

**Self-reported data vs. ID document**
The automated check runs on tenant-typed data. A tenant whose name is transliterated differently on their ID than they typed at onboarding could return a low automated score — and therefore never trigger human review — despite the ID being the more authoritative source. For ongoing monitoring, the question is: which data set do we run recurring checks against? Self-reported (consistent but potentially wrong) or ID-extracted (more accurate but requires a data extraction step that doesn't exist today)?

**Address data is not currently collected at onboarding**
The NCA notification requires the tenant's current address. The current system collects name, DOB, and nationality. Current address is not part of the onboarding data model. This is a data gap that needs to be filled before the new legislation comes into force — either at onboarding or pulled from the tenancy agreement.

**Tenants who update their details mid-tenancy**
If a tenant changes their name (marriage, legal change) or is flagged under a previous name, the monitoring system needs to know which version of the data to run checks against, and whether a data change should trigger an immediate re-check.

---

## Operational gaps

**No SLA on human review**
The current review window is 5 minutes to 1 hour. Under ongoing monitoring, a flagged check could arrive at any time — including 4:50pm. Without a defined SLA and an escalation path, the manual review process is the weakest link in the compliance chain.

**Volume of ongoing checks is undefined**
The brief gives us 100,000 tenants on rent payments. If checks run monthly (aligned to payment cycle), that's 100,000 checks per month at £0.10 each = £10,000/month in direct costs. If checks run weekly or daily, costs and human review load scale proportionally. The legislation does not specify frequency — this is both a product decision and a cost question.

**No letting agent-facing visibility**
Today, letting agents have no dashboard view of sanctions check statuses across their tenant portfolio. Under ongoing monitoring, an agent whose tenant is flagged needs to know. There is currently no notification or reporting surface for this.

---

## New obligations with no current equivalent

**NCA notification**
An outbound email to a government body containing regulated personal data (name, DOB, current address) is a new integration type. It raises questions the current system has no answer for:
- What is the NCA's required format, data fields, and response expectation?
- What is the required timeframe — immediate on confirmed match, or within X hours?
- What happens if the email fails to send? Is there a retry obligation? An audit trail requirement?
- Does ABC send this directly, or does it notify the letting agent who then notifies the NCA?

**Audit trail for compliance**
If a letting agent is ever investigated for sanctions non-compliance, they will need to demonstrate that ongoing monitoring was running and that ABC's software performed it correctly. There is currently no audit log designed for this purpose.

**False positive handling mid-tenancy**
Today, a false positive at contract signing delays the tenancy starting. Mid-tenancy, a false positive is more damaging: rent is frozen, the tenant potentially can't pay, and the letting agent is stuck. The process for resolving a false positive quickly — and unfreezing payments — needs to be defined and fast.
