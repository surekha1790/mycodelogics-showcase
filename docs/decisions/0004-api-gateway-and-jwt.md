# ADR 0004 — API Gateway with JWT authentication

**Status:** Accepted

## Context

With many services, clients shouldn't call each one directly, and every service shouldn't reimplement
authentication, TLS and rate limiting. Cross-cutting concerns need a single, consistent home.

## Decision

Route all client traffic through a **Spring Cloud Gateway**. It handles routing, TLS termination, rate
limiting and **JWT authentication**: the Auth service issues stateless JWTs on login, the gateway validates
them, and downstream services trust the propagated identity and enforce role-based access.

## Consequences

**Positive**

- One entry point; consistent security and routing.
- **Stateless auth** (JWT) scales horizontally with no shared session store.
- Services stay focused on business logic.

**Negative / trade-offs**

- The gateway is a critical path → deployed redundantly and monitored.
- JWT revocation needs care → short token lifetimes plus refresh tokens.
