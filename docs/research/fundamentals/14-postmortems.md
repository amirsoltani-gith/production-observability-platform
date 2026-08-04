# Postmortems

> **Status:** Draft
>
> **Category:** Reliability Engineering
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-02

---

# Introduction

Restoring a service after an incident is only part of operating a reliable production platform.

The greater opportunity lies in understanding why the incident occurred and improving the system so that similar failures become less likely in the future.

This is the purpose of a postmortem.

A postmortem is a structured engineering review conducted after an incident has been resolved.

Its goal is not to assign blame.

Its goal is to improve systems, processes, documentation, automation, and operational practices through evidence-based learning.

---

# Why Postmortems Matter

Every production incident contains valuable operational knowledge.

If that knowledge is not captured, organizations risk repeating the same failures.

Effective postmortems help engineering teams:

- Understand what happened.
- Identify contributing factors.
- Improve operational procedures.
- Strengthen monitoring and alerting.
- Reduce future incident frequency.
- Share knowledge across teams.

A mature engineering organization treats every significant incident as an opportunity to improve.

---

# What Is a Postmortem?

A postmortem is a documented analysis of a completed production incident.

It answers questions such as:

- What happened?
- When did it happen?
- How was it detected?
- How did the team respond?
- What caused the incident?
- What factors increased its impact?
- What improvements should be implemented?

A postmortem should describe events objectively using evidence gathered during the incident.

Its purpose is learning—not justification.

---

# Blameless Culture

One of the defining characteristics of modern Site Reliability Engineering is the use of **blameless postmortems**.

The objective is to improve systems rather than identify individuals to blame.

Human mistakes are rarely the sole cause of production incidents.

Incidents usually result from multiple contributing factors, including:

- Incomplete automation
- Weak monitoring
- Poor documentation
- Process gaps
- Architectural limitations
- Unexpected interactions between components

Blameless postmortems encourage engineers to report issues openly, leading to more accurate investigations and more effective long-term improvements.

---

# When Should a Postmortem Be Written?

Organizations define different policies, but postmortems are commonly created for:

- SEV-1 incidents
- SEV-2 incidents
- Customer-impacting outages
- Security incidents
- Major deployment failures
- Significant reliability regressions

Minor operational issues may only require brief incident notes rather than a full postmortem.

The effort invested should be proportional to the incident's impact.

---

# Goals of a Good Postmortem

A high-quality postmortem should achieve four primary objectives:

1. Build a shared understanding of the incident.
2. Identify both technical and process-related improvements.
3. Define clear follow-up actions.
4. Reduce the likelihood and impact of similar incidents.

A successful postmortem is measured not by the quality of the document itself, but by the improvements that result from it.

---

# Incident Timeline

A postmortem should include a factual timeline of the incident.

The timeline provides a shared understanding of what occurred and when each significant event happened.

A typical timeline includes:

```
02:13
Monitoring detects increased latency.

        │

02:15
On-call engineer acknowledges the alert.

        │

02:18
Incident declared (SEV-2).

        │

02:25
Traffic redirected to healthy instances.

        │

02:31
Service performance returns to normal.

        │

02:40
Incident closed.

        │

Next Business Day

Postmortem begins.
```

Timelines should contain verified events rather than assumptions.

Accurate timelines help identify delays, unnecessary actions, and opportunities for process improvement.

---

# Root Cause Analysis

After documenting what happened, the next objective is understanding why it happened.

Root Cause Analysis (RCA) seeks to identify the underlying conditions that allowed the incident to occur.

Typical questions include:

- Why did the failure occur?
- Why was it not detected earlier?
- Why did the impact become significant?
- Why did existing safeguards fail?

Root cause analysis should focus on improving systems rather than assigning responsibility.

---

# Contributing Factors

Most production incidents do not have a single cause.

Instead, multiple factors combine to produce customer impact.

Examples include:

- Incomplete monitoring
- Insufficient testing
- Configuration errors
- Capacity limitations
- Deployment timing
- External dependency failures
- Missing operational documentation
- Human error combined with weak safeguards

Identifying contributing factors provides a more complete understanding than searching for one "root cause."

---

# Corrective vs Preventive Actions

A useful postmortem distinguishes between corrective and preventive actions.

| Corrective Action | Preventive Action |
|-------------------|-------------------|
| Restores the affected service | Reduces the likelihood of recurrence |
| Immediate priority | Long-term improvement |
| Addresses current impact | Improves future reliability |

Examples:

**Corrective Actions**

- Roll back the deployment.
- Restore database replication.
- Restart failed services.

**Preventive Actions**

- Add automated testing.
- Improve deployment validation.
- Create monitoring alerts.
- Update runbooks.
- Increase redundancy.

Both types of actions are important, but preventive actions deliver the greatest long-term value.

---

# Writing an Effective Postmortem

An effective postmortem should be:

- Objective
- Evidence-based
- Clear
- Actionable
- Easy to review later

A typical structure includes:

1. Incident summary
2. Customer impact
3. Timeline
4. Root cause
5. Contributing factors
6. Corrective actions
7. Preventive actions
8. Lessons learned
9. Action items with owners and due dates

The document should help future engineers understand the incident without requiring prior knowledge.

---

