# OpenTelemetry Fundamentals

> **Status:** Draft
>
> **Category:** Fundamentals
>
> **Audience:** DevOps Engineers, Site Reliability Engineers (SREs), Platform Engineers, Cloud Engineers
>
> **Last Updated:** 2026-08-01

---

# Introduction

Modern observability platforms depend on a consistent and standardized way to generate telemetry.

As organizations adopt distributed systems, cloud-native architectures, and multi-vendor observability platforms, producing telemetry in a portable and consistent format becomes increasingly important.

OpenTelemetry addresses this challenge by providing an open, vendor-neutral standard for telemetry generation, collection, and transport.

Today, it has become the de facto standard for cloud-native observability.

---

# Why OpenTelemetry Was Created

Before OpenTelemetry, every observability vendor provided its own instrumentation libraries, telemetry formats, and collection mechanisms.

As a result, engineering teams often became tightly coupled to a specific monitoring platform.

Changing vendors usually required significant application changes, making migrations expensive and time-consuming.

OpenTelemetry was created to separate application instrumentation from observability backends.

Applications generate telemetry once using a standardized approach, while organizations remain free to choose the backend that best fits their operational requirements.

This significantly reduces vendor lock-in and improves long-term platform flexibility.

---

# The Problem Before OpenTelemetry

Historically, organizations faced several common challenges.

## Vendor-specific SDKs

Each observability platform required its own SDK.

Examples included:

- Datadog SDK
- New Relic Agent
- AppDynamics Agent
- Dynatrace Agent

Applications often depended directly on these proprietary libraries.

---

## Inconsistent Telemetry

Different tools represented telemetry differently.

As a result:

- Metrics were incompatible.
- Trace formats differed.
- Context propagation varied.
- Correlation became difficult.

Building a unified observability platform required significant engineering effort.

---

## Migration Complexity

Replacing an observability platform often required:

- Rewriting instrumentation.
- Deploying new agents.
- Updating application code.
- Rebuilding dashboards.

The cost of migration discouraged organizations from adopting better solutions.

---

# The Emergence of OpenTelemetry

OpenTelemetry was developed under the Cloud Native Computing Foundation (CNCF) to establish a common standard for observability.

Rather than competing with monitoring platforms, OpenTelemetry provides a shared foundation that every platform can adopt.

Its primary objective is standardization.

Applications produce telemetry using OpenTelemetry, while any compatible backend can receive and process that telemetry.

This creates a clear separation between telemetry producers and telemetry consumers.

---

# OpenTelemetry Architecture

At a high level, OpenTelemetry sits between applications and observability platforms.

```text
                  Application
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ▼                               ▼
 Auto Instrumentation         Manual Instrumentation
        │                               │
        └───────────────┬───────────────┘
                        │
                        ▼
                 OpenTelemetry SDK
                        │
                        ▼
              OpenTelemetry Collector
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Prometheus         Loki            Tempo
        │               │               │
        └───────────────┴───────────────┘
                        │
                        ▼
                     Grafana
```

This layered architecture provides flexibility, portability, and scalability without tightly coupling applications to a specific observability vendor.

---

## OpenTelemetry in the Observability Ecosystem

OpenTelemetry is not an observability platform.

It is the telemetry layer that connects applications to observability backends.

```text
                 Application
                      │
                      ▼
             OpenTelemetry
       (API • SDK • Collector)
                      │
      ┌───────────────┼────────────────┐
      ▼               ▼                ▼
 Prometheus         Loki            Tempo
  (Metrics)         (Logs)         (Traces)
      └───────────────┼────────────────┘
                      ▼
                   Grafana
```

OpenTelemetry standardizes telemetry generation and transport.

Prometheus, Loki, and Tempo specialize in storing different telemetry signals, while Grafana provides visualization and exploration.

---

# Core Components

OpenTelemetry consists of four primary building blocks:

| Component | Responsibility |
|-----------|----------------|
| API | Defines how applications create telemetry |
| SDK | Generates and exports telemetry |
| Collector | Receives, processes, and forwards telemetry |
| OTLP | Standard protocol for telemetry transport |

Each component has a distinct responsibility, allowing organizations to evolve their observability platform without changing application code.

---

# OpenTelemetry API

The OpenTelemetry API defines how applications describe telemetry.

It provides a consistent programming interface for creating:

- Metrics
- Logs
- Traces

The API itself does not collect or export telemetry.

Instead, it defines **what telemetry should be generated**.

