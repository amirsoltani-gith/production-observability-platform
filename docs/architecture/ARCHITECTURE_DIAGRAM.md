# Architecture Diagram

## Overview

This document provides a high-level view of the Production Observability Platform.

The architecture is organized into four logical layers:

1. Workloads
2. Telemetry Collection
3. Observability Platform
4. Visualization

The goal is to maintain clear separation of responsibilities while enabling end-to-end observability.

---

## High-Level Architecture

```mermaid
flowchart TB
    subgraph Internet
        User[User]
    end
    subgraph Edge
        Traefik[Traefik]
    end
    subgraph Workloads
        SampleApp[Sample Application]
        WordPress[WordPress]
        Gitea[Gitea]
        PostgreSQL[(PostgreSQL)]
        MariaDB[(MariaDB)]
        Redis[(Redis)]
    end
    subgraph Collection
        NodeExporter[Node Exporter]
        cAdvisor[cAdvisor]
        Blackbox[Blackbox Exporter]
        OTel[OpenTelemetry Collector]
    end
    subgraph Observability
        Prometheus[Prometheus]
        Loki[Loki]
        Tempo[Tempo]
        Alertmanager[Alertmanager]
    end
    subgraph Visualization
        Grafana[Grafana]
    end
    User --> Traefik
    Traefik --> SampleApp
    Traefik --> WordPress
    Traefik --> Gitea
    SampleApp --> PostgreSQL
    WordPress --> MariaDB
    SampleApp --> Redis
    NodeExporter --> Prometheus
    cAdvisor --> Prometheus
    Blackbox --> Prometheus
    SampleApp --> OTel
    OTel --> Tempo
    Prometheus --> Grafana
    Loki --> Grafana
    Tempo --> Grafana
    Prometheus --> Alertmanager

```

---

## Architecture Layers

### Edge Layer

Handles incoming traffic and routes requests to internal services.

Current component:

* Traefik

---

### Workload Layer

Represents the applications and databases that generate operational telemetry.

These services are the primary observation targets.

---

### Telemetry Collection Layer

Collects infrastructure and application telemetry.

Responsibilities include:

* Metrics collection
* Infrastructure monitoring
* Trace collection
* Service availability checks

---

### Observability Platform Layer

Stores and processes telemetry.

Each service has a dedicated responsibility:

* Prometheus → Metrics
* Loki → Logs
* Tempo → Traces
* Alertmanager → Alerts

---

### Visualization Layer

Grafana provides a unified operational interface.

Operators should be able to move seamlessly between metrics, logs, traces, and alerts.

---

## Design Goals

The architecture prioritizes:

* Modularity
* Scalability
* Replaceable components
* Open standards
* Operational simplicity
* Loose coupling

---

## Related Documents

- [System Overview](SYSTEM_OVERVIEW.md)
- [Components](COMPONENTS.md)
- [Data Flow](DATA_FLOW.md)
- [ADR-001](../adr/ADR-001-platform-architecture.md)
