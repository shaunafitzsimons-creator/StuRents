# Proposed Solution Flow

> End-to-end flow for ongoing sanctions monitoring under the new legislation.

---

## Overview

The new system adds a recurring monitoring layer that runs independently of the contract signing flow. It operates on a monthly schedule, aligned to the rent payment cycle, and introduces three new system capabilities:

1. **Scheduled check trigger** — automated checks run for all 100,000 active rent tenants monthly
2. **Real-time payment hold** — able to freeze a specific tenant's payment before the 5pm payout
3. **NCA notification** — outbound email triggered on confirmed match

---

## Scenario 1 — No match found (expected for the vast majority)

```
Monthly schedule triggers
        ↓
Automated sanctions check runs for each active tenant
(using name, DOB, nationality on record)
        ↓
Score < 0.70
        ↓
Status: 'no match' — logged with timestamp
        ↓
Rent payment cycle proceeds as normal
        ↓
9am: funds arrive in client money account
5pm: funds paid out to letting agent
```

No human intervention required. Audit log updated. Letting agent has no visibility of individual checks — only sees a portfolio-level compliance status (all clear).

---

## Scenario 2 — Automated flag, pending human review

```
Monthly schedule triggers
        ↓
Automated sanctions check runs
        ↓
Score ≥ 0.70
        ↓
Status: 'pending review' — logged with timestamp
        ↓
⚠️ PAYMENT HOLD applied immediately to this tenant
(funds will not be released at 5pm payout regardless of review outcome)
        ↓
Letting agent notified: "Tenant [X] is under review. Rent payment paused pending resolution."
        ↓
Colleague begins manual review using ID document on file
(SLA: must complete before next 5pm payout — max 8 hours if flagged at 9am)
        ↓
┌─────────────────────────┬──────────────────────────┐
│   Manual review:        │   Manual review:          │
│   No match confirmed    │   Match confirmed         │
│         ↓               │         ↓                 │
│  Status: 'no match'     │  Status: 'match'          │
│  Payment hold released  │  Payment hold maintained  │
│  Letting agent notified │  → See Scenario 3         │
│  Funds paid at next     │                           │
│  available payout       │                           │
└─────────────────────────┴──────────────────────────┘
```

**Key design decision:** The payment hold is applied at the point of automated flag, not after human review. This is the safest legal position — funds cannot move while a potential match is unresolved.

**Unresolved at 5pm:** If a manual review is still in progress at 5pm, the hold remains in place. Funds are not released. The review must complete before the next payout cycle.

---

## Scenario 3 — Confirmed match mid-tenancy

```
Manual review confirms: match
        ↓
Status set to: 'match'
        ↓
Two simultaneous actions triggered:

┌────────────────────────────────┐  ┌────────────────────────────────┐
│  PAYMENT HOLD — permanent      │  │  NCA NOTIFICATION              │
│                                │  │                                │
│  Funds remain in client money  │  │  Automated email sent to NCA   │
│  account                       │  │  containing:                   │
│  All future rent payments      │  │  - Tenant full name            │
│  blocked for this tenant       │  │  - Date of birth               │
│  Letting agent notified        │  │  - Current address             │
│  immediately                   │  │                                │
│                                │  │  Notification logged with      │
│                                │  │  timestamp and delivery status │
└────────────────────────────────┘  └────────────────────────────────┘
        ↓
Audit log updated: match confirmed, NCA notified, payment blocked
        ↓
Case handed to compliance / legal for further action
(outside ABC's system scope)
```

**What ABC does not do:**
- ABC does not determine what happens next with the tenant or tenancy
- ABC does not contact the tenant directly
- ABC does not clawback funds already paid to the letting agent in prior cycles
- These decisions sit with the letting agent and their legal counsel

---

## Payment hold logic — decision tree

```
Is there an active payment hold on this tenant?
        ↓
        YES                         NO
        ↓                           ↓
Is the hold status:          Proceed with normal
'pending review'             payout at 5pm
or 'match'?
        ↓
Both: do NOT release
funds at 5pm payout
        ↓
'pending review' → hold until manual review resolves
'match' → hold permanently; NCA already notified
```

---

## NCA notification flow

```
Match confirmed by human reviewer
        ↓
System retrieves from tenant record:
- Full name (from ID document, not self-reported)
- Date of birth
- Current address (collected at tenancy start — see data gap in doc 03)
        ↓
Automated email generated and sent to NCA
        ↓
Email delivery confirmed?
        ↓
YES: logged, timestamp recorded, audit trail updated
        ↓
NO: retry up to 3 times (5-minute intervals)
    If still failing: alert compliance team immediately
    Manual send required; failure logged
```

---

## What this flow does not cover (yet)

- Tenant-initiated data changes mid-tenancy (e.g. name change) — flagged as a question for legal in doc 04
- Checks for the 100,000 tenants not on rent payments — out of scope for compliance; could be a future commercial offering
- Letting agent-facing dashboard for portfolio monitoring status — flagged for Release 2
- Automated triage of review priority (e.g. high-score flags reviewed first) — flagged for Release 2
