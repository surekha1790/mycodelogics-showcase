# ADR 0002 — Database-per-service

**Status:** Accepted

## Context

Microservices need to be independently deployable. A shared database would recreate a hidden monolith:
schema changes would ripple across services and one team's migration could break another.

## Decision

Each service owns its **own PostgreSQL schema**. No service reads another's tables. Cross-service data is
referenced by ID and retrieved via that service's API or via events; consistency is maintained through
**Kafka events**, not foreign keys.

## Consequences

**Positive**

- Services evolve their schema independently.
- Strong encapsulation; the database is a private implementation detail.
- Enables per-service scaling and technology fit.

**Negative / trade-offs**

- No cross-service joins or ACID transactions spanning services.
- Requires **eventual consistency** and, where needed, the **saga / compensation** pattern
  (e.g. release stock and refund payment if an order is cancelled — see the order flow).
- Some data is duplicated across services by design.
