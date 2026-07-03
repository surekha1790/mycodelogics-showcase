# MyCodeLogics — Platform Engineering Showcase

> Backend architecture and system design documentation for two production platforms we designed and built:
> an **online grocery e-commerce platform** and a **food-ordering & delivery platform**.
>
> This repository is **documentation only**. The application source code is private. The goal here is to
> show *how* the systems are designed — architecture, service boundaries, event flows, data models and the
> engineering decisions behind them.

**Authors:**

**Surekha Garre** - 11+ years of experience · Backend Engineers · Berlin, Germany
- [Surekha Garre — LinkedIn](https://www.linkedin.com/in/surekha-garre-b725691b4)

**Sai Sarath Allada** — 9+ years of experience · Backend Engineers · Berlin, Germany
- [Sai Sarath Allada — LinkedIn](https://www.linkedin.com/in/sai-sarath-allada-505b33202)
---

## Live demos

| Platform                 | Live app                                 | Domain                         |
|--------------------------|------------------------------------------|--------------------------------|
| E-commerce - Customer    | https://app.ecommerce.mycodelogics.com   | Online grocery store           |
| E-commerce - Admin       | https://admin.ecommerce.mycodelogics.com | Admin Operations               |
| Food ordering - Customer | https://app.food.mycodelogics.com        | Restaurant ordering & delivery |
| Food ordering - Admin    | https://admin.food.mycodelogics.com      | Admin Operations               |

---

## What these platforms do

**E-commerce** — a single-company online grocery store selling groceries, vegetables and fruits, where
customers browse the product catalog, manage a cart, place orders, pay online and track fulfilment; the
company's staff manage products and inventory; the system handles pricing, stock reservation, payments and
notifications.

**Food ordering** — customers browse nearby restaurants and menus, place orders, pay, and track the order
from *accepted → prepared → out for delivery → delivered* in near real time, with rider assignment and
delivery tracking.

Both are built on the same backend backbone: **Spring Boot, Microservices**, communicating over
**REST** (synchronous) and **Apache Kafka** (asynchronous, event-driven), each service owning its own
**PostgreSQL** database, containerised with **Docker** and deployed to **Hetzner Cloud**.

---

## Tech stack

| Area                       | Technologies                                                                         |
|----------------------------|--------------------------------------------------------------------------------------|
| Language                   | Java 21                                                                              |
| Frameworks                 | Spring Boot, Spring Data JPA, Hibernate, Spring Security (JWT), Spring Cloud Gateway |
| Messaging / streaming      | Apache Kafka (event-driven architecture)                                             |
| Databases                  | PostgreSQL (database-per-service), Redis (cache / cart / sessions)                   |
| API                        | REST, OpenAPI / Swagger                                                              |
| Frontend                   | React (Vite, TypeScript), HTML, CSS; React Native (Expo) mobile app                  |
| Testing                    | JUnit, Mockito, Cucumber, TestNG                                                     |
| Build                      | Maven, Gradle                                                                        |
| Containers & orchestration | Docker                                                                               |
| Cloud                      | Hetzner Cloud Platform                                                               |

---

## Documentation index

| Section                                                                         | Contents                                               |
|---------------------------------------------------------------------------------|--------------------------------------------------------|
| [Architecture — system overview](docs/architecture/system-overview.md)          | High-level context & the shared microservices backbone |
| [Architecture — e-commerce](docs/architecture/ecommerce-architecture.md)        | Service map, responsibilities and communication        |
| [Architecture — food ordering](docs/architecture/food-ordering-architecture.md) | Service map, responsibilities and communication        |
| [Database design](docs/database/README.md)                                      | ER diagrams and schema notes (database-per-service)    |
| [Key flows](docs/flows/README.md)                                               | Sequence diagrams for the main user journeys           |
| [API overview](docs/api/api-overview.md)                                        | Representative REST endpoints per service              |
| [Architecture decisions](docs/decisions/README.md)                              | ADRs — the reasoning behind key choices                |

---

## Engineering highlights

- **Microservices with clear boundaries** — each service owns a single business capability and its own data.
- **Event-driven design** — Kafka decouples services so orders, payments, inventory and notifications react
  to events asynchronously, improving resilience and scalability.
- **Database-per-service** — no shared database; services stay independently deployable.
- **API gateway + JWT security** — a single entry point handles routing, authentication and rate limiting.
- **Containerised & cloud-native** — Docker images orchestrated on Hetzner Cloud.
- **Test-driven** — unit tests (JUnit/Mockito) and behaviour tests (Cucumber) across services.

---

## Note on scope

All diagrams describe the *architecture and design* of privately-hosted systems. No proprietary source code,
credentials or client-specific data is included in this repository.
