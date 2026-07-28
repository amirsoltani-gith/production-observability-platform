# ADR-001: Adopt a Layered Observability Platform Architecture

- **Status:** Accepted
- **Date:** 2026-07-27
- **Decision Makers:** Project Maintainer

---

## Context

The goal of this project is to build a production-inspired observability platform that demonstrates modern Platform Engineering and Site Reliability Engineering (SRE) practices.

Rather than focusing on a single monitoring tool, the platform should provide a complete observability solution that can evolve over time.

The architecture should remain understandable, modular, and maintainable while supporting future expansion.

---

## Decision

The project adopts a layered architecture consisting of four logical layers:

1. Workloads
2. Telemetry Collection
3. Observability Platform
4. Visualization

Each layer has clearly defined responsibilities and communicates through standard interfaces.

Telemetry is separated into three independent domains:

* Metrics
* Logs
* Distributed Traces

These domains are unified through Grafana for day-to-day operations.

---

## Alternatives Considered

### Option 1 — Monitoring Only

Deploy only Prometheus and Grafana.

### Advantages

* Simple
* Quick to deploy

### Disadvantages

* No centralized logging
* No distributed tracing
* Limited operational visibility

---

## Option 2 — Monitoring + Logging

Deploy Prometheus, Grafana, and Loki.

### Advantages

* Better troubleshooting
* Centralized logs

### Disadvantages

* Missing distributed tracing
* Incomplete observability

---

## Option 3 — Complete Observability Platform (Selected)

Deploy a complete platform including:

* Prometheus
* Grafana
* Loki
* Tempo
* Alertmanager
* OpenTelemetry Collector
* Exporters

### Advantages

* Covers all telemetry domains
* Reflects modern production environments
* Scalable architecture
* Better educational value
* Strong portfolio demonstration

### Disadvantages

* Higher implementation complexity
* Larger operational footprint

---

## Consequences

### Positive outcomes:

* Clear separation of responsibilities
* Independent component evolution
* Better documentation
* Easier troubleshooting
* Architecture aligned with industry practices

### Trade-offs:

* Increased learning curve
* More services to maintain
* Additional documentation effort

---

### Rationale

The selected architecture balances realism with educational value.

Although it introduces additional complexity compared to a minimal monitoring stack, it better reflects how modern observability platforms are designed in production environments.

The layered approach also supports incremental development throughout future project sprints.

---

### Related Documents

- [System Overview](../architecture/SYSTEM_OVERVIEW.md)
- [Components](../architecture/COMPONENTS.md)
- [Data Flow](../architecture/DATA_FLOW.md)
- [Architecture Diagram](../architecture/ARCHITECTURE_DIAGRAM.md)
