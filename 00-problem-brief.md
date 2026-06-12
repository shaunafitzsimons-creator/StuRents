# Problem Brief: Ongoing Sanctions Monitoring

**Date:** June 2026
**Author:** PM
**Status:** Draft — for alignment, not approval

---

## What's happening and why we need to act

The UK Government is introducing legislation in 2027 that extends the existing sanctions checking obligation on letting agents. Today, a sanctions check is required once — at the point a tenant signs their tenancy agreement. From 2027, that check must become continuous throughout the tenancy.

ABC Ltd currently performs the one-time check on behalf of letting agents automatically within our software. When the new legislation comes into force, our letting agent customers will need to demonstrate ongoing monitoring is in place. If our software doesn't support it, they are non-compliant — and so are we.

This is not optional and it is not a roadmap item. It is a legal deadline.

---

## What the legislation requires

If at any point during a tenancy a tenant is matched against the UK sanctions list:

1. Any rent payment associated with that tenant must be frozen immediately — funds stay wherever they are at the moment the match is determined
2. The National Crime Agency (NCA) must be notified by email with the tenant's name, date of birth, and current address

---

## What we need to build

Our current system was designed for a single trigger: contract signing. It has no mechanism to re-check a tenant after that point, no way to freeze a specific tenant's payment mid-cycle, and no outbound notification to any external body.

At a high level, we need to add three things:

1. **A scheduled monthly check** — automated sanctions checks running for all tenants whose rent payments we facilitate (~100,000 tenants)
2. **A real-time payment hold** — the ability to freeze a specific tenant's funds before they are paid out, triggered the moment a potential match is flagged
3. **An NCA notification** — an automated outbound email sent on confirmed match, containing the required personal data

We also have a data gap to close: the NCA notification requires a tenant's current address, which we do not currently collect.

---

## Who this affects

**Letting agents** — the compliance obligation sits with them. Our software fulfils it on their behalf. If we don't ship this, they are exposed.

**Tenants** — the vast majority will never know this process exists. For the rare tenant flagged, their rent payment will be paused and their data sent to a government agency. Getting false positives resolved quickly matters.

**The manual review colleague** — today they review flags from 200,000 contract signings per year. Ongoing monitoring of 100,000 tenants monthly will increase their workload materially. We need to understand current flag rates before we commit to volume expectations.

**ABC Ltd** — ongoing checks cost £0.10 each. At monthly frequency across 100,000 tenants, that's £120,000/year in new direct costs. The current pricing model does not recover this. A commercial decision on pricing needs to follow R1.

---

## What we are not solving right now

- Sanctions monitoring for the 100,000 tenants not on rent payments — out of scope for this legislation
- Automated resolution of false positives — human review stays in the loop
- A letting agent-facing dashboard for portfolio monitoring status — this comes after we've shipped the compliance core
- Pricing changes — we will absorb the cost in R1 and address in R2

---

## The core tension

The legislation requires speed: freeze funds the moment a match is determined. But our current human review process takes up to an hour with no defined SLA. A flag at 4pm needs to be resolved before the 5pm payout. We need an operational answer to this — either an SLA, escalation path, or a default-hold rule — before we ship.

---

## What we need to align on before design starts

- Confirmed legal interpretation: does "ongoing monitoring" specify a frequency, or do we define it?
- NCA notification requirements: format, timeframe, delivery confirmation expectations
- Current flag rate from existing sanctions checks: this determines operational load
- Engineering assessment: can the current API support scheduled batch calls at this volume?
- Current address collection: do we add this at onboarding going forward, and how do we handle existing tenants?

---

## Proposed next step

Short alignment session with engineering and legal — 60 minutes — to walk through this brief, validate the technical assumptions, and agree on the open questions we need answered before writing the spec.

No decisions need to be made in that session. The goal is shared understanding.
