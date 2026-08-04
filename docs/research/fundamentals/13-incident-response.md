# Incident Response

> **Status:** Draft
>
> **Category:** Reliability Engineering
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-02

---

# Introduction

No production system is immune to failure.

Even well-designed, thoroughly tested, and highly available systems will eventually experience incidents.

Hardware fails.

Software contains defects.

Dependencies become unavailable.

Unexpected traffic patterns emerge.

The goal of Site Reliability Engineering is therefore not to eliminate incidents entirely.

The goal is to detect incidents quickly, minimize customer impact, restore service efficiently, and continuously improve the system afterward.

Incident Response provides the structured process that enables engineering teams to achieve these objectives.

---

# Why Incident Response Matters

Production incidents are often time-sensitive.

Without a well-defined response process, engineering teams may experience:

- Delayed detection
- Confused communication
- Duplicate troubleshooting efforts
- Incorrect prioritization
- Longer outages
- Increased customer impact

A structured Incident Response process helps teams remain organized under pressure and make decisions based on evidence rather than assumptions.

Its primary objective is to reduce both business impact and recovery time.

---

# What Is an Incident?

An incident is any unplanned event that negatively affects the availability, reliability, performance, or security of a production service.

Not every technical issue is an incident.

An incident is declared when the issue has a measurable impact on users or business operations.

Examples include:

- Service outage
- Significant increase in latency
- Elevated error rates
- Failed production deployment
- Database unavailability
- Loss of network connectivity
- Authentication failures
- Data replication failures

An incident may affect a single service or an entire platform.

The severity depends on the scope and impact rather than the technical complexity.

---

# Incident vs Problem

These terms are often confused.

They represent different concepts.

| Incident | Problem |
|----------|----------|
| Immediate operational disruption | Underlying root cause |
| Requires rapid response | Requires long-term resolution |
| Focuses on restoring service | Focuses on preventing recurrence |
| Time-sensitive | Investigation-focused |

For example:

```
Database becomes unavailable.
        │
        ▼
Incident Declared
        │
        ▼
Traffic redirected to replica.
        │
        ▼
Service Restored
        │
        ▼
Root Cause Investigation
        │
        ▼
Problem Identified
```

Restoring service resolves the incident.

Identifying and eliminating the underlying cause resolves the problem.

---

# Incident Severity Levels

Organizations typically classify incidents according to their business impact.

Although naming conventions differ, a common severity model is:

| Severity | Typical Impact |
|----------|----------------|
| SEV-1 | Critical outage affecting most users or core business operations |
| SEV-2 | Major degradation affecting a significant portion of users |
| SEV-3 | Limited service degradation with available workarounds |
| SEV-4 | Minor issue with minimal customer impact |

Severity should reflect customer and business impact rather than technical complexity.

A technically simple issue that prevents customers from completing payments may deserve a higher severity than a complex infrastructure issue with no user impact.

---

# Incident Response Goals

Every incident response process should pursue four primary objectives:

1. Detect the incident as quickly as possible.
2. Reduce customer impact.
3. Restore normal service safely.
4. Learn from the incident to reduce future risk.

These objectives establish the foundation for every subsequent stage of the incident response lifecycle.

---

# Incident Response Lifecycle

Although every organization has its own operational procedures, most incident response processes follow the same high-level lifecycle.

```
Detect
   │
   ▼
Assess
   │
   ▼
Declare
   │
   ▼
Mitigate
   │
   ▼
Recover
   │
   ▼
Review
```

Each stage has a different objective.

Skipping a stage often increases recovery time or introduces additional operational risk.

---

# Detection

Incident response begins when abnormal system behavior is detected.

Detection may originate from:

- Monitoring alerts
- Automated health checks
- Customer reports
- Internal engineering teams
- Synthetic monitoring
- On-call engineers

The objective is to identify production issues as early as possible.

Early detection reduces customer impact and shortens recovery time.

---

# Assessment

Once an issue has been detected, engineers evaluate:

- Which services are affected?
- How many users are impacted?
- Is the issue still ongoing?
- What is the business impact?
- What severity should be assigned?

Assessment should prioritize facts over assumptions.

