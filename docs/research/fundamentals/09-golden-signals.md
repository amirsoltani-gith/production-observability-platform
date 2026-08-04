# Golden Signals

> **Status:** Draft
>
> **Category:** Reliability Engineering
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-02

---

# Introduction

Operating a production system requires more than collecting telemetry.

Engineering teams need a small set of reliable indicators that quickly answer a fundamental question:

**Is the service healthy from the user's perspective?**

The Four Golden Signals provide this operational perspective.

Originally introduced by Google's Site Reliability Engineering (SRE) practices, they help engineers detect service degradation, prioritize investigations, and measure system health consistently.

Rather than monitoring every available metric, the Golden Signals focus on the indicators that most directly reflect user experience.

---

# Why Golden Signals Matter

Modern production systems generate thousands of metrics.

Without a structured approach, engineers may struggle to determine which metrics actually indicate service health.

The Golden Signals solve this problem by identifying four categories that reveal whether a service is operating correctly.

These signals enable engineering teams to:

- Detect production incidents quickly.
- Prioritize operational investigations.
- Measure service health consistently.
- Build meaningful dashboards.
- Define service reliability objectives.
- Reduce unnecessary monitoring noise.

They provide a common operational language across development, operations, and SRE teams.

---

# The Four Golden Signals

The model consists of four complementary signals.

```
            Service Health

                  │

    ┌─────────────┼─────────────┐

    ▼             ▼             ▼

 Latency       Traffic       Errors

                  │

                  ▼

             Saturation
```

Each signal represents a different aspect of system behavior.

No single signal is sufficient on its own.

Together, they provide a balanced view of production health.

---

# Relationship Between the Signals

The four signals are closely connected.

For example:

- Increasing traffic may increase latency.
- Higher latency may lead to request timeouts.
- Timeouts increase error rates.
- Growing traffic may consume available resources and cause saturation.

A production incident often affects multiple signals simultaneously.

Understanding these relationships allows engineers to identify root causes more quickly rather than investigating isolated metrics.

---

# Latency

Latency measures how long it takes for a service to process a request and return a response.

From a user's perspective, latency is often the most visible indicator of service quality.

Even if a request eventually succeeds, excessive latency may still result in a poor user experience.

Typical examples include:

- Slow page loads
- Delayed API responses
- Long checkout times
- Slow database queries

Latency should always be evaluated from the perspective of the user rather than the internal implementation of the system.

---

## Types of Latency

Google SRE distinguishes between two important categories.

### Successful Request Latency

Measures the response time of requests that complete successfully.

This value reflects normal service performance.

---

### Failed Request Latency

Failed requests should also be measured.

Many systems return failures almost immediately, while others fail only after long timeout periods.

These two situations have very different operational implications.

Separating successful and failed request latency provides a more accurate understanding of system behavior.

---

## Common Latency Metrics

Examples include:

- Average response time
- P95 latency
- P99 latency
- Maximum response time

Percentiles are generally preferred over averages because they better represent the experience of most users.

---

# Traffic

Traffic measures the amount of work a service performs.

It answers the question:

> **How much demand is the system currently handling?**

Traffic should represent actual workload rather than infrastructure utilization.

Examples include:

- HTTP requests per second (RPS)
- Transactions per second (TPS)
- Active users
- Messages processed
- Database queries
- Queue throughput

The most appropriate traffic metric depends on the purpose of the service.

---

## Why Traffic Matters

Traffic provides the context needed to interpret other Golden Signals.

For example:

- High latency during low traffic may indicate inefficient code.
- High latency during peak traffic may suggest capacity limitations.
- Increased traffic without additional errors may indicate successful scaling.

Traffic rarely provides useful insights when viewed in isolation.

It should always be interpreted together with latency, errors, and saturation.

---

# Errors

Errors measure the proportion of requests that fail to achieve their intended outcome.

From a user's perspective, an unsuccessful request is often more significant than a slow request.

Errors may include:

- HTTP 5xx responses
- Failed API requests
- Database failures
- Authentication failures
- Timeout errors
- Application exceptions

Not every error represents a production incident.

For example, an HTTP 404 response may be expected, while repeated HTTP 500 responses usually indicate a service problem.

Engineering teams should clearly distinguish between expected and unexpected failures.

---

## Error Rate

A common reliability indicator is the error rate.

```
Error Rate = Failed Requests / Total Requests
```

Tracking error rate over time helps engineers detect service degradation before complete outages occur.

Error rate is frequently used when defining Service Level Indicators (SLIs) and Service Level Objectives (SLOs).

---

# Saturation

Saturation measures how close a service is to exhausting its available resources.

Unlike traffic, which measures demand, saturation measures capacity.

A service can handle increasing traffic successfully until one or more critical resources approach their operational limits.

At that point, performance typically degrades rapidly.

---

## Common Sources of Saturation

Depending on the system, saturation may involve:

- CPU utilization
- Memory consumption
- Disk I/O
- Network bandwidth
- Database connection pools
- Thread pools
- Message queue depth
- Kubernetes Pod capacity

The specific resource is less important than understanding whether the system has sufficient capacity to continue handling additional workload.

---

## Why Saturation Matters

Saturation is often an early warning signal.

A service may continue operating normally while approaching resource limits.

Without monitoring saturation, engineering teams may only become aware of a problem after users begin experiencing increased latency or request failures.

Monitoring saturation supports proactive capacity planning and helps prevent performance degradation before customer impact occurs.

