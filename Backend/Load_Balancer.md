# Load Balancer (Backend)

## 1. Formal Definition
A **Load Balancer** is a component that distributes incoming network traffic across multiple backend servers.
Its goal is to ensure **high availability, scalability, and reliability**.

In simple terms:
> A load balancer decides which server should handle each incoming request.

---

## 2. Why It Matters for Backend Development
Load balancers are critical because they:
- Prevent single-server overload
- Improve system availability
- Enable horizontal scaling
- Allow rolling deployments with minimal downtime

Without a load balancer, backend systems become fragile and hard to scale.

---

## 3. How Load Balancing Works
Typical flow:
1. Client sends request
2. Load balancer receives request
3. Load balancer selects a healthy backend instance
4. Request is forwarded
5. Response flows back through the load balancer

Clients usually never know which backend handled the request.

---

## 4. Common Load Balancing Algorithms
Common strategies include:
- **Round Robin**: requests distributed evenly
- **Least Connections**: send to least busy server
- **Weighted Round Robin**: uneven capacity handling
- **IP Hash**: same client → same server

Algorithm choice affects performance and fairness.

---

## 5. Good Example: Stateless Backend + Load Balancer
```text
Client → Load Balancer → App Instance A
                      → App Instance B
                      → App Instance C
```

Why this is good:
- Any instance can serve any request
- Easy horizontal scaling
- Failures isolated
- Works well with containerized systems

---

## 6. Bad Example: Stateful Backend Without Strategy
Problems when:
- Sessions stored in memory
- Requests require same server
- No session replication

Result:
- Sticky sessions required
- Poor fault tolerance
- Limited scalability

Stateless services work best with load balancers.

---

## 7. Load Balancer Types
- **Layer 4 (Transport)**: TCP/UDP based
- **Layer 7 (Application)**: HTTP-aware

Layer 7 load balancers can:
- Inspect headers
- Route by path
- Terminate TLS

---

## 8. Health Checks
Load balancers rely on health checks to:
- Detect unhealthy servers
- Remove failed instances
- Restore healthy ones automatically

Poor health checks lead to traffic being sent to broken servers.

---

## 9. Common Backend Mistakes
- Storing state in application memory
- Ignoring health check design
- Using single load balancer without redundancy
- Not handling client IP forwarding
- Overusing sticky sessions

---

## 10. Interview Questions You Should Expect
1. What is a load balancer?
2. Why are load balancers important?
3. Layer 4 vs Layer 7 load balancing?
4. What is round robin?
5. How do health checks work?
6. What are sticky sessions?
7. How do load balancers affect scalability?
8. How do you deploy with zero downtime?
9. Can load balancers improve security?
10. Single vs multi-load balancer setups?

---

## 11. Answer Template for Interviews
> “A load balancer distributes incoming traffic across multiple backend servers.
> It improves availability, scalability, and fault tolerance.
> Modern backend systems rely on stateless services behind load balancers to scale horizontally.”

---

## 12. One-Minute Summary
- Load balancers distribute traffic
- Enable horizontal scaling
- Improve availability
- Work best with stateless services
- Core component of production systems
