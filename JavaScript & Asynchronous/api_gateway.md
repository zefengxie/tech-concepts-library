# API Gateway

## What an API Gateway Is

An API Gateway is a server that acts as a single entry point for clients to access multiple backend services.
It sits between clients and microservices, handling cross-cutting concerns centrally.

---

## Why API Gateways Exist

In distributed systems and microservices architectures, clients would otherwise need to:
- Know the location of every service
- Handle authentication for each service
- Manage retries, rate limits, and failures

An API Gateway provides:
- Unified access
- Centralized security
- Simplified client logic
- Better observability

---

## Core Responsibilities

### Request Routing
Routes incoming requests to the correct backend service.

```text
GET /users → User Service
GET /orders → Order Service
```

---

### Authentication and Authorization
Validates tokens, API keys, or sessions before forwarding requests.

---

### Rate Limiting and Throttling
Prevents abuse and protects backend services.

---

### Request and Response Transformation
- Modify headers
- Transform payload formats
- Aggregate responses from multiple services

---

### Load Balancing
Distributes traffic across multiple service instances.

---

### Caching
Caches responses to reduce backend load and latency.

---

## API Gateway vs Reverse Proxy

API Gateway:
- Application-aware
- Handles authentication, rate limiting, aggregation
- Designed for APIs

Reverse Proxy:
- Traffic-focused
- Often unaware of business logic
- Focuses on routing and load balancing

---

## API Gateway in Microservices

Without a gateway:
- Client talks to many services
- Complex client-side logic
- Tight coupling

With a gateway:
- Client talks to one endpoint
- Backend services evolve independently

---

## Aggregation Pattern

```text
Client → API Gateway
        ├─ User Service
        ├─ Order Service
        └─ Payment Service
```

The gateway combines responses into a single payload.

---

## Deployment Models

- Edge Gateway (close to clients)
- Internal Gateway (inside private network)
- Managed Gateway (cloud services)

Examples:
- AWS API Gateway
- Kong
- NGINX
- Envoy
- Apigee

---

## Performance Considerations

- Adds network hop
- Must be highly available
- Can become a bottleneck if overloaded

Mitigations:
- Horizontal scaling
- Caching
- Circuit breakers

---

## Security Considerations

- Validate all inputs
- Enforce TLS
- Centralize authentication logic
- Apply consistent security policies

---

## Common Mistakes

- Putting business logic in the gateway
- Over-aggregating responses
- Tight coupling between gateway and services
- Single point of failure without redundancy

---

## Interview Questions

- What problem does an API Gateway solve?
- API Gateway vs Reverse Proxy?
- What responsibilities belong in a gateway?
- Can an API Gateway become a bottleneck?
- How do you scale an API Gateway?

---

## Key Takeaways

- API Gateway is a unified entry point
- Centralizes cross-cutting concerns
- Simplifies clients
- Essential in microservices architectures