Avoid beginning deep troubleshooting before understanding the overall scope of the incident.

---

# Incident Declaration

If the issue meets the organization's incident criteria, an incident should be declared.

Declaring an incident typically triggers:

- Incident communication channels
- Assignment of response roles
- Stakeholder notification
- Incident tracking
- Coordination procedures

Delaying declaration often delays recovery because engineers continue working independently rather than as a coordinated team.

---

# Mitigation

Mitigation focuses on reducing customer impact as quickly as possible.

The objective is **service restoration**, not identifying the root cause.

Typical mitigation actions include:

- Rolling back a deployment
- Restarting failed services
- Redirecting traffic
- Scaling infrastructure
- Failing over to replicas
- Disabling problematic features
- Activating disaster recovery procedures

Temporary solutions are acceptable if they safely restore service.

Root cause analysis comes later.

---

# Recovery

Recovery begins after mitigation has stabilized the service.

Engineers verify that:

- The service is functioning normally.
- Error rates have returned to expected levels.
- Latency is stable.
- Customer impact has ended.
- Monitoring alerts have cleared.

Recovery should always be validated using telemetry rather than assumptions.

---

# Review

The final stage occurs after the incident has been resolved.

The review focuses on learning rather than assigning blame.

Typical questions include:

- What happened?
- Why did it happen?
- How was it detected?
- What worked well?
- What delayed recovery?
- How can similar incidents be prevented?

This stage feeds directly into the Postmortem process discussed in the next document.

---

# Incident Response Roles

Clear responsibilities reduce confusion during high-pressure situations.

Although role names vary between organizations, most incident response teams include the following responsibilities.

## Incident Commander

The Incident Commander coordinates the overall response.

Responsibilities include:

- Prioritizing actions
- Coordinating responders
- Making operational decisions
- Managing timelines
- Keeping the investigation focused

The Incident Commander should coordinate rather than perform every technical task.

---

## Operations Responders

Operations responders investigate and mitigate the technical issue.

Typical activities include:

- Analyzing metrics
- Reviewing logs
- Investigating traces
- Restarting services
- Deploying fixes
- Executing recovery procedures

Their primary objective is restoring service safely.

---

## Communications Lead

Large incidents often require a dedicated communications role.

Responsibilities include:

- Updating stakeholders
- Informing customer support teams
- Publishing status updates
- Maintaining incident timelines

Separating communication from technical troubleshooting allows responders to remain focused on recovery.

---

# Detection and Escalation

Early detection alone is not sufficient.

An effective incident response process also ensures that incidents reach the appropriate responders quickly.

Escalation should follow predefined procedures rather than depending on personal judgment.

Typical escalation paths include:

- On-call engineer
- Service owner
- Platform engineering
- Site Reliability Engineering (SRE)
- Database administrators
- Security team (if applicable)

The objective is to involve the right people at the right time while avoiding unnecessary escalation.

---

# Mitigation vs Root Cause Analysis

One of the most common mistakes during an incident is attempting to identify the root cause before restoring service.

These activities have different objectives.

| Mitigation | Root Cause Analysis |
|------------|---------------------|
| Restore service | Explain why the incident occurred |
| Reduce customer impact | Prevent recurrence |
| Time-critical | Can continue after recovery |
| Accepts temporary solutions | Requires complete investigation |

A typical sequence is:

```
Incident Detected
        │
        ▼
Mitigate Customer Impact
        │
        ▼
Restore Service
        │
        ▼
Perform Root Cause Analysis
```

Production stability should take priority over immediate explanation.

---

# Communication During Incidents

Clear communication is as important as technical troubleshooting.

During an active incident, stakeholders need accurate and timely updates.

Typical communication includes:

- Current incident status
- Affected services
- Customer impact
- Mitigation progress
- Estimated next update

Avoid speculation.

Communicate confirmed facts and clearly identify any assumptions.

Consistent communication builds trust even when the incident has not yet been resolved.

---

# Example Incident Timeline

The following simplified timeline illustrates a typical production incident.

