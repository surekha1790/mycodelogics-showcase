# ADR 0001 — Microservices over a monolith

**Status:** Accepted

## Context

The platforms have distinct capabilities (catalog, cart, orders, payments, inventory, delivery) with very
different scaling and change profiles. Catalog reads are high-volume and read-heavy; payments are low-volume
but sensitive; delivery tracking is write-heavy and real-time. A single deployable would couple these
together and force the whole system to scale and release as one unit.

## Decision

Decompose each platform into independently deployable **Spring Boot microservices**, one per business
capability, behind an API Gateway.

## Consequences

**Positive**

- Services scale independently (e.g. scale Catalog and Delivery without touching Payments).
- Independent deployments and smaller, safer releases.
- Clear ownership boundaries and focused, testable codebases.

**Negative / trade-offs**

- More operational complexity (many services, network calls, distributed tracing needed).
- Requires disciplined contracts and handling of eventual consistency.
- Mitigated with containerisation, CI/CD automation and centralised observability.
