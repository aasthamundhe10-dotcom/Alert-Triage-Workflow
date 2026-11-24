# Presentation Script (about 3 minutes)

Matches the slides in `Alert_Triage_Workflow.pptx`.

## Slide 1: Title (0:00-0:15)
Hi, I'm Aastha Mundhe. For this assignment I chose Option 2, the Alert Triage Workflow. I picked it because it is a real workflow problem, so I can show how I think about users, priorities and metrics.

## Slide 2: The problem (0:15-0:40)
Cloud security tools raise hundreds of alerts every day. For each alert, an engineer has to open four places: the cloud console, the logs, the IAM page and Jira. They are trying to answer three simple questions: Is this real? How bad is it? What do I do? This is slow and tiring, and real threats can get missed. My goal is to take an alert from "fired" to "resolved with a recorded reason" on one screen.

## Slide 3: Meet Rahul (0:40-1:00)
My user is Rahul, a cloud security engineer on a small team of three. He is an assumed persona, not based on interviews. His pains are too many alerts, no clear priority, and checking the same false alarms again and again. He needs fast triage, evidence he can trust, and an audit trail for compliance like SOC 2.

## Slide 4: Screen 1, Alert queue (1:00-1:20)
Screen one is the alert queue. It is sorted by risk score, not just severity. Risk combines severity, how exposed the resource is, and how sensitive the data is. Each alert has a one-line "why it matters", and duplicate alerts are grouped into one row.

## Slide 5: Screen 2, Investigation (1:20-1:50)
Screen two is the core of the design. At the top is a plain-English summary. Below it are a timeline of events and context cards, such as whether the resource is public, what the role can access, and whether it holds sensitive data. At the bottom, it shows similar past alerts, for example "closed as false positive 4 times". Every claim links to raw evidence, so the engineer can verify it.

## Slide 6: Screen 3, Resolve and record (1:50-2:10)
Screen three is where the engineer decides. There are three paths: fix it, mark it as not a risk, or escalate. Fixes show an impact preview first. Marking "not a risk" needs a reason and an expiry date. Every decision goes into an audit trail.

## Slide 7: What we build first (2:10-2:30)
The must-haves are the risk-based queue, the investigation view, and resolve with an audit log. The should-haves are duplicate grouping, similar past alerts, and Jira and Slack handoff. I left auto-remediation out of version one, because a wrong automatic fix can break production. My principle is trust first, then speed, then automation.

## Slide 8: How we measure success (2:30-2:48)
My north star metric is median time-to-triage, the time from alert created to decision made. Supporting metrics are time-to-resolve, percent of alerts closed with a reason, false-positive rate and reopen rate. The guardrail is missed true positives, so we never get faster by being careless. I would measure a baseline first before setting targets.

## Slide 9: For the dev team (2:48-3:00)
For the dev team, I would start with four things: the risk score formula, a context service, the duplicate-grouping rule, and safe remediation with approval and rollback. Thank you for watching.
