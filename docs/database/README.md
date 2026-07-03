# Database Design

Both platforms follow **database-per-service**: every microservice owns its schema, and no other service
reads or writes it directly. Services reference data owned by others **by ID only** (there are no
cross-database foreign keys); such references are marked *"ref &lt;service&gt;"* in the diagrams below.

- Primary datastore: **PostgreSQL** (one logical database / schema per service)
- Ephemeral state: **Redis** (carts, sessions, hot catalog data)
- Consistency across services is achieved through **Kafka events**, not shared tables
  (see [Architecture Decisions](../decisions/0002-database-per-service.md)).

## Diagrams

- [E-commerce data model](ecommerce-erd.md)
- [Food-ordering data model](food-ordering-erd.md)

## Conventions

| Convention         | Notes                                                                          |
|--------------------|--------------------------------------------------------------------------------|
| Keys               | Surrogate `bigint` primary keys (`PK`); intra-service foreign keys marked `FK` |
| Cross-service refs | Stored as plain IDs, annotated `"ref <service>"`, never enforced by FK         |
| Money              | `numeric` amount + ISO `currency` code                                         |
| Timestamps         | UTC `timestamp`, `created_at` / `updated_at` audit columns                     |
| Status fields      | Modelled as enumerated `string` status columns driving the state machine       |