# Lessons Learned

Every postmortem should conclude with clear lessons learned.

Lessons should describe improvements that strengthen the system rather than simply summarize the incident.

Examples include:

- Monitoring did not detect the failure early enough.
- Deployment validation was incomplete.
- Documentation was outdated.
- Escalation procedures were unclear.
- Recovery depended too heavily on manual intervention.

The objective is to improve engineering practices continuously.

---

# Action Items

A postmortem without follow-up actions provides limited value.

Every identified improvement should become a concrete action item.

Each action should include:

- Description
- Priority
- Owner
- Due date
- Current status

Example:

| Action | Owner | Priority | Status |
|---------|-------|----------|--------|
| Add database replication alert | Platform Team | High | Open |
| Improve deployment validation | DevOps Team | Medium | In Progress |
| Update incident runbook | SRE Team | Medium | Open |

Tracking action items ensures that lessons learned become measurable improvements.

---

# Characteristics of High-Quality Postmortems

Effective postmortems share several characteristics.

They are:

- Blameless
- Evidence-based
- Chronological
- Action-oriented
- Easy to understand
- Focused on system improvement

Their value comes from improving future reliability rather than documenting past failures.

---

# Relationship with Incident Response

Incident Response and Postmortems represent two consecutive stages of the same engineering process.

```
Incident Detected
        │
        ▼
Incident Response
        │
        ▼
Service Restored
        │
        ▼
Postmortem
        │
        ▼
Action Items
        │
        ▼
System Improvement
```

Incident Response minimizes immediate customer impact.

Postmortems reduce the likelihood and impact of future incidents.

Together, they create a continuous improvement cycle.

---

# Continuous Improvement

Mature engineering organizations treat every incident as an opportunity to strengthen the platform.

The continuous improvement cycle typically follows this pattern.

```
Incident
    │
    ▼
Postmortem
    │
    ▼
Engineering Improvements
    │
    ▼
Better Monitoring
    │
    ▼
Improved Reliability
    │
    ▼
Future Incident
        │
   Lower Impact
```

Reliability is not achieved by avoiding incidents.

It is achieved by learning from every incident and systematically improving the platform.

---

# Production Considerations

A postmortem should become part of the engineering workflow rather than a one-time documentation exercise.

Its real value comes from the improvements implemented afterward.

---

## Conduct Postmortems Promptly

Postmortems should be written while information is still fresh.

Waiting several weeks often leads to:

- Incomplete timelines
- Forgotten details
- Missing evidence
- Reduced participation

Aim to complete the initial postmortem within a few business days after the incident.

---

## Base Conclusions on Evidence

Every conclusion should be supported by operational evidence.

Possible evidence includes:

- Metrics
- Logs
- Distributed traces
- Deployment history
- Monitoring alerts
- Incident timeline

Avoid speculation whenever possible.

When uncertainty exists, clearly state what is known and what remains unconfirmed.

---

## Prioritize Action Items

Not every improvement has the same impact.

Action items should be prioritized according to:

- Customer impact
- Risk reduction
- Implementation effort
- Likelihood of recurrence

High-impact improvements should be addressed first.

---

## Verify Improvements

Completing a postmortem does not reduce operational risk by itself.

Risk is reduced only when agreed improvements are implemented and validated.

Engineering teams should periodically review outstanding action items until they are complete.

---

# Common Anti-patterns

## Treating the Postmortem as Documentation Only

A document without follow-up actions produces little operational value.

The goal is improving the system, not writing reports.

---

## Assigning Blame

Blame discourages transparency.

Engineers become less willing to report mistakes, reducing organizational learning.

Focus on improving systems and processes instead of individuals.

---

## Looking for a Single Root Cause

Complex production incidents often result from multiple contributing factors.

Avoid oversimplifying investigations.

Document both the primary cause and the contributing conditions.

---

## Ignoring Follow-up Actions

The same incident is likely to recur if corrective and preventive actions are never implemented.

Review action items regularly until they are completed.

---

# Best Practices

- Write postmortems soon after the incident.
- Maintain a blameless culture.
- Build timelines using verified evidence.
- Identify both root causes and contributing factors.
- Define clear corrective and preventive actions.
- Assign owners and deadlines to every action item.
- Review completed actions to verify measurable improvements.

---

# Key Takeaways

- Postmortems transform incidents into organizational learning.
- Their objective is system improvement rather than assigning blame.
- High-quality postmortems combine accurate timelines, evidence, and actionable follow-up tasks.
- Corrective actions restore reliability; preventive actions reduce future risk.
- Continuous improvement depends on implementing and tracking action items.

---

# Looking Ahead

Responding effectively to incidents is important.

Preventing unnecessary incidents is even better.

The next document explores **Alert Fatigue and Alert Design**, explaining how engineering teams build actionable alerts, reduce operational noise, and ensure responders are notified only when meaningful action is required.

---

# References

## Official Documentation

- Google Site Reliability Engineering
- The Site Reliability Workbook

## Industry Resources

- Google Cloud Architecture Center
- PagerDuty Incident Response Guide

## Further Reading

- Seeking SRE – David N. Blank-Edelman
- PagerDuty Incident Management Documentation
- Atlassian Incident Management Handbook
