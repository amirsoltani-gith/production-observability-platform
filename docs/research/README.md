# Research

## Purpose

The research section provides the technical foundation for this repository.

Its purpose is not to collect notes or summarize technologies.

Every research document exists to support future engineering decisions, architecture design, Architecture Decision Records (ADRs), implementation, and operational practices.

Research should always answer why a technology or concept exists before explaining how it works.

---

## Research Philosophy

This repository follows an engineering-first approach.

Research leads to Architecture.

Architecture leads to ADRs.

ADRs lead to Implementation.

Implementation leads to Operations.

The goal is to understand engineering trade-offs rather than memorize tools.

---

## Repository Organization

Research is organized by engineering domains instead of individual tools.

Current domains include:

- Fundamentals
- Monitoring
- Metrics
- Logging
- Tracing
- Alerting
- Reliability
- SRE
- Platform Engineering
- Capacity Planning
- Root Cause Analysis
- AI Operations

---

## Research Standards

Every research document should answer the following questions whenever applicable:

- What problem does this solve?
- Why does this exist?
- What are the alternatives?
- What are the advantages?
- What are the disadvantages?
- What operational complexity does it introduce?
- How does it scale?
- When should it be used?
- When should it not be used?
- What are the engineering trade-offs?

Research documents should prioritize engineering thinking over feature descriptions.

---

## Relationship to Architecture

Research findings provide the foundation for future architecture designs.

Architectural decisions should reference relevant research whenever possible.

---

## Relationship to ADRs

Major engineering decisions should be documented as ADRs.

Each ADR should be supported by previous research rather than personal preference.

---

## Relationship to Implementation

Implementation should validate research through practical, production-inspired examples.

No implementation should exist without a clear engineering purpose.

---

## Document Naming Convention

Documents use sequential numbering within each domain.

Example:

- 01-why-observability.md
- 02-monitoring-vs-observability.md
- 03-golden-signals.md

---

## Repository Workflow

Research

↓

Architecture

↓

ADR

↓

Implementation

↓

Operations

↓

Production Review