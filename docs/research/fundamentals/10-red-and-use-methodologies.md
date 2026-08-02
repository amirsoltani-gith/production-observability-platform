# RED and USE Methodologies

> **Status:** Draft
>
> **Category:** Reliability Engineering
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-02

---

# Introduction

Monitoring modern production systems requires more than collecting telemetry.

Engineering teams need practical frameworks that help them identify which metrics should be monitored for different types of systems.

The RED and USE methodologies provide two complementary approaches.

Rather than competing with one another, they focus on different layers of a production platform.

- RED focuses on services and applications.
- USE focuses on infrastructure and system resources.

Together, they provide a comprehensive monitoring strategy that complements the Four Golden Signals.

---

# Why Monitoring Methodologies Matter

Production environments generate thousands of metrics.

Without a structured monitoring model, engineering teams often face several problems:

- Monitoring too many low-value metrics.
- Missing critical indicators.
- Building inconsistent dashboards.
- Delayed incident detection.
- Longer root cause investigations.

Monitoring methodologies reduce this complexity by defining a small set of high-value metrics.

Instead of asking:

> "What should we monitor?"

They answer:

> "Which metrics best represent the health of this system?"

---

# Two Complementary Perspectives

The RED and USE methodologies observe systems from different viewpoints.

```
                Production Platform

          ┌──────────────┴──────────────┐

          ▼                             ▼

   Application Layer             Infrastructure Layer

          │                             │

          ▼                             ▼

        RED                           USE
```

RED helps engineers understand whether services are functioning correctly.

USE helps engineers understand whether infrastructure resources are becoming bottlenecks.

Using both methodologies provides visibility across the complete production stack.

---

# The RED Method

The RED Method was introduced by Tom Wilkie and became widely adopted within the Prometheus and Grafana communities.

It focuses on service-level monitoring.

RED consists of three metrics:

- Request Rate
- Error Rate
- Duration

These three indicators provide a concise view of application health.

---

# Request Rate

Request Rate measures how much work a service is performing.

Typical measurements include:

- Requests per second (RPS)
- Transactions per second (TPS)
- API calls per minute
- Queue messages processed

Request Rate represents demand placed on the service.

Understanding workload is essential when interpreting latency or error trends.

---

# Error Rate

Error Rate measures the proportion of requests that fail.

Examples include:

- HTTP 5xx responses
- Failed database operations
- Timeout responses
- Internal application exceptions

A rising error rate often indicates degraded service quality and should trigger operational investigation.

---

# Duration

Duration measures how long requests take to complete.

Unlike infrastructure metrics, Duration reflects service responsiveness from the user's perspective.

Production dashboards commonly monitor:

- Average latency
- P95 latency
- P99 latency

Percentile-based measurements provide a more accurate representation of user experience than simple averages.

The RED Method intentionally remains simple, allowing engineering teams to evaluate application health using only three core indicators.

---

# The USE Method

The USE Method was developed by Brendan Gregg as a practical framework for identifying infrastructure bottlenecks.

Unlike the RED Method, which focuses on application behavior, the USE Method evaluates the health of system resources.

USE stands for:

- Utilization
- Saturation
- Errors

It can be applied to almost any infrastructure resource, including CPUs, memory, disks, network interfaces, and storage systems.

---

# Utilization

Utilization measures how busy a resource is during a given period.

It answers the question:

> **How much of the available resource is currently being used?**

Examples include:

- CPU utilization
- Network bandwidth usage
- Disk throughput
- GPU utilization

High utilization is not necessarily a problem.

A CPU running at 80% utilization during peak traffic may be operating exactly as designed.

Utilization becomes meaningful when interpreted together with saturation and errors.

---

# Saturation

Saturation measures how much additional work is waiting because the resource has reached or is approaching its capacity.

It answers the question:

> **Is work waiting for this resource?**

Examples include:

- CPU run queue length
- Database connection queue
- Disk I/O queue depth
- Pending network packets
- Kubernetes Pod scheduling delays

