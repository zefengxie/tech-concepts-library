# Webhook (Backend)

## 1. Formal Definition
A **Webhook** is a mechanism that allows a system to **push real-time data** to another system when a specific event occurs.
Instead of polling an API, the receiving system exposes an HTTP endpoint that the sender calls automatically.

In simple terms:
> A webhook is an event-driven HTTP callback.

---

## 2. Why It Matters for Backend Development
Webhooks are important because they:
- Enable real-time integrations
- Reduce unnecessary polling
- Decouple systems using event-driven design
- Scale well for asynchronous workflows

Common use cases:
- Payment notifications (e.g., payment succeeded)
- CI/CD events (build completed)
- Third-party integrations (GitHub, Stripe, Slack)
- Data synchronization across systems

---

## 3. How Webhooks Work (Conceptual Flow)
Typical webhook flow:
1. Receiver registers an endpoint URL with the provider
2. An event occurs in the provider system
3. Provider sends an HTTP request (usually POST) to the endpoint
4. Receiver validates and processes the payload
5. Receiver returns a success response (2xx)

Webhooks rely on **push**, not pull.

---

## 4. Good Example: Receiving a Webhook Safely

```js
app.post("/webhooks/payment", express.json(), (req, res) => {
  const event = req.body;

  if (!verifySignature(req)) {
    return res.status(401).send("Invalid signature");
  }

  if (event.type === "payment.succeeded") {
    handlePayment(event.data);
  }

  res.status(200).send("ok");
});
```

Why this is good:
- Validates authenticity
- Handles specific event types
- Responds quickly
- Avoids blocking work in request handler

---

## 5. Bad Example: Unsafe Webhook Handling

```js
app.post("/webhook", (req, res) => {
  processEvent(req.body); // heavy work
  res.send("done");
});
```

Problems:
- No authentication or verification
- Heavy processing blocks request
- No idempotency handling
- Vulnerable to abuse or replay attacks

---

## 6. Security Considerations
Webhooks must be secured because:
- They are publicly exposed endpoints
- They can trigger sensitive actions

Best practices:
- Verify signatures or shared secrets
- Use HTTPS only
- Validate payload structure
- Enforce IP allowlists where possible
- Use idempotency keys

---

## 7. Reliability and Delivery Guarantees
Webhooks are inherently unreliable:
- Network failures can occur
- Receiver may be temporarily down

Common strategies:
- Provider retries on failure
- Receiver returns quick 2xx responses
- Asynchronous processing via queues
- Deduplication of repeated events

Webhooks should be **idempotent**.

---

## 8. Webhooks vs Polling vs Pub/Sub

| Mechanism | Model | Latency | Scalability |
|--------|------|--------|------------|
| Webhook | Push | Low | High |
| Polling | Pull | High | Low |
| Pub/Sub | Brokered | Low | Very High |

Webhooks are simpler than Pub/Sub but less robust.

---

## 9. Common Backend Mistakes

- Trusting webhook payloads blindly
- Doing long-running work synchronously
- Not handling retries properly
- Ignoring duplicate events
- Logging sensitive payload data

---

## 10. Interview Questions You Should Expect

1. What is a webhook?
2. Webhook vs polling?
3. How do you secure webhook endpoints?
4. Why must webhook handlers be idempotent?
5. How do retries work?
6. What happens if your server is down?
7. How do you verify webhook signatures?
8. When should you use queues with webhooks?
9. Can webhooks guarantee delivery?
10. Webhooks vs message queues?

---

## 11. Answer Template for Interviews

> “A webhook is an event-driven mechanism where one system pushes HTTP requests to another when an event occurs.
> Webhooks reduce polling and enable real-time integrations, but require strong security, validation, and idempotent handling.
> In production systems, webhooks are typically processed asynchronously for reliability.”

---

## 12. One-Minute Summary

- Webhooks push events over HTTP
- Enable real-time system integration
- Must be secured and validated
- Require idempotent handlers
- Often combined with queues for reliability
