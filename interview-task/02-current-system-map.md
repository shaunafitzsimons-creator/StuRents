# Current System Map

> How the existing sanctions check and rent payment flows work today.

---

## Flow A — Sanctions check (at contract signing)

```
Letting agent issues contract to tenant
        ↓
Tenant inputs personal details: name, date of birth, nationality
Tenant uploads: ID document
        ↓
Tenant signs contract
        ↓
Automated sanctions check runs
(uses self-reported name / DOB / nationality)
        ↓
┌─────────────────────┬──────────────────────────┐
│   Score < 0.70      │      Score ≥ 0.70         │
│   Status: no match  │  Flagged for human review │
│   ✓ Flow continues  │          ↓                │
└─────────────────────┘  Colleague manually       │
                         re-runs check using      │
                         ID document details      │
                         (5 min – 1 hour)         │
                                 ↓                │
                    ┌────────────┴────────────┐   │
                    │  No match found         │  Match found
                    │  Status: no match       │  Status: match
                    │  ✓ Flow continues       │  ✗ Future rent blocked
                    └─────────────────────────┘
```

---

## Flow B — Rent payments (ongoing)

```
Precondition: all parties signed + all sanctions checks = 'no match'
        ↓
Rent collection enabled for that tenant
        ↓
Each month: tenant pays rent by card
        ↓
9am next morning: payment lands in ABC Ltd client money account
        ↓
        [8-hour window — funds sit in client money account]
        ↓
5pm daily: ABC pays out all rent received in last 24hrs to letting agents
        ↓
If sanctions status = 'match' at any point → payment blocked
(currently only possible if flagged at contract signing)
```

---

## Key observations

**1. One trigger, one moment**
The sanctions check fires exactly once: at contract signing. There is no mechanism to re-check a tenant after that point. The entire new legislative requirement sits outside what this system was designed to do.

**2. Self-reported data drives the automated check**
The automated check uses what the tenant typed in — not what's on their ID. The manual review is the only point where the ID document is actually used. A tenant with a low-risk score (< 0.70) based on incorrect self-reported data will never be cross-checked against their ID.

**3. The 9am–5pm window is unprotected**
Funds arrive at 9am and are paid out at 5pm. There is currently no check or trigger in that window. Under the new legislation, a match found at any point during that window must freeze funds before the 5pm payout. That window becomes a critical design constraint.

**4. Human review has no SLA**
The manual review takes anywhere from 5 minutes to 1 hour with no upper bound defined. If a match is flagged close to the 5pm payout, there is no current rule for whether payout proceeds or pauses while review is in progress.

**5. Payment blocking is binary and passive**
Today, a 'match' status blocks future payments automatically. But there is no active notification — not to the letting agent, not to any external body. The new legislation requires an outbound NCA notification, which has no equivalent anywhere in the current system.

**6. Only 50% of tenants are on rent payments**
200,000 tenants use ABC's software for contract signing and sanctions checks. Only 100,000 have rent facilitated by ABC. Ongoing monitoring is only legally required where ABC is involved in the payment flow — the other 100,000 are out of scope for the new requirement.
