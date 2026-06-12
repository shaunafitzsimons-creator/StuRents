# Two-Release Plan

> If we could only ship this over two releases, here's what goes in each and why.

---

## Questions I'd ask before planning this

Before committing to any timeline, I'd need answers to the following:

| Question | Why it matters |
|---|---|
| What is the current SDLC? Sprints, kanban, ad hoc? | Determines how I structure and sequence work |
| What is the typical release cadence — weekly, fortnightly, monthly? | Shapes what "two releases" actually means in calendar time |
| How many engineers are available to work on this, and what are their specialisms? | A backend-heavy team ships the scheduled check differently than a full-stack team |
| Are there other workstreams competing for engineering time in this window? | If the team is already committed elsewhere, the R1 scope needs to shrink |
| Is there an existing test and QA process, and what's the typical cycle time? | Compliance features need rigorous testing — this adds time I need to account for |
| Who is the legal/compliance sign-off, and what's their availability? | NCA notification and payment hold logic will need legal review before shipping |

---

## The constraint

The legislation comes into force in 2027. We are in mid-2026. That gives us roughly 12–18 months. The release split below is based on **priority logic** — legally required versus operationally good — not a precise timeline. The actual dates would be set in conversation with engineering once the above questions are answered.

---

## Release 1 — Compliance minimum
**Goal: Be legally compliant before 2027. Nothing more.**

### What's in

| Capability | Why it's in R1 |
|---|---|
| Scheduled monthly sanctions check for all 100,000 active rent tenants | Core legal requirement — without this we are non-compliant |
| Automated payment hold triggered on score ≥ 0.70 | Funds must not move while a potential match is unresolved — legal exposure if we don't |
| Manual review queue for flagged checks | Already exists; needs to be extended to handle ongoing checks, not just contract-signing ones |
| Permanent payment block on confirmed match | Exists today; must be triggered by ongoing monitoring, not just contract signing |
| NCA email notification on confirmed match | Explicit legislative requirement — name, DOB, current address |
| Current address field added to tenant onboarding | Required for NCA notification; data gap must be closed before R1 ships |
| Basic letting agent notification on payment hold | Agents need to know rent is paused — silent blocking causes relationship damage and support volume |
| Audit log of all ongoing checks and outcomes | Compliance evidence; agents and ABC need to demonstrate the system is working |

### What's explicitly out of R1

| Capability | Why it's out |
|---|---|
| Letting agent portfolio dashboard | Useful but not legally required; email notification covers the minimum |
| Automated review triage / priority scoring | Improves efficiency but the manual queue works without it |
| Automated ID data extraction for check input | Self-reported data is what we have; improving data quality is R2 |
| Tenant-facing communication | Not required by the legislation; adds complexity and legal review time |
| Pricing / billing changes for ongoing monitoring | Commercial decision that can follow R1; absorb cost short-term |
| Checks for 100,000 non-rent tenants | Out of scope for this legislation |

### Key trade-offs in R1

**We are accepting operational fragility to hit the compliance deadline.**
The manual review process will be under pressure at volume. We don't yet know the flag rate. If it's above 1%, the single reviewer will struggle. We are shipping R1 knowing this risk exists, with a plan to address it in R2.

**We are using self-reported data for ongoing checks, not ID-extracted data.**
This is less accurate but faster to build. A tenant with a common name who typed their details incorrectly at onboarding may generate false positives. We accept this risk for R1 and resolve it in R2.

---

## Release 2 — Operational improvement
**Goal: Make the system reliable, scalable, and commercially sustainable.**

### What's in

| Capability | Why it's in R2 |
|---|---|
| Letting agent portfolio dashboard — sanctions monitoring status across all tenants | Reduces inbound support; gives agents confidence the system is working |
| Automated review triage — priority queue by confidence score | High-score flags (e.g. 0.90+) reviewed first; reduces risk of a serious match sitting in a queue |
| Improved data quality — ID-extracted name/DOB used for ongoing checks | Reduces false positive rate; less manual review volume |
| Review SLA enforcement — alert if a flagged check is unresolved within X hours | Closes the operational gap identified in R1; ensures 5pm payout is never missed due to review backlog |
| Pricing model update — recurring charge for ongoing monitoring | Recovers the £120,000/year cost added in R1; converts one-time fee to recurring revenue |
| Retry and failure alerting for NCA notifications | R1 has basic retry logic; R2 makes failure handling robust with escalation paths |
| Frequency review — assess whether monthly is sufficient or needs adjustment | Based on R1 data: flag rates, match rates, operational load — adjust cadence if needed |