---

# How the Four Signals Work Together

The Golden Signals should always be interpreted collectively.

A single metric rarely provides enough information to understand production health.

Consider the following examples.

## Example 1 — Increased Traffic

```
Traffic ↑
Latency →
Errors →
Saturation →
```

Interpretation:

The service is handling additional workload without signs of degradation.

This may indicate successful horizontal scaling or available spare capacity.

---

## Example 2 — Capacity Bottleneck

```
Traffic →
Latency ↑
Errors →
Saturation ↑
```

Interpretation:

Demand has not changed, but resource utilization is approaching operational limits.

The likely cause is a capacity constraint or an inefficient component within the service.

---

## Example 3 — Service Failure

```
Traffic →
Latency ↑
Errors ↑
Saturation →
```

Interpretation:

The service is becoming slower and requests are failing, but infrastructure resources appear healthy.

This pattern often indicates an application-level problem such as:

- Software defects
- External dependency failures
- Database issues
- Configuration problems

---

## Example 4 — Traffic Surge

```
Traffic ↑
Latency ↑
Errors ↑
Saturation ↑
```

Interpretation:

The service is experiencing increased demand while approaching or exceeding available capacity.

Possible causes include:

- Traffic spikes
- Insufficient scaling
- Resource exhaustion
- Capacity planning issues

This scenario typically requires immediate operational attention.

---

# Golden Signals and Observability

The Golden Signals are not replacements for metrics, logs, or traces.

Instead, they define **what should be measured**.

Observability provides the telemetry required to investigate why a Golden Signal changes.

For example:

```
Golden Signal

High Latency

      │

      ▼

Metrics

Response Time Increased

      │

      ▼

Trace

Payment Service Waiting

      │

      ▼

Logs

Database Connection Timeout
```

This relationship illustrates how reliability indicators and observability complement one another.

The Golden Signals identify that a problem exists.

Observability explains why it exists.

---

# Production Considerations

The Golden Signals should be used as a decision-making framework rather than a checklist of metrics.

Different services expose different operational characteristics, but every production service should be evaluated through the lenses of latency, traffic, errors, and saturation.

---

## Focus on User Experience

Golden Signals measure service health from the user's perspective.

Infrastructure metrics such as CPU utilization or memory consumption are valuable, but they should support—not replace—user-centric indicators.

A healthy infrastructure does not necessarily mean users are having a healthy experience.

---

## Establish Meaningful Thresholds

Thresholds should reflect service objectives rather than arbitrary values.

Examples include:

- P95 latency below the agreed target.
- Error rate below the acceptable threshold.
- Saturation remaining below operational limits.
- Traffic remaining within expected capacity.

Thresholds should evolve as the system and workload evolve.

---

## Combine Golden Signals with Observability

The Golden Signals identify that a problem exists.

Observability explains why it exists.

A mature engineering workflow typically follows this sequence:

```
Golden Signal Alert
          │
          ▼
Identify Affected Service
          │
          ▼
Inspect Metrics
          │
          ▼
Analyze Traces
          │
          ▼
Review Logs
          │
          ▼
Identify Root Cause
```

This workflow reduces Mean Time to Detection (MTTD) and Mean Time to Resolution (MTTR).

---

## Review Trends Instead of Individual Values

A single measurement rarely provides sufficient insight.

Engineers should evaluate:

- Historical trends
- Seasonal patterns
- Deployment changes
- Capacity growth
- Long-term reliability

Trend analysis provides far more operational value than isolated measurements.

---

# Common Anti-patterns

## Monitoring Only Infrastructure

Healthy servers do not guarantee healthy services.

Always evaluate service-level indicators alongside infrastructure metrics.

---

## Watching Individual Metrics in Isolation

Latency, traffic, errors, and saturation influence one another.

Investigating only one signal may lead to incorrect conclusions.

Always evaluate the complete operational picture.

---

## Using Average Latency Alone

Average latency often hides poor user experiences.

Prefer percentile-based measurements such as:

- P95
- P99

These values better represent real production behavior.

---

## Ignoring Saturation

Many incidents begin with gradual resource exhaustion.

Monitoring saturation helps identify problems before users experience failures.

---

# Best Practices

- Build dashboards around the Four Golden Signals.
- Define alerts using service-level indicators instead of infrastructure metrics alone.
- Monitor latency using percentiles rather than averages.
- Correlate Golden Signals with metrics, logs, and traces.
- Review trends over time instead of focusing only on current values.
- Continuously refine thresholds as applications evolve.

---

# Key Takeaways

- The Four Golden Signals provide a simple framework for evaluating service health.
- Latency measures responsiveness.
- Traffic measures workload.
- Errors measure service reliability.
- Saturation measures remaining capacity.
- The signals are most effective when interpreted together.
- Observability provides the evidence required to explain changes in the Golden Signals.

---

# Looking Ahead

The Golden Signals identify what should be monitored from a service reliability perspective.

The next document introduces two complementary operational models:

- RED Method
- USE Method

Together, these frameworks help engineering teams design more comprehensive monitoring strategies for applications and infrastructure.

---

# References

## Official Documentation

- Google Site Reliability Engineering
- The Site Reliability Workbook

## Industry Resources

- Google Cloud Architecture Center
- CNCF Observability Whitepaper

## Further Reading

- Observability Engineering – Charity Majors, Liz Fong-Jones, George Miranda
- Prometheus Documentation
- Grafana Documentation
