# Service Level Indicators, Objectives, and Agreements (SLI, SLO, SLA)

> **Status:** Draft
>
> **Category:** Reliability Engineering
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-02

---

# Introduction

Reliable systems are not built by intuition.

Engineering teams need measurable objectives that define what "reliable" actually means.

Without measurable targets, questions such as:

- Is our service reliable?
- Are users receiving an acceptable experience?
- Has reliability improved over time?

cannot be answered objectively.

The SLI, SLO, and SLA framework provides a structured approach for measuring, defining, and communicating service reliability.

These concepts form the foundation of modern Site Reliability Engineering (SRE).

---

# Why SLI, SLO, and SLA Matter

Monitoring tells engineers what is happening.

The Golden Signals, RED Method, and USE Method identify operational problems.

However, they do not answer another critical question:

> **How reliable should the service be?**

Engineering teams require measurable targets that distinguish acceptable service behavior from unacceptable service behavior.

SLIs, SLOs, and SLAs provide those targets.

They transform telemetry into measurable reliability goals.

---

# The Relationship Between SLI, SLO, and SLA

These three concepts build upon one another.

```
Telemetry
      │
      ▼
Service Level Indicator (SLI)
      │
      ▼
Service Level Objective (SLO)
      │
      ▼
Service Level Agreement (SLA)
```

Each level depends on the previous one.

Telemetry provides operational data.

SLIs measure service performance.

SLOs define engineering targets.

SLAs communicate contractual commitments.

---

# Service Level Indicator (SLI)

A Service Level Indicator (SLI) is a quantitative measurement of a service's performance or reliability.

An SLI answers the question:

> **How is the service performing right now?**

An SLI is calculated directly from telemetry.

Examples include:

- Successful request percentage
- API availability
- Request latency
- Error rate
- Job completion rate
- Message delivery success rate

An SLI is a measurement, not a target.

It simply describes the current behavior of the service.

---

# Characteristics of a Good SLI

A useful SLI should be:

- Measurable
- Objective
- Customer-focused
- Repeatable
- Easy to calculate
- Derived from reliable telemetry

Poor SLIs often measure internal implementation details rather than user experience.

For example:

- CPU utilization is rarely a good SLI.
- Successful API requests are often an excellent SLI.

SLIs should reflect the quality of the service delivered to users rather than the health of individual infrastructure components.

---

# Service Level Objective (SLO)

A Service Level Objective (SLO) defines the target value that an engineering team wants an SLI to achieve.

An SLO answers the question:

> **What level of reliability are we aiming to provide?**

Unlike an SLI, which measures current performance, an SLO defines the desired level of performance.

Examples include:

- 99.9% successful requests over 30 days.
- 95% of API requests complete within 300 ms.
- 99.95% service availability each calendar month.
- 99% of background jobs complete within 5 minutes.

An SLO is an engineering objective rather than a contractual commitment.

It guides development priorities, operational improvements, and reliability planning.

---

# Characteristics of a Good SLO

A well-designed SLO should be:

- Measurable
- Realistic
- Customer-focused
- Time-bound
- Based on historical data
- Supported by reliable SLIs

Setting unrealistic objectives often leads to unnecessary operational effort without improving customer experience.

Conversely, objectives that are too relaxed may fail to protect service quality.

An effective SLO balances reliability with engineering velocity.

---

# Service Level Agreement (SLA)

A Service Level Agreement (SLA) is a formal commitment made to customers regarding the expected level of service.

Unlike an SLO, which is primarily an internal engineering target, an SLA is a business or legal agreement.

An SLA answers the question:

> **What level of service are we contractually committing to provide?**

Typical SLA commitments include:

- Monthly availability
- Maximum response times
- Support response targets
- Incident resolution targets

Failure to meet an SLA may result in:

- Service credits
- Financial penalties
- Contractual obligations
- Customer compensation

Because of these consequences, organizations usually define internal SLOs that are more demanding than their published SLAs.

---

# Relationship Between SLO and SLA

A common practice is:

```
Internal Reliability

SLO = 99.95%

        │

        ▼

Customer Commitment

SLA = 99.90%
```

The gap between the SLO and the SLA provides a safety margin.

This allows engineering teams to absorb minor incidents without immediately violating contractual commitments.

---

# Practical Example

Consider an online payment service.

```
SLI

Successful payment requests

↓

SLO

99.95% successful payments
during a rolling 30-day period

↓

SLA

99.90% payment availability
guaranteed to customers
```

The SLI provides the measurement.

The SLO defines the engineering target.

The SLA communicates the customer commitment.

Each serves a different purpose while remaining closely connected.

---

# Choosing Meaningful SLIs

Selecting the right Service Level Indicators is one of the most important engineering decisions in SRE.

An SLI should measure what customers actually experience rather than internal system behavior.

For example:

| Poor SLI | Better SLI |
|-----------|------------|
| CPU utilization | Successful API requests |
| Memory usage | Request latency |
| Disk usage | Order completion rate |
| Network throughput | Login success rate |

Infrastructure metrics remain valuable for troubleshooting, but they rarely describe the quality of the service delivered to users.

---

# Common Categories of SLIs

Most production systems define SLIs in one or more of the following categories.

## Availability

Measures whether the service can successfully process requests.

Examples:

- Successful HTTP requests
- Service uptime
- API availability

---

## Latency

Measures how quickly requests are completed.

Examples:

- P95 response time
- P99 response time
- Average request duration

Latency SLIs should typically use percentiles rather than averages.

---

## Quality

Measures whether the service behaves correctly.

Examples:

- Successful payment rate
- Successful order processing
- Message delivery success rate
- Authentication success rate

Quality SLIs are often closely aligned with business objectives.

---

## Freshness

For systems that process data, freshness measures how current the available data is.

Examples:

- Data replication delay
- Cache synchronization delay
- Report generation delay

Freshness is especially important for analytics and data platforms.

---

# Common Misconceptions

Several misunderstandings frequently appear when teams begin adopting SLI, SLO, and SLA.

## "An SLI is just another metric."

Incorrect.

A metric is simply a measurement.

An SLI is a carefully selected metric that represents service quality from the user's perspective.

---

## "Higher SLOs are always better."

Not necessarily.

Increasing an SLO usually requires additional engineering effort and operational cost.

For many services, improving availability from 99.9% to 99.99% may require significantly more infrastructure while providing limited additional customer value.

Reliability should match business requirements.

---

## "SLA and SLO are the same."

They serve different purposes.

- SLOs guide engineering decisions.
- SLAs define customer commitments.

Confusing the two often leads to unrealistic operational expectations.

---

# Relationship with Previous Topics

SLIs are built from the telemetry discussed in earlier documents.

```
Metrics
Logs
Traces
      │
      ▼
Golden Signals
RED
USE
      │
      ▼
SLIs
      │
      ▼
SLOs
      │
      ▼
SLAs
```

This progression demonstrates how raw telemetry evolves into measurable reliability objectives that guide engineering decisions.

---

# Production Considerations

Defining SLIs, SLOs, and SLAs is not enough.

They must be reviewed regularly to ensure they continue reflecting customer expectations and business objectives.

Reliability targets should evolve alongside the service.

---

## Define User-Centric SLIs

The best SLIs measure what users actually experience.

Examples include:

- Successful API requests
- Checkout completion rate
- Login success rate
- Request latency

Avoid defining SLIs solely around infrastructure metrics such as CPU or memory utilization.

Infrastructure metrics help explain problems but rarely represent service quality.

---

## Set Realistic SLOs

An SLO should challenge the engineering team without being unattainable.

When defining SLOs, consider:

- Historical performance
- Business impact
- Customer expectations
- Operational cost
- Engineering capacity

An unrealistic SLO often results in unnecessary operational effort and alert fatigue.

---

## Align SLAs with Business Needs

SLAs should reflect customer commitments rather than internal engineering aspirations.

A common approach is:

```
Engineering Target

SLO = 99.95%

        │

        ▼

Customer Commitment

SLA = 99.90%
```

Maintaining this safety margin reduces the risk of violating contractual obligations during routine operational incidents.

---

## Review Reliability Objectives Regularly

Reliability requirements change as systems evolve.

Review SLOs when:

- New features are introduced.
- Customer expectations change.
- System architecture changes.
- Capacity increases significantly.
- Historical reliability improves.

Reliability objectives should remain relevant throughout the service lifecycle.

---

# Common Anti-patterns

## Measuring Infrastructure Instead of Service Quality

High CPU utilization does not necessarily indicate poor customer experience.

SLIs should primarily reflect user-facing outcomes.

---

## Setting Unrealistic SLOs

Choosing "five nines" availability without a business justification often increases operational cost without delivering proportional customer value.

Reliability targets should be driven by business requirements rather than aspiration.

---

## Publishing Internal SLOs as SLAs

Engineering objectives should not automatically become contractual commitments.

Organizations typically maintain stricter internal targets than those promised to customers.

---

## Ignoring SLI Quality

Poor-quality SLIs lead to misleading SLOs.

If the measurement is inaccurate, the objective built upon it will also be unreliable.

Always validate that SLIs accurately represent customer experience.

---

# Best Practices

- Select SLIs that measure user experience.
- Build SLOs using historical operational data.
- Maintain a safety margin between SLOs and SLAs.
- Review reliability objectives regularly.
- Use SLOs to guide engineering priorities.
- Ensure SLIs are easy to measure and understand.
- Keep reliability goals aligned with business value.

---

# Key Takeaways

- SLIs measure current service performance.
- SLOs define internal engineering reliability targets.
- SLAs communicate contractual commitments to customers.
- Effective SLOs depend on meaningful SLIs.
- Reliability targets should balance customer expectations, engineering effort, and operational cost.
- SLI, SLO, and SLA form the foundation of modern Site Reliability Engineering.

---

# Looking Ahead

Defining reliability objectives naturally leads to an important question:

> **How much unreliability is acceptable while continuing to deliver new features?**

The next document introduces **Error Budgets**, a core SRE concept that helps engineering teams balance service reliability with development velocity.

---

# References

## Official Documentation

- Google Site Reliability Engineering
- The Site Reliability Workbook

## Industry Resources

- Google Cloud Architecture Center
- CNCF Reliability Resources

## Further Reading

- Seeking SRE – David N. Blank-Edelman
- Observability Engineering – Charity Majors, Liz Fong-Jones, George Miranda
- Prometheus Documentation