### Key trade-offs in R2

**We are delaying the commercial model change.**
We absorb the cost of ongoing monitoring between R1 and R2. This is a deliberate choice — launching a compliance feature alongside a price increase creates friction at exactly the wrong moment. Separate them.

**We are not automating the manual review.**
Automating compliance decisions carries regulatory risk. The human reviewer stays in the loop. R2 makes the human faster and better prioritised — it does not remove them.

---

## High-level epics and sprint breakdown

Assuming 2-week sprints and a typical StuRents-sized team. These are indicative — the actual breakdown would come out of sprint planning with engineering.

### Release 1 Epics

**Epic 1 — Data foundations** *(~1 sprint)*
- Add current address field to tenant onboarding data model
- Backfill or prompt address collection for existing active tenants
- Confirm which data set (self-reported vs. ID-extracted) is used for ongoing checks

**Epic 2 — Scheduled monitoring engine** *(~2 sprints)*
- Build scheduled job to run monthly checks across all 100,000 active rent tenants
- Handle API volume and error states
- Store check results with timestamps in audit log

**Epic 3 — Payment hold logic** *(~2 sprints)*
- Trigger automated hold on score ≥ 0.70 before 5pm payout
- Ensure hold persists until manual review resolves
- Ensure 5pm batch payout respects per-tenant hold status

**Epic 4 — Manual review queue extension** *(~1 sprint)*
- Extend existing review queue to surface ongoing monitoring flags
- Distinguish contract-signing flags from ongoing monitoring flags
- Confirm match/no match updates payment hold status in real time

**Epic 5 — NCA notification** *(~1 sprint)*
- Automated email on confirmed match with name, DOB, current address
- Basic retry logic on send failure
- Delivery logged in audit trail

**Epic 6 — Letting agent notification** *(~1 sprint)*
- Email to letting agent when payment hold applied
- Email to letting agent when hold resolved (no match or match confirmed)

**Epic 7 — Testing, legal review, and release** *(~2 sprints)*
- End-to-end testing across all three scenarios
- Legal sign-off on NCA notification content and payment hold logic
- Staged rollout — internal test → pilot agents → full release

**R1 total: ~10 sprints / ~5 months** *(indicative)*

---

### Release 2 Epics

**Epic 8 — Letting agent dashboard** *(~2 sprints)*
- Portfolio-level monitoring status view
- Filter by: all clear / under review / match confirmed

**Epic 9 — Review triage and SLA** *(~1 sprint)*
- Priority queue ordering by confidence score
- Alert if flagged check unresolved within defined SLA window

**Epic 10 — Improved data quality** *(~2 sprints)*
- Use ID-extracted data for ongoing checks where available
- Reduce false positive rate; measure impact

**Epic 11 — NCA notification hardening** *(~1 sprint)*
- Robust retry and failure escalation
- Manual send fallback with compliance team alert

**Epic 12 — Pricing model update** *(~1 sprint)*
- Recurring charge for ongoing monitoring
- Billing integration and letting agent communication

**R2 total: ~7 sprints / ~3.5 months** *(indicative)*

---

## What we would not do (in either release)

| What | Why not |
|---|---|
| Daily or weekly check cadence at launch | Cost is 10x–30x monthly; not legally required; not operationally ready |
| Automatic tenant notification of sanctions check | Legally complex (could tip off a sanctioned individual); needs legal sign-off we don't have time for |
| Clawback of funds already paid to letting agents | Outside ABC's contractual remit; this is a legal/agent matter |
| Build our own sanctions database | No commercial case; existing API is sufficient and low cost |
| In-app NCA notification (beyond email) | NCA requirement is email; over-engineering the integration adds no compliance value |
