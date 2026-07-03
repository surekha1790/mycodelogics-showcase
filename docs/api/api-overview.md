# API Overview

Representative REST endpoints exposed through the API Gateway. All are versioned under `/api/v1`, secured
with JWT (except registration/login), and documented per service with **OpenAPI / Swagger**.

## Common

| Method | Path | Description |
|---|---|---|
| POST | `/api/v1/auth/register` | Create an account |
| POST | `/api/v1/auth/login` | Authenticate, receive JWT |
| GET | `/api/v1/users/me` | Current user profile |

## E-commerce

| Method | Path | Description |
|---|---|---|
| GET | `/api/v1/products` | List / search / filter products |
| GET | `/api/v1/products/{id}` | Product detail |
| POST | `/api/v1/cart/items` | Add item to cart |
| GET | `/api/v1/cart` | View cart |
| POST | `/api/v1/orders` | Place an order |
| GET | `/api/v1/orders/{id}` | Order detail & status |
| POST | `/api/v1/payments` | Initiate payment |
| POST | `/api/v1/products/{id}/reviews` | Add a review |

## Food ordering

| Method | Path | Description |
|---|---|---|
| GET | `/api/v1/restaurants` | List nearby restaurants |
| GET | `/api/v1/restaurants/{id}/menu` | Restaurant menu |
| POST | `/api/v1/cart/items` | Add menu item to cart |
| POST | `/api/v1/orders` | Place a food order |
| GET | `/api/v1/orders/{id}/tracking` | Live order & delivery status |

## Conventions

- **Auth:** `Authorization: Bearer <JWT>`; roles enforced per endpoint.
- **Errors:** consistent JSON error body (`timestamp`, `status`, `error`, `message`, `path`).
- **Pagination:** `?page=&size=&sort=` on list endpoints.
- **Idempotency:** order and payment creation accept an `Idempotency-Key` header.
