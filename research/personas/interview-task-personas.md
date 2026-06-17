# Interview Task — User Personas
## Ongoing Sanctions Monitoring Feature

Three personas relevant to the 2027 sanctions monitoring legislation change.

---

## Persona 1: The Letting Agent
**"Maya" — Branch Manager, mid-sized independent agency**

**Background**
- 8 years in lettings, manages a team of 3
- Not a tech person — adopted property management software because regulations forced her to
- Juggles AML, Right to Rent, and sanctions checks simultaneously across multiple tenancies

**Goals**
- Get tenancies signed quickly — void periods cost her landlord clients money
- Stay compliant without it eating her day
- Not be personally liable for a sanctions breach she didn't know about

**Frustrations**
- False positives on common names waste hours with no tooling support
- When a check is flagged for review, she has no visibility on how long it'll take — she can't tell her tenant or landlord anything
- Compliance feels like a separate job bolted onto her actual job
- OFSI response times can extend void periods by days

**What she needs from the software**
- Real-time visibility on check status
- Clear alerts if a mid-tenancy match occurs — she needs to know before her landlord calls her
- Audit trail she can export if OFSI ever asks questions
- Confidence that ABC is handling the NCA notification — she doesn't want to own that

---

## Persona 2: The Tenant
**"Liam" — 27, moving to London for a new job, first time renting privately**

**Background**
- Renting alone for the first time after moving out of student accommodation
- Digitally comfortable but has never heard of sanctions checks
- On a deadline — needs to be in the flat before his job starts

**Goals**
- Get into the flat as fast as possible
- Understand what's being asked of him and why
- Not feel like he's being treated as a suspect

**Frustrations**
- Being asked to upload his passport feels invasive if not explained
- Delays with no explanation make him anxious — is something wrong with his application?
- If his rent payment gets frozen mid-tenancy with no warning, he'd be terrified

**What he needs from the software**
- Clear, plain-English explanation of why his details are being checked
- Status updates so he's not in the dark
- If flagged (false positive), a fast and human resolution process

---

## Persona 3: The Internal Compliance Reviewer
**"Priya" — Compliance & Operations Lead, ABC Ltd**

**Background**
- One of two people covering compliance at a 45-person company
- Owns sanctions check reviews, AML policy, and the SAR filing process
- Uses a mix of internal tools and third-party APIs — no single system does everything

**Goals**
- Clear the review queue before the 5pm payment run
- Never miss a genuine match — personal liability is real
- Not be overwhelmed by false positives eating her day

**Frustrations**
- Today she reviews flagged checks reactively, with no SLA or tooling support
- When ongoing monitoring arrives, the volume of checks will multiply massively — her current process won't scale
- Alert fatigue from high false positive rates is already a problem
- A match arriving at 4:55pm on a Friday has no clear resolution path

**What she needs from the software**
- Prioritised review queue (highest confidence scores first)
- Clear deadline visibility — which reviews must resolve before the 5pm payout
- One-click NCA notification workflow so she's not manually drafting emails under pressure
- Full audit log for every check, every decision, every notification

---

## Why these personas matter

The legislation change hits all three differently:

| Persona | Impact of ongoing monitoring |
|---|---|
| **Maya** | Needs confidence ABC handles compliance automatically — she can't check tenants monthly herself |
| **Liam** | May never know it's happening — but a false positive mid-tenancy that freezes his rent is a catastrophic UX failure |
| **Priya** | Her job fundamentally changes — manual process breaks at scale |

This tension drives the two-release strategy: v1 achieves legal compliance, v2 makes it operationally sustainable for Priya and transparent for Maya and Liam.
