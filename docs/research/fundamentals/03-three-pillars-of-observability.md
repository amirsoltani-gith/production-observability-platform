# Three Pillars of Observability

> **Status:** Reviewed
>
> **Category:** Fundamentals
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-07-31

---

# Introduction

Modern observability platforms are built around three primary telemetry signals:

- Metrics
- Logs
- Distributed Traces

These signals provide different perspectives into system behavior.

No single signal can provide complete visibility into a modern distributed system.

Metrics can indicate that a problem exists.

Logs can provide detailed information about events.

Traces can reveal how requests move through complex service architectures.

Together, these signals allow engineers to move from simple detection toward deeper understanding and effective incident investigation.

A mature observability strategy does not focus on collecting the maximum amount of data.

Instead, it focuses on collecting the right information with enough context to answer important operational questions.

---

# Why Multiple Telemetry Signals Are Required

Early monitoring systems often focused primarily on infrastructure metrics.

Examples:

- CPU usage
- Memory consumption
- Disk utilization
- Network traffic

These measurements were valuable for traditional environments where applications were relatively simple.

However, modern distributed systems introduced new challenges.

A production application may now include:

- Multiple microservices
- Containers
- Kubernetes workloads
- Databases
- Message queues
- External APIs
- Service dependencies

A single failure can involve interactions between multiple components.

For example:

User Request

  |
  v

API Gateway

  |
  v

Order Service

  |
  v

Payment Service

  |
  v

External Payment Provider


A metric may show:


Request latency increased


However, it does not immediately explain:

- Which service caused the delay?
- Why did latency increase?
- Which dependency failed?
- What changed before the incident?

Different telemetry signals answer different questions.

---

# The Three Telemetry Questions

A useful way to understand the relationship between the three pillars is through the questions they answer.

## Metrics

Metrics answer:

> "What is happening?"

Examples:

- Error rate increased
- CPU utilization is high
- Request latency is growing

Metrics are excellent for detecting trends and triggering alerts.

---

## Logs

Logs answer:

> "What happened?"

Examples:

- Application returned an error
- Database connection failed
- Authentication request was rejected

Logs provide detailed event information.

---

## Distributed Traces

Traces answer:

> "Where did it happen and how did the request flow?"

Examples:

- Payment service introduced latency
- Database query caused slowdown
- External dependency failed

Traces show relationships between distributed components.

---

# Metrics

## Definition

Metrics are numerical measurements collected over time that represent the state or behavior of a system.

A metric is typically represented as a value associated with:

- A name
- A timestamp
- A set of labels or dimensions

Example:

http_requests_total{service="api",status="200"} 15000


Metrics transform system behavior into measurable data that can be analyzed over time.

---

# What Problems Do Metrics Solve?

Metrics are primarily used for:

- Detecting system health changes
- Identifying trends
- Creating alerts
- Capacity planning
- Measuring reliability objectives

Examples:

## Availability Monitoring

Question:


Is the service available?


Metric:


HTTP availability percentage


---

## Performance Monitoring

Question:


Is the application becoming slower?


Metrics:


Request latency
Response time
Throughput


---

## Resource Monitoring

Question:


Are infrastructure resources becoming exhausted?


Metrics:


CPU usage
Memory usage
Disk utilization
Network traffic


---

# Common Metric Types

Different metric types are useful for different operational scenarios.

---

## Counter

A counter represents a value that only increases over time.

Examples:

- Total HTTP requests
- Total errors
- Total processed messages

Example:


api_requests_total = 150000


Counters are commonly used to calculate rates.

Example:


Requests per second
Error rate


---

## Gauge

A gauge represents a value that can increase or decrease.

Examples:

- Current memory usage
- Active connections
- Queue size

Example:


active_connections = 250


---

## Histogram

A histogram measures the distribution of values.

Common examples:

- Request duration
- Response size
- Processing time

Histograms are useful for understanding performance characteristics.

