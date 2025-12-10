# EventEmitter (Node.js)

## 1. Formal Definition
**EventEmitter** is a core Node.js class (from the `events` module) that implements the **publish–subscribe (observer) pattern**.
It allows objects to emit named events and allows other objects to subscribe (listen) to those events with callback functions.

In simple terms:
> An EventEmitter lets one part of the system say “something happened” without knowing who cares about it.

---

## 2. Why It Matters for Backend Development
In backend systems, EventEmitter is widely used to:
- Decouple different parts of the system
- Handle asynchronous workflows
- React to state changes without tight coupling
- Build extensible and pluggable architectures

Many Node.js core components rely on EventEmitter:
- HTTP servers
- Streams
- Process lifecycle events
- Database and message clients

---

## 3. Conceptual Model
EventEmitter follows a simple model:

1. An object **emits** an event with a name (string or symbol)
2. Zero or more listeners are **registered** for that event
3. When the event is emitted, all listeners are executed **synchronously** in the order they were registered

Key characteristics:
- Emission is synchronous
- Listeners are called in order
- One-to-many communication
- Loose coupling between producer and consumers

---

## 4. Good Example: Using EventEmitter Correctly

```js
const EventEmitter = require("events");

class OrderService extends EventEmitter {
  createOrder(order) {
    // business logic
    this.emit("orderCreated", order);
  }
}

const orderService = new OrderService();

orderService.on("orderCreated", (order) => {
  console.log("Send confirmation email for", order.id);
});

orderService.on("orderCreated", (order) => {
  console.log("Update analytics for", order.id);
});

orderService.createOrder({ id: 123 });
```

---

## 5. Bad Example: Misusing EventEmitter

```js
const EventEmitter = require("events");

const emitter = new EventEmitter();

emitter.on("data", () => {
  while (true) {} // blocks forever
});

emitter.emit("data");
console.log("This will never run");
```

---

## 6. Common Misconceptions

### “EventEmitter is async by default”
Wrong. Emitting an event is synchronous.

### “EventEmitter replaces message queues”
Wrong. It is in-process only.

---

## 7. Good vs Bad Usage Patterns

✅ Domain events  
✅ Plugin systems  

❌ Cross-service communication  
❌ Long blocking listeners  

---

## 8. Relation to Other Backend Concepts
- Event Loop
- Streams
- Pub/Sub systems
- Logging hooks

---

## 9. Common Backend Mistakes
- Memory leaks from unremoved listeners
- Ignoring `error` events
- Using global emitters

---

## 10. Interview Questions
1. Is EventEmitter synchronous?
2. What happens if an `error` event has no listener?
3. How does EventEmitter differ from a message queue?

---

## 11. Answer Template for Interviews
> “EventEmitter implements publish–subscribe for in-process communication.
> Emissions are synchronous, so blocking listeners can block the event loop.
> It is ideal for decoupling internal modules but not for distributed messaging.”

---

## 12. One-Minute Summary
- Publish–subscribe
- In-process only
- Synchronous execution
- Powerful but easy to misuse
