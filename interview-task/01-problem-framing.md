# Problem Framing

> What are we actually solving, and for whom?

---

## The regulatory trigger

The UK Government is proposing legislation (2027) that extends the existing sanctions checking obligation on letting agents from a one-time check at contract signing to **continuous monitoring throughout the tenancy**.

Under the proposed change, if a tenant is matched against the UK sanctions list at any point during their tenancy:
- Any rent payments associated with that tenant must be frozen immediately — funds stay wherever they are at the moment the match is determined
- The National Crime Agency (NCA) must be notified by email with the tenant's name, date of birth, and current address

ABC Ltd currently performs the one-time check on behalf of letting agents automatically. The new obligation lands on the same letting agents — and by extension, on ABC's software to fulfil it.

---

## The product problem

ABC Ltd's current system was designed for a single trigger: contract signing. There is no mechanism for ongoing monitoring, no payment-pause logic tied to a mid-tenancy event, and no outbound notification to any government body.

This is not a feature extension. It requires:
1. A new recurring check trigger (currently none exists)
2. New payment-hold logic at a different point in the payment lifecycle than today
3. A new external integration (NCA email notification) that involves regulated personal data
4. A human review workflow that can operate within a time-sensitive payment window

The core tension: **compliance requires speed** (freeze funds the moment a match is determined), but the current human-review process takes up to an hour and has no defined SLA. The system needs to handle both automated and human-reviewed matches without creating a window where funds move when they shouldn't.

---

## Who is affected

| Who | How |
|---|---|
| Letting agents | Compliance obligation sits with them; ABC's software must fulfil it on their behalf — failure exposes the agent |
| Tenants | Could have rent payments blocked and personal data sent to NCA — most will never know this system exists until it affects them |
| ABC Ltd (operations) | The colleague doing manual reviews faces a significant increase in workload if checks run frequently across 100,000 active tenants |
| ABC Ltd (commercial) | Ongoing monitoring has a direct cost per check (£0.10) that doesn't exist today; pricing model needs to reflect this |
| NCA | New recipient of structured notifications — their requirements for format, timing, and data fields are not yet defined in the brief |

---

## What success looks like at a high level

1. ABC Ltd is fully compliant with the new legislation before it comes into force in 2027
2. No sanctioned tenant's rent payment is processed after a confirmed match
3. The NCA is notified correctly and within a defined timeframe
4. Letting agents have visibility of the monitoring status of their tenants without needing to intervene manually
5. The operational load on the manual review colleague is manageable and bounded by an SLA
6. The cost of ongoing monitoring is priced into the product so it doesn't erode margin
