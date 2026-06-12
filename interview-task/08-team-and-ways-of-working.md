# Team and Ways of Working

> How I'd move this from problem to shipped, working with Design, Engineering, and stakeholders.

---

## Principles for a 45-person company

At a company this size, process should serve the work, not the other way around. I wouldn't introduce heavyweight frameworks or ceremony that adds coordination overhead without value. The goal is: shared understanding, clear ownership, fast feedback loops, and no surprises.

---

## Discovery phase

**Who's involved:** PM, one engineer (most familiar with payments/sanctions), compliance/legal, the manual review colleague

**What I'd do:**
- 1:1 with the manual review colleague first — they know where the current process breaks, how often flags are triggered, and where the real operational risk is. This is the most undervalued input on this problem.
- Walk through the current system flow with engineering to validate my map and surface any technical constraints I've missed (e.g. whether the API supports scheduled calls, how the 5pm payout batch actually works)
- Legal alignment session: confirm the compliance obligations, get a read on NCA notification requirements, flag the address data gap
- Write up a short problem brief (1–2 pages) — not a formal PRD yet, just enough to align everyone on what we're solving and why before we start designing

**Output:** Shared understanding of the problem, validated assumptions, open questions answered or escalated

---

## Spec and alignment

**Who's involved:** PM, engineering lead, design (if UI is involved — primarily the agent notification and review queue), legal

**What I'd do:**
- Write a lean spec — not a 20-page document. For a compliance feature at a 45-person company, the spec should cover: what the system must do, the decision logic (especially payment hold and NCA trigger), the scenarios (no match / pending / confirmed match), and what's explicitly out of scope
- Use the flow diagrams from doc 06 as the primary communication tool with engineering — a visual is faster than prose for logic-heavy problems
- Run a single alignment session with engineering and legal together before build starts — surface disagreements early, not mid-sprint
- For the review queue extension and agent notifications, a rough wireframe or even a annotated screenshot of the current UI is enough — this doesn't need a full design sprint

**Output:** Agreed spec, signed-off by engineering and legal, ready for sprint planning

---

## Build and review

**Who's involved:** Engineering (build), PM (ongoing), manual review colleague (UAT on review queue), legal (sign-off before release)

**What I'd do:**
- Stay close but not in the way — check in at the start and end of each sprint, not daily
- Prioritise unblocking over updating — if engineering hits a decision point mid-sprint, I want to be the person who resolves it in an hour, not the person who schedules a meeting for Thursday
- Test the critical paths myself before UAT — the payment hold logic, the 5pm payout behaviour, the NCA email trigger. These are the scenarios where a bug has legal consequences
- Involve the manual review colleague early in testing the updated review queue — they'll find issues in 20 minutes that QA would miss in a week
- Stage the rollout: internal test environment → small cohort of pilot letting agents → full release. Don't go straight to 100,000 tenants on a compliance feature

**Output:** Tested, legally signed-off R1 shipped before the 2027 deadline

---

## Stakeholder communication

**Who needs to know what, and when:**

| Stakeholder | What they need | How often |
|---|---|---|
| Engineering lead | Priorities, blockers cleared, decisions made fast | Ongoing — as needed |
| Legal / compliance | NCA notification content, payment hold logic, audit log design | At spec stage and before release |
| Manual review colleague | Changes to the review queue, volume expectations, SLA requirements | Discovery + UAT |
| Letting agents (customers) | That ongoing monitoring is coming, what it means for them, what they'll see | Before R1 ships — not after |
| Senior leadership | Compliance deadline, cost implications, R1/R2 plan | Once at kick-off, once before R1 release |

**On letting agents specifically:**
They should not find out about ongoing monitoring when their first tenant gets flagged. A short comms piece — what's changing, why, what they'll receive if a tenant is flagged — needs to go out before R1 launches. This is a trust issue as much as a product issue.

---

## What I would not do

- **I would not run a formal discovery sprint with workshops and journey mapping.** The problem is well-defined and time-constrained. Lightweight alignment sessions are faster and sufficient.
- **I would not write a full PRD with acceptance criteria for every edge case up front.** The spec should be tight enough to build from, not exhaustive. Edge cases surface in build and get resolved then.
- **I would not gate progress on perfect legal clarity.** Some legal questions (e.g. NCA email format) may not be answerable immediately. Build what we know, flag what's pending, don't let uncertainty become a blocker for the whole epic.