Unlike utilization, saturation often reveals performance problems before users experience failures.

It is one of the earliest indicators of resource bottlenecks.

---

# Errors

Errors measure whether a resource is failing to perform its intended function.

Examples include:

- Disk read/write errors
- Network packet errors
- Memory ECC errors
- Filesystem failures
- Hardware faults

Resource errors usually indicate underlying infrastructure problems rather than application defects.

Although less common than utilization or saturation issues, they often require immediate investigation.

---

# Applying USE to Infrastructure

One of the strengths of the USE Method is that it can be applied consistently across many resource types.

| Resource | Utilization | Saturation | Errors |
|----------|-------------|------------|---------|
| CPU | CPU Usage | Run Queue Length | Hardware Faults |
| Memory | Memory Usage | Memory Pressure | ECC Errors |
| Disk | Disk Utilization | I/O Queue Depth | Read/Write Errors |
| Network | Bandwidth Usage | Packet Queue | Packet Errors |

This consistent model allows engineers to troubleshoot infrastructure using the same mental framework regardless of the resource being analyzed.

---

# RED vs USE

Although both methodologies improve monitoring, they answer different engineering questions.

| RED | USE |
|------|-----|
| Service-oriented | Infrastructure-oriented |
| Measures user-facing behavior | Measures resource health |
| Request Rate | Utilization |
| Error Rate | Errors |
| Duration | Saturation |
| Application troubleshooting | Infrastructure troubleshooting |

Neither methodology replaces the other.

Instead, they complement one another by covering different layers of the production platform.

---

# RED, USE, and the Four Golden Signals

The RED Method, USE Method, and Four Golden Signals are closely related.

They should not be viewed as competing monitoring models.

Instead, each framework focuses on a different aspect of production operations.

```
                 Production Observability

        ┌──────────────┼──────────────┐

        ▼              ▼              ▼

 Golden Signals       RED            USE

  Service Health   Service Metrics   Resource Health
```

Together, these frameworks provide a complete operational view of both applications and infrastructure.

---

# Choosing the Right Methodology

The most appropriate methodology depends on what engineers are trying to investigate.

| Operational Question | Recommended Method |
|----------------------|--------------------|
| Is the API healthy? | RED |
| Is the database overloaded? | USE |
| Are users experiencing slow responses? | Golden Signals |
| Is CPU becoming a bottleneck? | USE |
| Is the payment service failing? | RED |
| Is the overall service healthy? | Golden Signals + RED + USE |

Rather than choosing one methodology, mature engineering teams combine them according to the problem they are solving.

---

# Example Production Investigation

Consider the following production incident.

```
Users report slow API responses.
```

A typical investigation might follow this sequence.

### Step 1 — Golden Signals

Engineers review service health.

```
Latency ↑
Traffic →
Errors ↑
Saturation →
```

The service is slower and requests are beginning to fail.

---

### Step 2 — RED Method

The application dashboard shows:

```
Request Rate →
Error Rate ↑
Duration ↑
```

The workload is stable, but requests are taking longer and more of them are failing.

This suggests the problem is within the service rather than caused by increased demand.

---

### Step 3 — USE Method

Infrastructure dashboards reveal:

```
CPU Utilization → 45%

Run Queue → Normal

CPU Errors → None

Disk Queue ↑

Disk Utilization → 95%
```

The investigation identifies the storage subsystem as the bottleneck.

Without combining RED and USE, engineers might have focused only on the application and overlooked the underlying infrastructure issue.

---

# Strengths and Limitations

## RED

### Strengths

- Simple to understand.
- Focuses on user-facing services.
- Excellent for dashboards.
- Supports rapid incident detection.

### Limitations

- Provides limited visibility into infrastructure bottlenecks.
- Cannot explain resource contention by itself.

---

## USE

### Strengths

- Excellent for infrastructure troubleshooting.
- Helps identify capacity bottlenecks.
- Supports capacity planning.
- Applies consistently across many resource types.

### Limitations

- Does not measure user experience directly.
- Infrastructure may appear healthy while the application is failing.

