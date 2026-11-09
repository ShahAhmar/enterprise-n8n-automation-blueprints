# ADR 0001: Self-Hosted n8n Orchestration Architecture for Enterprise Automation

## Status
Accepted

## Context & Problem Statement
Modern enterprise applications require asynchronous workflow orchestration across third-party SaaS APIs, internal databases, and LLM processing models. 

Relying entirely on custom microservice code for routine integrations (e.g., webhook routing, CRM enrichment, notification triggers) introduces high maintenance overhead, rigid deployment cycles, and scattered integration monitoring. Conversely, fully managed cloud iPaaS solutions (such as Zapier or Make) impose strict rate limits, high per-execution costs, and compliance risks regarding data residency.

## Decision
We adopted **Self-Hosted n8n** running on containerized infrastructure (Docker & PostgreSQL) as our centralized workflow automation engine.

Key architectural mechanics:
- Modular, JSON-serialized workflow definitions decoupled from the core application codebase.
- Dedicated PostgreSQL persistence ensuring workflow execution histories and state transitions survive container restarts.
- Asynchronous webhook triggers connecting external APIs with internal database services without blocking user-facing HTTP request cycles.

## Consequences

### Positive
- **Data Privacy & Compliance:** All webhook payloads and database queries execute inside our controlled infrastructure boundary (no third-party data egress).
- **Zero Execution Limits:** High-throughput batch processing without per-task metering fees.
- **Visual Observability:** Real-time visual monitoring of execution nodes, retry logic, and error boundaries.

### Negative / Trade-Offs
- Requires infrastructure management (monitoring Docker container health, PostgreSQL backups, and n8n version upgrades).
- Credential injection requires careful environment variable management and encrypted credential stores.