Example:


95th percentile latency = 300ms


---

# Advantages of Metrics

Metrics provide several important benefits.

## Efficient Storage

Metrics are usually more compact than logs.

Large amounts of historical information can be stored efficiently.

---

## Fast Querying

Metrics systems are optimized for time-series analysis.

Engineers can quickly answer questions about:

- Trends
- Changes
- Patterns

---

## Powerful Alerting

Metrics are commonly used as the foundation for automated alerts.

Examples:


Error rate > 5%

CPU usage > 90%

Latency > 1 second


---

## Long-Term Analysis

Historical metrics help with:

- Capacity planning
- Performance optimization
- Reliability analysis

---

# Limitations of Metrics

Although metrics are one of the most important observability signals, they have limitations.

Metrics provide numerical information, but they often lack detailed context about individual events.

For example:

A metric shows:


HTTP error rate increased from 1% to 15%


This tells engineers that failures increased.

However, it does not immediately explain:

- Which users were affected?
- Which requests failed?
- Which service generated the errors?
- What exact error occurred?

Additional telemetry signals are required to answer these questions.

---

# Production Considerations for Metrics

A production metrics system requires careful engineering decisions.

Important considerations include:

## Metric Naming

Poorly designed metric names make troubleshooting difficult.

Good metrics should be:

- Consistent
- Meaningful
- Easy to query
- Clearly documented

Example:

Good:


http_request_duration_seconds


Poor:


request_time_value_1


---

## Labels and Cardinality

Labels provide additional dimensions for analysis.

Example:


http_requests_total{
service="payment",
region="eu-west",
status="500"
}


Labels improve investigation capabilities.

However, excessive labels can create high cardinality.

Examples of dangerous labels:

- User ID
- Request ID
- Random identifiers

High cardinality can increase:

- Storage requirements
- Query complexity
- Memory consumption

---

## Retention Strategy

Not all metrics require the same retention period.

Organizations should define:

- Short-term operational retention
- Long-term capacity planning retention
- Aggregated historical metrics

Retention policies help balance visibility and cost.

---

# Logs

## Definition

Logs are timestamped records of events generated by applications, operating systems, and infrastructure components.

Unlike metrics, which summarize system behavior numerically, logs provide detailed information about individual events.

Examples:


2026-07-31 12:00:01 ERROR Payment request failed

reason=timeout

provider=external-payment-api


Logs answer questions about what happened inside the system.

---

# What Problems Do Logs Solve?

Logs are primarily used for:

- Debugging application failures
- Investigating incidents
- Understanding application behavior
- Auditing system activity
- Identifying abnormal events

Examples:

A metric shows:


Payment error rate increased


A log explains:


Payment service failed because external provider returned timeout


Logs provide the context required to understand specific events.

---

# Structured Logging

Modern observability platforms rely heavily on structured logging.

Traditional logs are often plain text.

Example:


User login failed


Although readable, they are difficult for machines to analyze.

Structured logs store information in a consistent format.

Example:

```json
{
  "service": "authentication",
  "event": "login_failed",
  "user_id": "12345",
  "reason": "invalid_password",
  "timestamp": "2026-07-31T12:00:00Z"
}

Structured logging enables:

Faster searching
Better filtering
Easier correlation
Automated analysis
Log Levels

Most applications classify logs using different severity levels.

Common levels include:

DEBUG

Detailed information useful during development or troubleshooting.

Example:

Database query execution details
INFO

Normal operational events.

Example:

Application started successfully
WARNING

Unexpected conditions that may require attention.

Example:

Cache response time increased
ERROR

Failures that affect functionality.

Example:

Database connection failed
CRITICAL

Severe failures requiring immediate attention.

Example:

Service unavailable
Advantages of Logs
Detailed Context

Logs provide information that metrics cannot.

They explain specific events and failures.

Troubleshooting Capability

During incidents, logs help engineers understand:

What happened
When it happened
Which component generated the issue
Audit and Compliance

Logs can support:

Security investigations
Change tracking
Compliance requirements
Application Visibility

Logs reveal internal application behavior.

Examples:

Business logic failures
Dependency errors
User actions
Limitations of Logs

Logs are powerful, but they introduce challenges.

High Data Volume

Large applications can generate millions of log entries.

This creates challenges related to:

Storage
Processing
Searching
Retention
Poor Logging Practices

Bad logging can reduce observability.

Examples:

Missing context
Inconsistent formats
Excessive noise
Sensitive information exposure
Difficult Correlation

A single request may generate logs across many services.

Without identifiers such as request IDs or trace IDs, connecting related events becomes difficult.

Production Considerations for Logs

A production logging strategy should consider:

Centralized Collection

Logs should be collected from multiple sources into a centralized platform.

Common sources:

Applications
Containers
Kubernetes workloads
Operating systems
Log Retention

Organizations should define:

What logs must be stored
How long they should be retained
Which logs can be deleted
Security

Logs must be carefully managed.

Avoid storing:

Passwords
Tokens
Secrets
Personal information
Log Correlation

Logs become significantly more valuable when connected with:

Metrics
Traces
Request identifiers

---

# Distributed Traces

## Definition

Distributed tracing is a telemetry method that records the complete journey of a request as it moves through multiple services.

In modern distributed systems, a single user action may involve many independent components.

Example:


User Request

|
v

API Gateway

|
v

Authentication Service

|
v

Order Service

|
v

Payment Service

|
v

Database


A trace captures this entire request path and shows how much time each component spends processing the request.

---

# What Problems Do Traces Solve?

Distributed traces are primarily used for understanding request flow in complex systems.

They help answer questions such as:

- Where did latency occur?
- Which service caused the failure?
- Which dependency affected performance?
- How do services communicate with each other?

Without tracing, engineers often need to manually inspect multiple systems and guess where the problem originated.

---

# Components of a Trace

A distributed trace is composed of smaller units called spans.

A span represents a single operation within a request.

Example:


Trace: checkout-request

|
├── API Gateway 20ms
|
├── Authentication 50ms
|
├── Order Service 100ms
|
├── Payment Service 2500ms
|
└── Database 80ms


The complete trace shows:

- The full request path
- The relationship between services
- The time spent in each operation

---

# Advantages of Distributed Tracing

## Understanding Service Dependencies

Tracing reveals how services interact.

This is especially important in:

- Microservice architectures
- Kubernetes environments
- Service mesh platforms

---

## Finding Performance Bottlenecks

Tracing helps identify where time is spent.

Example:

A request takes five seconds.

Without tracing:


Something is slow


With tracing:


Payment Service
|
└── External API call
|
└── 4.5 seconds latency


The root cause becomes visible.

---

## Faster Incident Investigation

During production incidents, traces reduce investigation time.

Engineers can quickly identify:

- The failing service
- The slow operation
- The affected dependency

---

# Limitations of Distributed Tracing

Although tracing is extremely powerful, it has challenges.

---

## Instrumentation Complexity

Applications must be instrumented correctly to generate useful traces.

Poor instrumentation results in incomplete visibility.

---

## Storage Requirements

Large distributed systems can generate millions of traces.

Organizations need:

- Sampling strategies
- Retention policies
- Efficient storage systems

---

## Operational Complexity

Tracing platforms require additional components:

- Trace collectors
- Storage backends
- Query systems
- Visualization tools

---

# Production Considerations for Distributed Tracing

## Trace Sampling

Collecting every trace may be expensive at scale.

Organizations often use sampling strategies.

Examples:

- Head-based sampling
- Tail-based sampling

The goal is maintaining useful visibility while controlling cost.

---

## Context Propagation

For traces to work correctly, services must propagate request context.

Common identifiers include:

- Trace ID
- Span ID

These identifiers allow engineers to connect activity across multiple services.

---

## Privacy Considerations

Traces may contain:

- Request metadata
- User information
- Service details

Sensitive information should be handled carefully.

---

# Correlation Between Metrics, Logs, and Traces

The true power of observability appears when the three signals are connected.

Each signal provides a different perspective.


Metrics

"What is happening?"

    |
    v

Logs

"What happened?"

    |
    v

Traces

"Where did it happen?"


Together they create a complete investigation workflow.

Example:

## Step 1: Metric Alert

Monitoring detects:


HTTP error rate increased


The team knows a problem exists.

---

## Step 2: Trace Investigation

Tracing shows:


Payment Service
|
└── External Provider Timeout


The team identifies the problematic component.

---

## Step 3: Log Analysis

Logs reveal:


Connection timeout after 30 seconds
Retry limit exceeded


The team understands the exact failure.

---

This workflow transforms incident response from guessing into evidence-based investigation.

---

# OpenTelemetry and Telemetry Standardization

Modern observability requires consistent telemetry collection across many systems.

OpenTelemetry was created to provide a vendor-neutral standard for generating, collecting, and exporting telemetry.

It supports:

- Metrics
- Logs
- Distributed traces

The main goal is reducing dependency on specific monitoring vendors and simplifying telemetry integration.

A simplified architecture:


Application

|
v

OpenTelemetry Instrumentation

|
v

OpenTelemetry Collector

|
+-------------+
|             |
v             v

Metrics Traces
Backend Backend


OpenTelemetry does not replace storage or visualization systems.

Instead, it provides a standardized telemetry pipeline.

---

# Engineering Trade-offs

A complete observability platform requires balancing multiple factors.

## Visibility vs Cost

More telemetry improves understanding.

However:

- More data increases storage costs.
- More processing requires additional resources.

---

## Detail vs Performance

Detailed instrumentation provides better insights.

However:

- Instrumentation introduces overhead.
- Excessive telemetry can impact applications.

---

## Completeness vs Simplicity

Collecting every possible signal creates complexity.

A mature platform collects the information needed to support operational decisions.

---

# Common Mistakes

## Collecting Data Without Purpose

More telemetry does not automatically create better observability.

Every collected signal should support a clear operational goal.

---

## Ignoring Correlation

Metrics, logs, and traces provide limited value when isolated.

The strongest insights come from connecting them together.

---

## Lack of Standardization

Different formats and inconsistent naming make troubleshooting difficult.

Standard telemetry practices improve long-term maintainability.

---

## Focusing Only on Tools

Observability is not created by installing:

- Prometheus
- Grafana
- Loki
- Tempo

Tools support observability.

Engineering practices create observability.

---

# Best Practices

- Define operational questions before collecting telemetry.
- Use standardized telemetry formats.
- Correlate metrics, logs, and traces.
- Include meaningful context in telemetry.
- Control storage through retention and sampling strategies.
- Protect sensitive information.
- Review telemetry quality regularly.

---

# Key Takeaways

- Metrics, logs, and traces provide different but complementary views of system behavior.
- Metrics identify trends and abnormal conditions.
- Logs provide detailed information about events.
- Distributed traces reveal request flow across complex systems.
- The combination of all three signals enables effective incident investigation.
- OpenTelemetry provides a standardized approach for collecting telemetry.
- Successful observability requires balancing visibility, cost, performance, and complexity.
- Observability is an engineering capability, not simply a collection of tools.

---

# References

## Official Documentation

- OpenTelemetry Documentation
- Prometheus Documentation
- Grafana Observability Documentation

## Industry Resources

- Google Site Reliability Engineering Documentation
- The Site Reliability Workbook
- Observability Engineering – Charity Majors, Liz Fong-Jones, George Miranda

## Further Reading

- CNCF Observability Landscape
- Honeycomb Engineering Blog
- Grafana Labs Engineering Blog
- OpenTelemetry Specification