This separation allows application developers to instrument software without depending on a particular observability backend.

---

# OpenTelemetry SDK

The SDK is responsible for turning API calls into real telemetry.

Its responsibilities include:

- Creating telemetry signals
- Applying sampling policies
- Attaching metadata
- Managing resource attributes
- Exporting telemetry

Unlike the API, the SDK performs the actual telemetry generation.

In most production deployments, applications include both the API and the SDK.

---

# OpenTelemetry Collector

The Collector is one of the most important components in the OpenTelemetry ecosystem.

Instead of sending telemetry directly from applications to multiple backends, applications send telemetry to the Collector.

The Collector becomes the central telemetry pipeline.

Its responsibilities include:

- Receiving telemetry
- Processing telemetry
- Filtering unnecessary data
- Enriching metadata
- Batching requests
- Routing telemetry
- Exporting telemetry

This architecture simplifies application configuration and centralizes telemetry management.

---

# Why the Collector Matters

Without a Collector, every application must know where telemetry should be sent.

For example:

```
Application A ─────► Prometheus
Application B ─────► Loki
Application C ─────► Tempo
Application D ─────► Vendor Backend
```

Every backend change requires updating multiple applications.

With a Collector:

```
Applications
      │
      ▼
OpenTelemetry Collector
      │
 ┌────┼───────────┐
 ▼    ▼           ▼
Prometheus Loki Tempo
```

Applications export telemetry once.

The Collector decides where that telemetry should go.

This significantly reduces operational complexity.

---

# Collector Pipeline

Internally, the Collector processes telemetry using a pipeline.

```
Receiver

     │

     ▼

Processor

     │

     ▼

Exporter
```

Each stage has a specific responsibility.

## Receivers

Receivers accept telemetry from external sources.

Examples include:

- OTLP
- Prometheus
- Jaeger
- Zipkin

---

## Processors

Processors modify telemetry before export.

Common tasks include:

- Filtering
- Sampling
- Batching
- Attribute enrichment
- Resource detection

Processors improve efficiency while reducing storage costs.

---

## Exporters

Exporters send telemetry to external systems.

Examples include:

- Prometheus
- Loki
- Tempo
- Elasticsearch
- Datadog
- Splunk
- New Relic

Because exporters are interchangeable, organizations can adopt new backends without modifying application code.

---

# OTLP (OpenTelemetry Protocol)

OTLP is the standard protocol used by OpenTelemetry to transport telemetry.

It provides a unified mechanism for transmitting:

- Metrics
- Logs
- Traces

Using a single protocol simplifies interoperability between applications, collectors, and observability platforms.

OTLP supports multiple transport mechanisms, including gRPC and HTTP, allowing organizations to choose the option that best fits their environment.

---

# Signals Supported by OpenTelemetry

OpenTelemetry standardizes the generation and transport of the three primary observability signals.

## Metrics

Metrics represent numerical measurements collected over time.

Typical examples include:

- CPU utilization
- Memory consumption
- Request rate
- Error rate
- Response latency

Metrics are optimized for trend analysis, alerting, and long-term capacity planning.

---

## Logs

Logs record discrete events that occur during application execution.

Examples include:

- User authentication
- Service startup
- Configuration changes
- Exceptions
- Database errors

Logs provide detailed context that complements metrics.

---

## Traces

Traces describe the complete lifecycle of a request as it moves through multiple services.

Each trace consists of one or more spans representing individual operations.

Traces answer questions such as:

- Which service introduced latency?
- Where did the request fail?
- How long did each operation take?

Distributed tracing is one of the defining capabilities of modern observability platforms.

---

# Auto Instrumentation vs Manual Instrumentation

OpenTelemetry supports two approaches to instrumentation.

## Auto Instrumentation

Auto instrumentation generates telemetry without modifying application source code.

Advantages include:

- Fast deployment
- Minimal developer effort
- Consistent baseline telemetry

Limitations include:

- Limited business context
- Reduced customization
- Framework-dependent support

Auto instrumentation is an excellent starting point for existing applications.

---

## Manual Instrumentation

Manual instrumentation requires developers to explicitly define what telemetry should be generated.

Examples include:

- Business transactions
- Payment processing
- Inventory updates
- Customer workflows

Advantages include:

- Rich business context
- Precise telemetry
- Better debugging capabilities

Limitations include:

- Additional development effort
- Requires engineering discipline

Most mature organizations combine both approaches.

---

# Vendor Neutrality

