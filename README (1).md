# Alert Triage Workflow: Cloud Security Case Study

A product case study for a cloud security tool. Option chosen: **Alert Triage Workflow**, showing how a security engineer investigates and resolves a cloud security alert.

## Files in this repo

| File | What it is |
|---|---|
| `README.md` | The full case study (this file) |
| `Alert_Triage_Workflow.pptx` | 9-slide presentation with speaker notes |
| `presentation-script.md` | What to say on each slide (about 3 minutes) |

## 1. Problem statement

Cloud security tools such as AWS GuardDuty, Wiz and Prisma Cloud generate hundreds of alerts a day. When an alert fires, the engineer has to open the cloud console, the logs, the IAM page and the ticketing tool just to answer three questions: *Is this real? How bad is it? What do I do?* This is slow, and it causes alert fatigue, where real threats get ignored among noise.

**Goal:** Cut the time from "alert fires" to "alert resolved with a recorded reason" by putting context, evidence and next actions on one screen.

## 2. Persona (assumed)

**Rahul, Cloud Security Engineer.** 3-4 years of experience, at a mid-size SaaS company with 4-5 AWS accounts and a 3-person security team.

- **Day-to-day tools:** cloud console, Slack, Jira, a log tool.
- **Pains:** too many alerts, no clear priority, repeated investigation of the same false positives, and chasing developers for fixes.
- **Needs:** fast triage, evidence he can trust, and a clear audit trail for compliance (SOC 2, ISO 27001).

*This persona is an assumption, not based on user research.*

*Why context matters:* In the 2019 Capital One breach, an over-permissioned role on a misconfigured server gave attackers access to S3 data. An alert like "unusual API call" is only urgent if you can see that the role had broad access to sensitive data. That is the kind of context the tool should surface.

## 3. Wireframe: 3 screens

> Add your Figma link here: `<paste Figma link>`
> Optional: add exported screen images to an `images/` folder and embed them, e.g. `![Alert Queue](images/screen-1.png)`

### Screen 1: Alert Queue (the "inbox")

- Table columns: Risk score, Alert title, Resource, Account, Age, Status, Owner.
- Top filters: Severity, Account, Status, "Assigned to me".
- A "Why it matters" one-liner under each title, e.g. "Internet-facing server, holds customer data".
- Grouped duplicates, e.g. "12 similar alerts".
- **Annotation:** sort by *risk score*, not just severity. Risk means severity combined with exposure and data sensitivity.

### Screen 2: Investigation view (the core screen)

- **Top:** plain-English summary, e.g. "A user logged in from a new country and created an access key."
- **Left:** timeline of events with timestamps.
- **Right, "Context" cards:** is the resource public, what permissions the role has, whether it touches sensitive data, and related alerts.
- **Bottom:** "Similar past alerts: closed as false positive 4 times, reason: scheduled backup job."
- **Annotation:** every claim links to raw evidence, so the engineer can verify it.

### Screen 3: Resolve and record

- Three paths: **Fix it** (guided steps or a Jira ticket to the owner), **Not a risk** (suppress with a required reason and expiry date), **Escalate** (to an incident).
- Fix shows a preview: "This will remove public access from bucket X. Impact: none detected."
- A final note field, and an auto-generated audit trail.
- **Annotation:** requiring a reason makes triage decisions reusable and audit-ready.

## 4. Features and prioritization

| Priority | Feature | Why |
|---|---|---|
| Must have | Risk-based queue with context line | Fixes "where do I start?" |
| Must have | Investigation view with timeline and context | Core value, removes tool-switching |
| Must have | Resolve with required reason and audit log | Closes the loop, supports compliance |
| Should have | Grouping of duplicate alerts | Reduces volume quickly |
| Should have | Similar past alerts | Speeds up false-positive decisions |
| Should have | Jira and Slack handoff | Fits existing workflow |
| Could have | AI-written summary | Nice, but only after trust in the basics |
| Won't have (v1) | Auto-remediation by default | Risky; wrong fixes can break production |

**Reasoning:** trust first, then speed, then automation.

## 5. Success metrics

- **North star:** median time-to-triage (alert created to decision made).
- **Supporting:** time-to-resolve, % of alerts closed with a reason, false-positive rate, reopen rate (quality check), alerts handled per engineer per week.
- **Adoption:** weekly active security engineers, and % of alerts triaged inside the tool.
- **Guardrail:** missed true positives, so we don't get faster by being careless.

Targets: baseline to be measured first, then aim for improvement.

## 6. Development action items

1. Confirm alert sources for v1 (e.g. GuardDuty and CloudTrail first).
2. Define the risk score formula with security SMEs. Start simple and make it explainable.
3. Build a context service for resource exposure, role permissions and data sensitivity tags.
4. Define the duplicate-grouping rule (same resource plus same alert type within a time window).
5. Remediation permissions: least-privilege access, an approval step, a preview and a rollback plan.
6. Design the audit log and how long to retain it.
7. Jira and Slack integration scope.
8. Check timeline query performance on large log volumes.

**Questions to raise with devs:** How fresh does the data need to be? What if context data is missing? Who can approve a fix?