```
02:13
Monitoring detects elevated latency.

        │

02:15
On-call engineer acknowledges the alert.

        │

02:18
Incident declared (SEV-2).

        │

02:24
Traffic redirected to healthy instances.

        │

02:31
Latency returns to normal.

        │

02:36
Incident resolved.

        │

Next Business Day

Postmortem begins.
```

Structured timelines help teams reconstruct events accurately during post-incident reviews.

---

# Incident Response and Observability

Every observability capability introduced in previous documents contributes to incident response.

```
Alert
   │
   ▼
Golden Signals
   │
   ▼
Metrics
   │
   ▼
Traces
   │
   ▼
Logs
   │
   ▼
Mitigation
   │
   ▼
Recovery
```

Observability shortens investigation time by helping engineers move quickly from symptom to evidence.

Without reliable telemetry, incident response becomes slower and increasingly dependent on manual investigation.

---

# Characteristics of an Effective Incident Response Process

High-performing engineering organizations typically demonstrate the following characteristics:

- Clearly defined response procedures.
- Well-understood incident roles.
- Reliable monitoring and alerting.
- Fast communication.
- Evidence-based decision making.
- Continuous operational improvement.

The objective is not merely to resolve incidents.

The objective is to improve the organization's ability to respond to future incidents more effectively.

---

# Production Considerations

An effective incident response process is built before an incident occurs.

Preparation, automation, and clearly defined responsibilities significantly reduce recovery time when production issues arise.

---

## Define Runbooks

Frequently occurring incidents should have documented runbooks.

A runbook should include:

- Detection criteria
- Initial investigation steps
- Common mitigation actions
- Verification procedures
- Escalation contacts

Runbooks improve consistency and reduce decision-making time during high-pressure situations.

---

## Regularly Practice Incident Response

Incident response is an operational skill.

Engineering teams should periodically conduct exercises such as:

- Game Days
- Disaster Recovery drills
- Failure injection
- Tabletop exercises

Regular practice improves coordination, confidence, and response speed.

---

## Continuously Improve Monitoring

Every major incident should lead to improvements in observability.

Examples include:

- Better alerts
- Additional telemetry
- Improved dashboards
- Enhanced runbooks
- Clearer escalation procedures

Each incident should strengthen the platform's future resilience.

---

## Measure Incident Response Performance

Engineering organizations commonly track:

- Mean Time to Detect (MTTD)
- Mean Time to Acknowledge (MTTA)
- Mean Time to Resolve (MTTR)
- Incident frequency
- Recurring incident rate

These indicators help evaluate and improve the effectiveness of the incident response process.

---

# Common Anti-patterns

## Skipping Incident Declaration

Waiting too long to declare an incident delays coordination and often increases customer impact.

Declare incidents early when predefined criteria are met.

---

## Investigating Before Mitigating

Engineers sometimes spend excessive time searching for the exact root cause before restoring service.

Customer impact should be reduced first.

Root cause analysis can continue after recovery.

---

## Poor Communication

Technical progress alone is not enough.

Without timely updates, stakeholders lose visibility and confidence.

Maintain clear, factual, and regular communication throughout the incident.

---

## Blaming Individuals

Production incidents are usually the result of multiple contributing factors.

Blame discourages learning and reduces psychological safety.

The objective is to improve systems and processes, not assign fault.

---

# Best Practices

- Define incident severity levels before they are needed.
- Assign clear response roles.
- Detect incidents through reliable monitoring and alerting.
- Prioritize mitigation over root cause analysis during active incidents.
- Maintain accurate and regular communication.
- Validate recovery using telemetry.
- Conduct post-incident reviews and improve operational processes continuously.

---

# Key Takeaways

- Incidents are inevitable in production systems.
- A structured response process reduces customer impact and recovery time.
- Incident response prioritizes service restoration; root cause analysis follows recovery.
- Clearly defined roles improve coordination under pressure.
- Effective communication is essential throughout the incident lifecycle.
- Every incident should improve future operational readiness.

---

# Looking Ahead

Recovering from an incident is only part of the engineering process.

The next document introduces **Postmortems**, explaining how engineering teams analyze incidents, identify contributing factors, and implement long-term improvements without assigning blame.

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
- PagerDuty Incident Response Documentation
- Atlassian Incident Management Handbook
