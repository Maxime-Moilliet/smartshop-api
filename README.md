# SmartShop API

> E-commerce REST API built with Spring Boot 3.x

[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.6-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-latest-blue.svg)](https://www.postgresql.org/)

## About the Project

**SmartShop API** is a Java/Spring Boot learning and practice project. The goal is to discover and master the Spring ecosystem through the creation of a complete e-commerce REST API.

## Table of Contents

- [Data Model](#data-model)
- [API Endpoints](#api-endpoints)
- [Security](#security)

## Data Model

### Entity-Relationship Diagram

```
┌─────────────────┐
│      USER       │
├─────────────────┤       1:N        ┌─────────────────┐
│ id (PK)         │─────────────────>│     ORDER       │
│ username        │                  ├─────────────────┤
│ email           │                  │ id (PK)         │
│ password        │                  │ user_id (FK)    │
│ role            │                  │ total_amount    │
│ created_at      │                  │ status          │
│ updated_at      │                  │ created_at      │
└─────────────────┘                  │ updated_at      │
        │                            └─────────────────┘
        │ 1:N                                 │
        │                                     │ 1:N
        ↓                                     ↓
┌─────────────────┐                  ┌─────────────────┐
│   CART_ITEM     │                  │   ORDER_ITEM    │
├─────────────────┤                  ├─────────────────┤
│ id (PK)         │                  │ id (PK)         │
│ user_id (FK)    │                  │ order_id (FK)   │
│ product_id (FK) │                  │ product_id (FK) │
│ quantity        │                  │ quantity        │
│ added_at        │                  │ unit_price      │
└─────────────────┘                  │ created_at      │
                                     └─────────────────┘
        │                                     │
        │ N:1                                 │ N:1
        ↓                                     ↓
┌─────────────────┐                          │
│    PRODUCT      │<─────────────────────────┘
├─────────────────┤
│ id (PK)         │
│ name            │
│ description     │
│ price           │
│ stock_quantity  │
│ category_id(FK) │
│ image_url       │
│ created_at      │
│ updated_at      │
└─────────────────┘
        │ N:1
        ↓
┌─────────────────┐
│    CATEGORY     │
├─────────────────┤
│ id (PK)         │
│ name            │
│ description     │
│ created_at      │
│ updated_at      │
└─────────────────┘
```

## API Endpoints

### Authentication (`/api/auth`)

| Method | Endpoint | Description | Auth |
|---------|----------|-------------|------|
| `POST` | `/api/auth/register` | Create a new account | Public |
| `POST` | `/api/auth/login` | Authentication and JWT token retrieval | Public |
| `GET` | `/api/auth/me` | Current user information | User |
| `POST` | `/api/auth/refresh` | JWT token renewal | User |

### Products (`/api/products`)

| Method | Endpoint | Description | Auth |
|---------|----------|-------------|------|
| `GET` | `/api/products` | Paginated list of products | Public |
| `GET` | `/api/products/{id}` | Product details | Public |
| `GET` | `/api/products/search` | Search products | Public |
| `POST` | `/api/products` | Create a product | Admin |
| `PUT` | `/api/products/{id}` | Update a product | Admin |
| `DELETE` | `/api/products/{id}` | Delete a product | Admin |
| `PATCH` | `/api/products/{id}/stock` | Update stock | Admin |

### Categories (`/api/categories`)

| Method | Endpoint | Description | Auth |
|---------|----------|-------------|------|
| `GET` | `/api/categories` | List all categories | Public |
| `GET` | `/api/categories/{id}` | Category details | Public |
| `POST` | `/api/categories` | Create a category | Admin |
| `PUT` | `/api/categories/{id}` | Update a category | Admin |
| `DELETE` | `/api/categories/{id}` | Delete a category | Admin |

### Cart (`/api/cart`)

| Method | Endpoint | Description | Auth |
|---------|----------|-------------|------|
| `GET` | `/api/cart` | User's cart contents | User |
| `POST` | `/api/cart/items` | Add an item to cart | User |
| `PUT` | `/api/cart/items/{productId}` | Update quantity | User |
| `DELETE` | `/api/cart/items/{productId}` | Remove an item | User |
| `DELETE` | `/api/cart` | Empty cart completely | User |

### Orders (`/api/orders`)

| Method | Endpoint | Description | Auth |
|---------|----------|-------------|------|
| `POST` | `/api/orders/checkout` | Convert cart to order | User |
| `GET` | `/api/orders` | User's order list | User |
| `GET` | `/api/orders/{id}` | Order details | User/Admin |
| `PATCH` | `/api/orders/{id}/status` | Update status | Admin |
| `DELETE` | `/api/orders/{id}` | Cancel an order | User/Admin |

### Administration (`/api/admin`)

| Method | Endpoint | Description | Auth |
|---------|----------|-------------|------|
| `GET` | `/api/admin/users` | List of all users | Admin |
| `GET` | `/api/admin/users/{id}` | User details | Admin |
| `PATCH` | `/api/admin/users/{id}/role` | Update user role | Admin |
| `GET` | `/api/admin/orders` | List of all orders | Admin |

## Security

### Roles and Permissions

| Role | Permissions |
|------|------------|
| **Guest (unauthenticated)** | View products and categories, Register, Login |
| **ROLE_USER (Customer)** | All guest permissions + Manage cart, Place orders, View own orders |
| **ROLE_ADMIN** | All permissions + CRUD products/categories, Manage all users, Manage all orders |

---

Made with ☕ by Maxime Moilliet
