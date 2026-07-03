# System Overview

Both platforms — the online grocery **e-commerce** store and the **food-ordering & delivery** app — share the
same cloud-native, event-driven microservices backbone. Clients talk only to an **API Gateway**, which
authenticates requests (JWT) and routes them to the responsible service. Services communicate **synchronously
over REST (Feign)** for request/response needs and **asynchronously over Apache Kafka** for domain events. Each
service owns its **own PostgreSQL database** (database-per-service).

## Context

![System context](diagrams/system-context.svg)

<details>
<summary>Diagram source (Mermaid)</summary>

```mermaid
flowchart LR
    C(["Customer"])
    V(["Store / Restaurant Admin"])
    R(["Delivery Rider"])

    SYS["mycodelogics Platform<br/>e-commerce (grocery) · food ordering"]

    PAYG["Payment Gateway · Razorpay"]
    MSG["Email / SMS Provider"]
    MAPS["Maps / Geocoding"]

    C -->|browse, order, pay, track| SYS
    V -->|manage catalog / orders / fulfilment| SYS
    R -->|update deliveries| SYS
    SYS -->|charge / refund| PAYG
    SYS -->|notifications / OTP| MSG
    SYS -->|routing / ETA| MAPS
```
</details>

## Microservices backbone

![Microservices backbone](diagrams/system-backbone.svg)

<details>
<summary>Diagram source (Mermaid)</summary>

```mermaid
flowchart TB
    subgraph Clients
        WEB["Web App · React 18 + Vite"]
        MOB["Mobile App · React Native / Expo"]
    end

    GW["API Gateway · Spring Cloud Gateway :8080<br/>routing · JWT auth · rate limiting · circuit breakers"]

    SVCS["Business microservices · Spring Boot<br/>one capability + one PostgreSQL DB each<br/>(see per-platform service maps)"]

    CFG["Config Server"]
    ADM["Spring Boot Admin"]

    KAFKA{{"Apache Kafka — event bus"}}

    PG[("PostgreSQL<br/>database-per-service")]
    REDIS[("Redis<br/>catalog cache · rate-limit")]
    EXT["External providers<br/>Razorpay · Email/SMS · Maps"]

    WEB --> GW
    MOB --> GW
    GW --> SVCS
    SVCS -. publish / consume .- KAFKA
    SVCS --- PG
    SVCS --- REDIS
    SVCS --> EXT
    CFG -. serves config .- SVCS
    ADM -. monitors .- SVCS
```
</details>

The backbone is deliberately shown at the pattern level; the **actual services, ports and events** for each
platform are documented in their service maps:
[E-commerce](ecommerce-architecture.md) · [Food ordering](food-ordering-architecture.md).

## Why this shape

- **Single entry point.** The gateway centralises cross-cutting concerns — authentication, routing, Redis-backed
  rate limiting and per-route circuit breaking — so individual services stay focused on business logic.
- **REST where it must be synchronous** (e.g. reading the catalog, reserving stock at checkout), **events where
  it should be asynchronous** (e.g. an order emitting `order-placed`, which payment, shipping and customer
  services react to independently and in parallel).
- **Database-per-service.** No shared schema; services stay independently deployable and cross-service data is
  referenced by ID only, never by a cross-database join.
- **Stateless services** behind the gateway scale horizontally; **Redis** backs the product-catalog cache and
  gateway rate-limiting, and **Kafka** absorbs load spikes so a slow downstream service never blocks intake.
- **Containerised with Docker** and deployed to cloud infrastructure, with **Config Server** supplying
  centralised configuration and **Spring Boot Admin** providing health and metrics monitoring.
