# Caching (Backend)

## 1. Formal Definition
**Caching** is a technique where frequently accessed data is stored in a fast-access storage layer so future requests can be served faster without recomputing or re-fetching the data.

In simple terms:
> Caching saves results so you don’t have to do the same work repeatedly.

---

## 2. Why It Matters for Backend Development
Caching is critical because it:
- Improves response time
- Reduces database and service load
- Lowers infrastructure costs
- Enables systems to scale under high traffic

Many high-performance systems would collapse without caching.

---

## 3. Where Caching Can Happen
Caching exists at multiple layers:
- **Client-side cache** (browser, HTTP cache)
- **CDN cache**
- **Reverse proxy cache**
- **Application-level cache**
- **Database cache**

Effective systems use caching at multiple layers.

---

## 4. Good Example: Read-Through Cache

```js
async function getUser(id) {
  const cached = await cache.get(`user:${id}`);
  if (cached) return cached;

  const user = await db.findUser(id);
  await cache.set(`user:${id}`, user, 60);
  return user;
}
```

Why this is good:
- Cache hit avoids database call
- Cache miss transparently loads data
- TTL prevents stale data
- Clear cache key strategy

---

## 5. Bad Example: Cache Without Invalidation

```js
cache.set("users", users);
// never invalidated
```

Problems:
- Data becomes stale
- Bugs are hard to detect
- Inconsistent application state
- Incorrect business behavior

Cache invalidation is one of the hardest problems in backend engineering.

---

## 6. Cache Invalidation Strategies
Common strategies:
- **Time-based (TTL)**: expires after time
- **Write-through**: update cache on write
- **Write-behind**: async cache updates
- **Manual eviction**: explicit removal

Wrong invalidation causes correctness bugs.

---

## 7. Caching vs Persistence
Important distinction:

| Aspect | Cache | Database |
|---|---|---|
| Purpose | Speed | Durability |
| Data lifetime | Temporary | Permanent |
| Source of truth | No | Yes |

Caches should never be treated as the source of truth.

---

## 8. Common Caching Patterns
- Cache-aside (lazy loading)
- Read-through
- Write-through
- Write-behind

Cache-aside is the most common and safest.

---

## 9. Common Backend Mistakes
- Caching mutable data blindly
- No versioning in cache keys
- Storing huge objects in cache
- Forgetting cache eviction
- Assuming cache is always available

Cache failures must be handled gracefully.

---

## 10. Interview Questions You Should Expect
1. What is caching?
2. Why is caching important?
3. What is cache invalidation?
4. TTL vs manual eviction?
5. Cache-aside vs write-through?
6. Where should caching be applied?
7. What can go wrong with caching?
8. How does caching affect consistency?
9. What data should not be cached?
10. How do you test cache behavior?

---

## 11. Answer Template for Interviews
> “Caching stores frequently accessed data in a fast layer to reduce latency and load.
> It improves performance but introduces consistency challenges.
> Proper invalidation strategies are critical to correct backend behavior.”

---

## 12. One-Minute Summary
- Caching improves speed and scalability
- Exists at multiple layers
- Cache is not the source of truth
- Invalidation is hard
- Essential for high-performance systems
