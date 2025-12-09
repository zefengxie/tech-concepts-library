# Reverse Proxy

## What a Reverse Proxy Is

A reverse proxy is a server that sits between clients and backend servers.
Clients send requests to the reverse proxy, and the proxy forwards those requests to one or more backend servers.

Unlike a forward proxy, a reverse proxy is controlled by the server-side infrastructure, not the client.

---

## Why Reverse Proxies Exist

Reverse proxies solve several infrastructure problems:
- Hiding internal server architecture
- Load balancing traffic
- Improving security
- Improving performance
- Enabling TLS termination

They are a core component of modern web architectures.

---

## How a Reverse Proxy Works

1. Client sends a request to a public endpoint
2. Reverse proxy receives the request
3. Proxy selects a backend server
4. Proxy forwards the request
5. Backend responds to the proxy
6. Proxy returns the response to the client

The client never communicates directly with backend servers.

---

## Core Responsibilities

### Request Forwarding
Routes incoming traffic to internal services based on:
- URL path
- Hostname
- Headers

---

### Load Balancing
Distributes traffic across multiple servers to improve availability and performance.

Common strategies:
- Round-robin
- Least connections
- IP hash

---

### TLS Termination
Handles HTTPS encryption and decryption.

Benefits:
- Centralized certificate management
- Reduced load on backend servers

---

### Security Layer
Provides protection by:
- Hiding internal IP addresses
- Blocking malicious traffic
- Enforcing request limits
- Adding security headers

---

### Caching
Stores frequently accessed responses to reduce backend load.

---

## Reverse Proxy vs Forward Proxy

| Reverse Proxy | Forward Proxy |
|--------------|---------------|
| Server-side | Client-side |
| Protects servers | Protects clients |
| Hides backend | Hides client |
| Used by service owners | Used by end users |

---

## Reverse Proxy vs Load Balancer

Reverse proxy:
- Can load balance
- Handles HTTP-level logic
- Application-aware

Load balancer:
- Focused on traffic distribution
- Can operate at L4 or L7
- Often simpler

Many modern reverse proxies act as both.

---

## Reverse Proxy vs API Gateway

Reverse Proxy:
- Infrastructure-focused
- Routes and balances traffic
- Minimal business logic

API Gateway:
- Application-aware
- Handles authentication, rate limiting, aggregation

---

## Common Reverse Proxy Software

- NGINX
- Apache HTTP Server
- Envoy
- HAProxy
- Traefik
- Cloud provider edge proxies

---

## Typical Use Cases

- Web application frontends
- Microservices routing
- Blue-green deployments
- Canary releases
- Zero-downtime deployments

---

## Performance Considerations

- Adds an extra hop
- Improves performance through caching
- Must be horizontally scalable
- Requires monitoring and tuning

---

## Security Considerations

- Enforce HTTPS
- Validate headers
- Protect against request smuggling
- Limit request size
- Apply rate limits

---

## Common Mistakes

- Putting business logic in the proxy
- No redundancy
- Weak TLS configuration
- Misconfigured header forwarding
- Ignoring proxy timeouts

---

## Interview Questions

- What is a reverse proxy?
- Reverse proxy vs forward proxy?
- Reverse proxy vs API gateway?
- How does TLS termination work?
- How do reverse proxies improve security?
- Can a reverse proxy be a bottleneck?

---

## Key Takeaways

- Reverse proxy sits in front of backend servers
- Clients never see internal architecture
- Enables load balancing, security, and performance improvements
- Fundamental to scalable web systems
