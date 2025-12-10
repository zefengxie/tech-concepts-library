# Pub/Sub (Publish–Subscribe) (Backend)

## 1. Formal Definition
**Pub/Sub (Publish–Subscribe)** is a messaging pattern where publishers send events to a topic, and subscribers receive events they are interested in.
Publishers and subscribers are **decoupled** and do not know about each other directly.

In simple terms:
> Pub/Sub lets many consumers react to events without the producer knowing who they are.

---

## 2. Why It Matters for Backend Development
Pub/Sub is important because it:
- Enables event-driven architectures
- Allows fan-out (one event → many consumers)
- Improves scalability and extensibility
- Reduces tight coupling between services

It is common in:
- Microservices
- Real-time analytics
- Notifications
- Data pipelines

---

## 3. Core Pub/Sub Concepts
Key components:
- **Publisher**: emits events
- **Topic**: channel events are published to
- **Subscriber**: listens to a topic
- **Broker**: manages delivery (Kafka, Pub/Sub, SNS)

Events are immutable facts that something happened.

---

## 4. Good Example: Event Fan-Out

```js
// publisher
publish("order.created", { orderId: 123 });
```

```js
// subscribers
subscribe("order.created", sendEmail);
subscribe("order.created", updateAnalytics);
subscribe("order.created", notifyWarehouse);
```

Why this is good:
- Publisher is unaware of consumers
- New subscribers can be added easily
- Clear separation of responsibilities
- High scalability

---

## 5. Bad Example: Synchronous Fan-Out

```js
await sendEmail();
await updateAnalytics();
await notifyWarehouse();
```

Problems:
- Tight coupling
- Slow response times
- One failure blocks all
- Hard to evolve

Pub/Sub avoids synchronous dependencies.

---

## 6. Pub/Sub vs Message Queue

| Aspect | Pub/Sub | Message Queue |
|---|---|---|
| Consumers | Many | Typically one |
| Pattern | Broadcast | Work distribution |
| Coupling | Very low | Low |
| Use case | Events | Tasks |

Both patterns are often used together.

---

## 7. Delivery Guarantees
Delivery semantics vary by system:
- At-most-once
- At-least-once
- Exactly-once (rare)

Consumers must be **idempotent** in most systems.

---

## 8. Ordering and Replay
Considerations:
- Ordering is usually per partition or topic
- Replay allows reprocessing historical events
- Not all systems support replay equally

Kafka excels at ordering and replay.

---

## 9. Common Backend Mistakes
- Treating events as commands
- Putting business logic in publishers
- Large, unstable event schemas
- Ignoring idempotency
- No consumer monitoring

Event-driven systems require discipline.

---

## 10. Interview Questions You Should Expect
1. What is Pub/Sub?
2. Pub/Sub vs message queue?
3. What is a topic?
4. How do subscribers scale?
5. What delivery guarantees exist?
6. How do you handle duplicates?
7. What is event replay?
8. When should you use Pub/Sub?
9. Pub/Sub vs REST?
10. Common Pub/Sub pitfalls?

---

## 11. Answer Template for Interviews

> “Pub/Sub is a messaging pattern where publishers emit events to topics and subscribers consume them independently.
> It enables loose coupling and scalable fan-out.
> Pub/Sub is ideal for event-driven systems where multiple services react to the same event.”

---

## 12. One-Minute Summary

- Pub/Sub enables event-driven design
- Publishers and subscribers are decoupled
- Supports fan-out
- Consumers must handle duplicates
- Core pattern for modern backends
