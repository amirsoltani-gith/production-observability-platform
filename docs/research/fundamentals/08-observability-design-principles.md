# Observability Design Principles

> **Status:** Draft
>
> **Category:** Fundamentals
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-02

---

# Introduction

Building an observability platform is not simply a matter of deploying tools.

Poor design decisions can result in excessive telemetry, high operational costs, slow investigations, and unreliable dashboards.

Effective observability begins with sound engineering principles that guide how telemetry is generated, structured, and used.

This document introduces the design principles that enable scalable, maintainable, and production-ready observability platforms.

---

# Why Design Principles Matter

Observability platforms process enormous volumes of telemetry.

As systems grow, telemetry volume often increases faster than the infrastructure itself.

Without clear design principles, organizations commonly experience:

- High storage costs
- Noisy dashboards
- Alert fatigue
- Slow investigations
- Difficult telemetry correlation
- Poor query performance

Good observability is not measured by the amount of telemetry collected.

It is measured by how effectively telemetry answers operational questions.

---

# Design for Questions, Not Data

A common mistake is collecting telemetry simply because it is available.

Instead, telemetry should exist to answer specific engineering questions.

Examples include:

- Why did request latency increase?
- Which deployment introduced this error?
- Which dependency is failing?
- Why is only one region affected?
- Which service is the bottleneck?

If telemetry cannot help answer meaningful operational questions, it probably should not be collected.

---

# Principles of Good Observability

A production-ready observability platform should follow these principles:

- Collect meaningful telemetry.
- Preserve operational context.
- Correlate telemetry across systems.
- Minimize unnecessary complexity.
- Balance visibility with operational cost.
- Standardize telemetry generation.
- Design for long-term maintainability.

These principles apply regardless of the specific tools being used.

---

# High Cardinality vs Low Cardinality

One of the most important design decisions in observability is choosing the right attributes for telemetry.

Every metric label, log field, and trace attribute affects storage efficiency, query performance, and operational cost.

Understanding cardinality is therefore essential.

---

## What Is Cardinality?

Cardinality refers to the number of unique values an attribute can have.

Examples:

| Attribute | Cardinality |
|----------|-------------|
| environment | Low |
| service.name | Low |
| region | Low |
| availability_zone | Low |
| http.status_code | Low |
| user.id | High |
| session.id | High |
| email | High |
| request.id | Very High |

The more unique values an attribute contains, the higher its cardinality.

---

## Why High Cardinality Is Challenging

High-cardinality attributes increase:

- Storage requirements
- Memory consumption
- Query execution time
- Index size
- Infrastructure cost

Poor label design is one of the most common causes of expensive observability platforms.

Collecting more metadata is not always beneficial if it significantly reduces platform efficiency.

---

## When High Cardinality Is Appropriate

High-cardinality data is not inherently bad.

It simply belongs in the right telemetry signal.

A general guideline is:

| Signal | High Cardinality |
|--------|------------------|
| Metrics | Avoid |
| Logs | Acceptable |
| Traces | Expected |

For example:

- `user.id` should rarely be used as a metric label.
- The same value may be perfectly appropriate as a trace attribute.
- Logs often benefit from additional contextual fields for investigation.

Selecting the correct signal is often more important than collecting additional data.

---

# Labels and Attributes

Labels and attributes provide the context that makes telemetry meaningful.

Without context, a metric such as:

```
http_requests_total
```

provides limited operational value.

Adding structured attributes allows engineers to ask richer questions.

Example:

```
service.name = payment
environment = production
region = eu-central
status.code = 500
```

Well-designed attributes improve:

- Filtering
- Aggregation
- Correlation
- Dashboard design
- Incident investigation

They should follow consistent naming conventions across the organization.

---

# Context First

Telemetry should always preserve operational context.

Context answers questions such as:

- Which service generated this signal?
- Which deployment version is running?
- Which environment is affected?
- Which request triggered this event?

Without sufficient context, telemetry becomes isolated data rather than actionable operational information.

A useful principle is:

> Every telemetry signal should explain **where**, **what**, and **under which conditions** it was generated.

---

# Correlation by Default

Individual metrics, logs, and traces provide only partial visibility.

Observability platforms become significantly more valuable when telemetry is correlated by default.

Examples include:

- Linking logs to traces using Trace IDs.
- Navigating from an alert to the related trace.
- Opening logs directly from a distributed trace.
- Connecting metrics to deployments and infrastructure metadata.

Correlation reduces investigation time and minimizes manual searching across multiple tools.

It should be designed into the platform rather than added later.

---

# Sampling Strategies

As systems scale, telemetry volume increases rapidly.

Collecting every log, metric, and trace is often impractical due to storage, processing, and cost constraints.

Sampling is the practice of collecting only a representative subset of telemetry while preserving enough information for operational analysis.

Sampling should improve efficiency without significantly reducing observability.

---

## Why Sampling Matters

Without sampling, organizations may experience:

- Excessive storage costs
- High network utilization
- Slow query performance
- Increased infrastructure requirements
- Operational inefficiency

Sampling helps maintain a sustainable observability platform.

---

## Common Sampling Strategies

### Head Sampling

