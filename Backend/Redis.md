# Redis (Backend)

## 1. Formal Definition
**Redis** is an in-memory data store commonly used as a **cache, key–value store, message broker, and fast data structure server**.
It prioritizes speed by keeping data primarily in memory while offering optional persistence.

In simple terms:
> Redis is a very fast in-memory database used to speed up backend systems.

---

## 2. Why It Matters for Backend Development
Redis is important because it:
- Provides extremely low-latency data access
- Offloads databases under high load
- Supports multiple data structures
- Enables distributed system patterns

Redis is widely used for:
- Caching
- Session storage
- Rate limiting
- Pub/Sub
- Distributed locks

---

## 3. Core Redis Data Structures
Redis supports rich data structures:
- **String**: simple key-value
- **Hash**: object-like structures
- **List**: ordered collections
- **Set**: unique unordered values
- **Sorted Set**: ordered by score
- **Bitmap / HyperLogLog**: specialized use cases

Choosing the right structure is critical for performance.

---

## 4. Good Example: Using Redis as a Cache

```js
const cached = await redis.get(`user:${id}`);
if (cached) return JSON.parse(cached);

const user = await db.getUser(id);
await redis.setEx(`user:${id}`, 60, JSON.stringify(user));
return user;
```

Why this is good:
- Database load reduced
- Fast reads
- TTL prevents stale data
- Clear key naming convention

---

## 5. Bad Example: Treating Redis as Primary Database

```js
await redis.set("order:123", JSON.stringify(order));
// no persistence strategy
```

Problems:
- Data loss on restart (without persistence)
- No strong durability guarantees
- Harder data consistency
- Redis not designed for complex queries

Redis should not replace primary databases.

---

## 6. Persistence and Durability
Redis offers optional persistence:
- **RDB snapshots**: periodic backups
- **AOF (Append Only File)**: write-ahead logging
- **Hybrid**: combines both

Trade-off:
- More durability = slower writes
- Less durability = faster performance

---

## 7. Redis vs Traditional Databases

| Aspect | Redis | Relational DB |
|---|---|---|
| Storage | In-memory | Disk-based |
| Speed | Very fast | Slower |
| Querying | Limited | Rich |
| Durability | Optional | Strong |

Redis complements databases; it does not replace them.

---

## 8. Common Redis Use Cases
- Caching hot data
- Session storage
- Rate limiting counters
- Job queues
- Distributed locks

Redis often sits between application and database.

---

## 9. Common Backend Mistakes
- No TTL on cached keys
- Key naming collisions
- Treating Redis as durable storage
- Not handling Redis outages
- Large values stored unnecessarily

Redis failures must be tolerated gracefully.

---

## 10. Interview Questions You Should Expect
1. What is Redis?
2. Why is Redis fast?
3. Redis vs Memcached?
4. What data structures does Redis support?
5. How does Redis persistence work?
6. When should Redis not be used?
7. Redis as cache vs DB?
8. How do you handle Redis failures?
9. What is Redis TTL?
10. Common Redis use cases?

---

## 11. Answer Template for Interviews
> “Redis is an in-memory data store used for caching and fast data access.
> It significantly improves performance but is not a replacement for a primary database.
> Redis is commonly used alongside databases to scale backend systems.”

---

## 12. One-Minute Summary
- Redis is an in-memory data store
- Extremely fast
- Supports rich data structures
- Used for caching and coordination
- Complements, not replaces, databases