One of OpenTelemetry's primary design goals is vendor neutrality.

Applications generate telemetry once.

Multiple backends can consume the same telemetry without requiring changes to application code.

```
Application
      │
      ▼
OpenTelemetry
      │
 ┌────┼───────────────┐
 ▼    ▼               ▼
Grafana Cloud   Datadog   Elastic
```

This architecture enables organizations to:

- Avoid vendor lock-in
- Compare observability platforms
- Migrate incrementally
- Build hybrid observability architectures

Vendor neutrality protects long-term engineering investments.

---

# Benefits of OpenTelemetry

Organizations adopting OpenTelemetry typically gain:

- Standardized instrumentation
- Consistent telemetry formats
- Easier platform migrations
- Reduced operational complexity
- Improved interoperability
- Strong CNCF ecosystem support
- Broad language and framework compatibility

The greatest benefit is architectural flexibility rather than any individual feature.

---

# Production Considerations

Adopting OpenTelemetry is not simply a tooling decision.

A successful deployment requires thoughtful planning around performance, scalability, governance, and operational consistency.

---

## Standardize Instrumentation

Different teams should follow the same instrumentation guidelines.

Organizations should standardize:

- Naming conventions
- Resource attributes
- Span attributes
- Metric names
- Log structure

Standardization improves telemetry quality and simplifies cross-team operations.

---

## Centralize Telemetry Collection

Applications should not communicate directly with multiple observability backends.

Instead, use the OpenTelemetry Collector as the central telemetry gateway.

Benefits include:

- Simplified application configuration
- Centralized processing
- Easier backend migration
- Better security controls
- Reduced operational complexity

---

## Control Telemetry Cost

Telemetry volume grows rapidly.

Without proper controls, storage and infrastructure costs can become significant.

Common optimization techniques include:

- Sampling traces
- Filtering unnecessary logs
- Aggregating metrics
- Defining appropriate retention policies

Observability should provide value without generating excessive operational cost.

---

## Monitor the Observability Platform

The observability platform itself is production infrastructure.

Organizations should monitor:

- Collector availability
- Export failures
- Queue sizes
- Processing latency
- Dropped telemetry
- Resource utilization

An unhealthy telemetry pipeline reduces confidence in operational data.

---

# Common Anti-patterns

## Instrumenting Everything

More telemetry does not always produce better observability.

Excessive instrumentation increases:

- Storage costs
- Query complexity
- Operational noise

Telemetry should be intentional.

---

## Skipping the Collector

Sending telemetry directly from applications to multiple backends tightly couples applications to observability infrastructure.

This makes future migrations and platform evolution more difficult.

---

## Inconsistent Resource Attributes

Different naming conventions across teams make telemetry difficult to search and correlate.

Consistency is essential for large-scale observability platforms.

---

## Treating OpenTelemetry as a Monitoring Tool

OpenTelemetry does not replace Prometheus, Grafana, Loki, or Tempo.

It provides a standard way to generate and transport telemetry.

Storage, visualization, and alerting remain the responsibility of downstream platforms.

---

# Best Practices

- Adopt OpenTelemetry early in the application lifecycle.
- Use the Collector as the central telemetry pipeline.
- Standardize instrumentation across teams.
- Prefer open standards over vendor-specific SDKs.
- Correlate metrics, logs, and traces using Trace IDs.
- Monitor the health of the telemetry pipeline itself.
- Regularly review telemetry quality and operational cost.

---

# Key Takeaways

- OpenTelemetry is the industry standard for telemetry generation and transport.
- It separates application instrumentation from observability backends.
- The Collector centralizes telemetry processing and routing.
- OTLP provides a standardized transport protocol.
- Vendor neutrality reduces long-term migration costs.
- Effective observability depends on engineering practices as much as technology.

---

# Looking Ahead

This document introduced the architecture and responsibilities of OpenTelemetry.

The next document explores how engineers instrument applications to produce meaningful telemetry using these concepts.

---

# References

## Official Documentation

- OpenTelemetry Documentation
- OpenTelemetry Specification
- CNCF OpenTelemetry Project

## Industry Resources

- Google Site Reliability Engineering
- The Site Reliability Workbook
- Observability Engineering – Charity Majors, Liz Fong-Jones, George Miranda

## Further Reading

- OpenTelemetry Collector Documentation
- OpenTelemetry Semantic Conventions
- Grafana Labs Engineering Blog
- Honeycomb Engineering Blog