A sampling decision is made when a request begins.

Advantages:

- Simple implementation
- Low processing overhead
- Predictable resource usage

Limitations:

- Important traces may be discarded before an error occurs.
- Sampling decisions are made without knowing the final outcome.

---

### Tail Sampling

A sampling decision is made after a request completes.

Advantages:

- Error traces can always be retained.
- Slow requests can be prioritized.
- Better support for production troubleshooting.

Limitations:

- Higher memory usage.
- Increased Collector complexity.
- Additional processing overhead.

Tail sampling is commonly preferred for production environments where preserving valuable traces is more important than minimizing processing cost.

---

# Cost vs Visibility

Observability always involves trade-offs.

Increasing telemetry improves visibility but also increases operational cost.

Reducing telemetry lowers cost but may limit investigation capabilities.

Engineering teams should continuously balance:

- Visibility
- Performance
- Scalability
- Storage
- Operational complexity

The objective is not maximum visibility.

The objective is sufficient visibility to operate systems confidently.

---

# Telemetry Quality

Telemetry quality is more important than telemetry quantity.

High-quality telemetry is:

- Accurate
- Consistent
- Well-structured
- Easy to correlate
- Operationally meaningful

Poor-quality telemetry often includes:

- Duplicate information
- Missing context
- Inconsistent naming
- Excessive labels
- Unused metrics

Regular telemetry reviews should be part of platform maintenance.

---

# Signal-to-Noise Ratio

One of the primary goals of observability design is improving the signal-to-noise ratio.

Signal represents telemetry that helps engineers make decisions.

Noise represents telemetry that consumes resources without providing operational value.

Examples of noise include:

- Unused dashboards
- Redundant metrics
- Duplicate logs
- Low-value alerts
- Excessive trace data

Reducing noise improves investigation speed, dashboard clarity, and operational efficiency.

A mature observability platform continuously removes unnecessary telemetry rather than continuously adding more.

---

# Production Considerations

Design principles should guide every instrumentation and observability decision throughout the software lifecycle.

A production-ready observability platform should prioritize long-term maintainability over short-term visibility.

---

## Design for Scalability

An observability platform must continue operating as telemetry volume grows.

When designing telemetry pipelines, consider:

- Expected traffic growth
- Telemetry ingestion rate
- Storage scalability
- Query performance
- Collector capacity

Scalable observability should be planned from the beginning rather than introduced after operational problems appear.

---

## Design for Reliability

Telemetry pipelines are production infrastructure.

If the telemetry pipeline fails, engineers lose visibility into production systems.

Critical components such as collectors, storage backends, and visualization platforms should be designed with:

- High availability
- Redundancy
- Health monitoring
- Failure recovery

Observability platforms should themselves be observable.

---

## Design for Maintainability

Consistency reduces operational complexity.

Organizations should establish standards for:

- Metric naming
- Resource attributes
- Log structure
- Dashboard organization
- Alert naming
- Instrumentation guidelines

Standardization enables different engineering teams to work with the same operational model.

---

# Common Anti-patterns

## Collecting Everything

More telemetry does not automatically improve observability.

Unnecessary telemetry increases:

- Cost
- Noise
- Query latency
- Maintenance effort

Collect only telemetry with a clear operational purpose.

---

## Designing Around Tools

Observability should be designed around engineering requirements rather than specific products.

Tools evolve.

Good design principles remain valuable regardless of the technology stack.

---

## Ignoring Operational Cost

Every metric, log, and trace consumes infrastructure resources.

Cost should be treated as an architectural consideration rather than an operational afterthought.

---

## Missing Standards

Without shared conventions, each team creates its own telemetry model.

This leads to:

- Inconsistent dashboards
- Difficult investigations
- Duplicate metrics
- Poor correlation

Standardization should be established early.

---

# Best Practices

- Design telemetry around operational questions.
- Prefer low-cardinality labels for metrics.
- Preserve meaningful context.
- Correlate telemetry by default.
- Apply sampling intentionally.
- Review telemetry quality regularly.
- Eliminate low-value telemetry.
- Treat observability as part of system architecture, not as an add-on.

---

# Key Takeaways

- Good observability is driven by design principles, not by tooling alone.
- Telemetry should answer engineering questions rather than maximize data collection.
- Context and correlation are essential for effective incident investigation.
- Cardinality, sampling, and telemetry quality directly affect scalability and cost.
- Reducing operational noise improves both platform efficiency and engineer productivity.

---

# Looking Ahead

The following documents build upon these design principles by introducing reliability-focused operational models:

- Golden Signals
- RED Method
- USE Method
- SLI, SLO, and SLA

These frameworks transform well-designed telemetry into measurable reliability objectives.

---

# References

## Official Documentation

- OpenTelemetry Documentation
- OpenTelemetry Semantic Conventions
- Prometheus Documentation

## Industry Resources

- Google Site Reliability Engineering
- The Site Reliability Workbook
- Observability Engineering – Charity Majors, Liz Fong-Jones, George Miranda

## Further Reading

- CNCF Observability Whitepaper
- Grafana Labs Engineering Blog
- Honeycomb Engineering Blog
