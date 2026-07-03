# ADR 0003 — Event-driven integration with Kafka

**Status:** Accepted

## Context

Placing an order must trigger several downstream actions — reserve stock, take payment, create a shipment
or assign a rider, notify the customer. Doing all of this synchronously would make order intake slow and
fragile: if any downstream service is down, the order fails.

## Decision

Integrate services **asynchronously through Apache Kafka**. Services publish domain events
(`OrderPlaced`, `PaymentCompleted`, `OrderPrepared`, `Delivered`, …) and interested services subscribe.
Synchronous REST is reserved for genuine request/response needs (reading a catalog, initiating a payment).

## Consequences

**Positive**

- **Resilience:** order intake succeeds even if a consumer is temporarily down; events are processed when it recovers.
- **Scalability:** consumers scale independently via partitions/consumer groups.
- **Extensibility:** new consumers (e.g. analytics, loyalty) subscribe without changing producers.
- **Real-time tracking:** the event stream naturally powers live order status.

**Negative / trade-offs**

- Eventual consistency and out-of-order/duplicate handling → consumers are **idempotent**.
- Requires schema/versioning discipline for event contracts.
- More moving parts to operate and monitor.
