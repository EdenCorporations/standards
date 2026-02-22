# Incident Response

This document defines EdenCORP's incident response process, severity classifications, communication protocols, and postmortem requirements.

---

## Severity Levels

| Severity | Definition | Examples |
|---|---|---|
| **P0 — Critical** | Complete outage or data loss affecting production. Revenue impact or customer data at risk. | Production database unreachable; authentication service down; security breach |
| **P1 — High** | Major functionality degraded for a significant portion of users. No workaround available. | Payment processing failing; AI pipeline producing errors for > 20% of requests |
| **P2 — Medium** | Partial degradation or reduced functionality. A workaround exists. | Slower-than-normal API responses; non-critical feature unavailable |
| **P3 — Low** | Minor issue with minimal user impact. Can be addressed in normal sprint cycle. | Cosmetic UI bug; logging gap; non-critical monitoring alert |

---

## Response Expectations

| Severity | Acknowledge | Initial response | Resolution target |
|---|---|---|---|
| P0 | 15 minutes | 30 minutes | 4 hours |
| P1 | 30 minutes | 1 hour | 8 hours |
| P2 | 2 hours | 4 hours | 3 business days |
| P3 | 1 business day | 3 business days | Next sprint |

**Acknowledge** = Incident declared, severity assigned, on-call engineer engaged.
**Initial response** = Incident commander assigned, communication channel open, first status update posted.
**Resolution target** = Service restored to normal operation (not necessarily root cause resolved).

---

## Incident Response Process

### Step 1: Detect and Declare

Incidents are detected via:
- PagerDuty alert from automated monitoring.
- Customer report escalated by support.
- Engineer observing anomalous behaviour.

Any engineer can declare an incident. When in doubt, declare and downgrade later. Use the Slack command `/incident declare` in `#incidents` to open an incident.

### Step 2: Assign the Incident Commander

The on-call engineer becomes the **Incident Commander (IC)** by default. The IC:

- Coordinates the response team.
- Sends status updates every 30 minutes (P0/P1) or every 2 hours (P2).
- Makes the decision to escalate, roll back, or invoke additional resources.
- Closes the incident when service is restored.

### Step 3: Assess and Mitigate

The response team works in the dedicated incident Slack channel (`#inc-YYYYMMDD-short-description`).

Priority order:
1. **Protect data integrity.** If data is at risk, stop the bleeding first.
2. **Restore service.** Roll back, failover, or apply an emergency fix.
3. **Communicate.** Keep stakeholders informed throughout.
4. **Investigate root cause.** This happens after service is restored, not during.

### Step 4: Resolve

The IC declares the incident resolved when:
- Service metrics return to normal baselines.
- The immediate risk to data and availability is eliminated.
- A post-incident monitoring period (30 minutes for P0/P1) has elapsed without recurrence.

### Step 5: Postmortem

See the Postmortem section below.

---

## Communication Protocol

### Internal Communication

- All incident communication occurs in the dedicated incident Slack channel.
- Status updates are posted on a defined cadence (every 30 minutes for P0/P1).
- The IC pings `@here` in `#incidents` for major status changes.
- `@EdenCorporations/leadership` is notified immediately for all P0 incidents and within 1 hour for P1 incidents.

### External Communication (Customer-Facing)

- P0 incidents receive a public status page update within 30 minutes.
- P1 incidents receive a status page update within 1 hour.
- Status page updates are written by the IC or a designated communications owner.
- Customer communications use factual, measured language. Do not speculate on root cause in external communications.

Status page template:
```
[Investigating] We are investigating reports of [brief description].
Our team is actively working on this. Next update in 30 minutes.

[Identified] We have identified the root cause of [brief description]
and are working on a fix.

[Resolved] The issue affecting [brief description] has been resolved.
We will publish a full postmortem within 5 business days.
```

---

## Postmortem Requirements

A postmortem is required for all P0 and P1 incidents. P2 incidents require a postmortem if they recur more than twice in 30 days.

### Timeline

- Draft postmortem due within **2 business days** of incident resolution.
- Postmortem review meeting within **5 business days**.
- Action items assigned and tracked in Jira within **5 business days**.

### Postmortem Principles

- **Blameless.** The postmortem process exists to learn, not to assign fault. Systems fail; people make decisions with the information they had at the time.
- **Focus on systemic causes.** Ask "why did the system allow this to happen?" not "who made the mistake?".
- **Every postmortem produces action items.** Postmortems without follow-through are wasted effort.

---

## Root Cause Analysis Template

```markdown
# Postmortem: [Short Incident Title]

**Incident ID:** INC-YYYYMMDD-001
**Severity:** P0 / P1 / P2
**Date:** YYYY-MM-DD
**Duration:** X hours Y minutes
**Incident Commander:** @username
**Authors:** @username, @username

---

## Summary

One paragraph describing what happened, what the impact was,
and what the resolution was.

---

## Timeline

| Time (UTC) | Event |
|---|---|
| 14:02 | PagerDuty alert fires: error rate > 5% on payments service |
| 14:07 | On-call engineer acknowledges alert |
| 14:10 | Incident declared. IC assigned. #inc-20251015-payments opened |
| 14:25 | Root cause identified: bad deploy introduced unhandled promise rejection |
| 14:30 | Rollback initiated |
| 14:38 | Service restored. Error rate back to baseline |
| 14:42 | Monitoring period begins |
| 15:12 | Incident resolved |

---

## Impact

- **Users affected:** ~1,200 users unable to complete payments
- **Revenue impact:** Estimated $X,XXX in failed transactions
- **Duration:** 40 minutes

---

## Root Cause

Describe the technical root cause in detail.

---

## Contributing Factors

- Factor 1: Missing integration test for this code path
- Factor 2: Alerting threshold too high — delayed detection by ~5 minutes
- Factor 3: ...

---

## What Went Well

- Rollback executed within 8 minutes of identification
- Communication cadence maintained throughout

---

## What Went Poorly

- Detection was delayed due to alert threshold misconfiguration
- Runbook for this failure mode was missing

---

## Action Items

| Action | Owner | Due date | Ticket |
|---|---|---|---|
| Lower error rate alert threshold to 1% | @username | YYYY-MM-DD | ENG-XXX |
| Add integration test for payment promise rejection | @username | YYYY-MM-DD | ENG-XXX |
| Write runbook for payments service rollback | @username | YYYY-MM-DD | ENG-XXX |
```

All postmortems are stored in the `docs/postmortems/` directory of the relevant service repository and linked from the incident tracking system.