---

# Why Mature Teams Use Multiple Models

No single methodology provides complete operational visibility.

Production systems are multi-layered.

Applications depend on infrastructure, infrastructure supports applications, and users ultimately experience the combined behavior of both.

Modern engineering teams therefore combine:

- Golden Signals for overall service health.
- RED for application and service monitoring.
- USE for infrastructure and resource monitoring.

Each methodology reinforces the others, reducing investigation time and improving operational confidence.

---

# Production Considerations

The RED and USE methodologies should be viewed as complementary operational frameworks rather than independent monitoring solutions.

A production-ready monitoring strategy typically combines multiple methodologies to provide complete visibility across the application and infrastructure stack.

---

## Choose Metrics That Reflect Reality

Not every metric contributes equally to operational awareness.

Select metrics that:

- Represent actual system behavior.
- Help detect customer-impacting incidents.
- Support rapid root cause analysis.
- Remain stable over time.

Avoid collecting metrics simply because they are available.

---

## Build Layered Dashboards

Separate dashboards according to operational responsibility.

Examples include:

- Executive service health dashboards
- Application (RED) dashboards
- Infrastructure (USE) dashboards
- Capacity planning dashboards
- Incident investigation dashboards

Different audiences require different levels of operational detail.

---

## Combine with Observability

Neither RED nor USE explains *why* problems occur.

When abnormal behavior is detected:

```
RED / USE Alert
        │
        ▼
Metrics Investigation
        │
        ▼
Distributed Trace
        │
        ▼
Application Logs
        │
        ▼
Root Cause
```

Monitoring identifies the problem.

Observability explains the problem.

---

## Review Monitoring Strategy Regularly

Production systems evolve continuously.

As applications change, monitoring should also evolve.

Regularly review:

- Dashboard usefulness
- Alert quality
- Missing metrics
- Obsolete metrics
- Investigation workflows

Monitoring should improve alongside the platform it observes.

---

# Common Anti-patterns

## Treating RED as Infrastructure Monitoring

RED is designed for applications and services.

It should not replace infrastructure monitoring.

---

## Treating USE as Service Monitoring

USE evaluates resource health.

It cannot determine whether users are having a good experience.

---

## Building Separate Operational Silos

Application teams and infrastructure teams should not investigate incidents independently.

Shared dashboards and correlated telemetry improve collaboration and reduce investigation time.

---

## Monitoring Too Many Metrics

A dashboard containing hundreds of metrics is difficult to interpret during an incident.

Prioritize metrics that directly support operational decisions.

---

# Best Practices

- Use RED for application and service monitoring.
- Use USE for infrastructure and resource monitoring.
- Combine both with the Four Golden Signals.
- Correlate monitoring data with metrics, logs, and traces.
- Build dashboards around operational questions rather than available metrics.
- Review dashboards and alerts as systems evolve.
- Keep monitoring focused, actionable, and easy to interpret.

---

# Key Takeaways

- RED and USE solve different monitoring problems.
- RED focuses on service behavior.
- USE focuses on infrastructure resources.
- Neither methodology replaces the Four Golden Signals.
- Combining all three frameworks provides comprehensive operational visibility.
- Monitoring identifies problems, while observability enables engineers to understand and resolve them.

---

# Looking Ahead

This document introduced two practical monitoring methodologies used in production environments.

The next document explains how organizations define measurable reliability targets using:

- Service Level Indicators (SLIs)
- Service Level Objectives (SLOs)
- Service Level Agreements (SLAs)

These concepts transform monitoring data into measurable reliability goals that guide engineering and operational decisions.

---

# References

## Official Documentation

- Google Site Reliability Engineering
- The Site Reliability Workbook

## Industry Resources

- Brendan Gregg — Systems Performance
- Google Cloud Architecture Center
- Prometheus Documentation

## Further Reading

- Observability Engineering – Charity Majors, Liz Fong-Jones, George Miranda
- Grafana Labs Engineering Blog
- CNCF Observability Whitepaper
