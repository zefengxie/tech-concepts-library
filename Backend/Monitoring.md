# Monitoring (Backend)

## 1. Formal Definition
**Monitoring** is the practice of continuously observing a backend system to understand its health, performance, and behavior.
It collects metrics, logs, and signals to detect issues and ensure reliability.

In simple terms:
> Monitoring tells you whether your backend is working as expected.

---

## 2. Why It Matters for Backend Development
Monitoring is essential because it:
- Detects failures early
- Helps diagnose performance problems
- Supports capacity planning
- Improves reliability and uptime

Without monitoring, backend failures are discovered only by users.

---

## 3. What Should Be Monitored
Key monitoring signals:
- **Availability**: is the service up?
- **Latency**: how fast is it?
- **Traffic**: how much load is it handling?
- **Errors**: are requests failing?
- **Saturation**: are resources exhausted?

This is often referred to as the **Golden Signals**.

---

## 4. Good Example: Basic Metrics Collection

```js
metrics.increment("http.requests.total");
metrics.observe("http.request.duration", duration);
```

Why this is good:
- Quantifiable system behavior
- Enables alerting
- Helps identify trends
- Supports root cause analysis

---

## 5. Bad Example: Relying Only on Logs
Problems:
- Logs are noisy
- Hard to aggregate
- Poor real-time visibility
- Slow incident response

Monitoring complements logging; it does not replace it.

---

## 6. Metrics vs Logs vs Traces

| Signal | Purpose |
|---|---|
| Metrics | System health over time |
| Logs | Detailed event records |
| Traces | Request flow across services |

Modern systems use **all three**.

---

## 7. Alerting Principles
Effective alerts should be:
- Actionable
- Based on symptoms, not causes
- Tuned to avoid alert fatigue
- Tested regularly

Example:
- Alert on high error rate, not CPU spikes alone

---

## 8. Monitoring in Distributed Systems
Challenges include:
- Multiple services
- Network failures
- Partial outages

Solutions:
- Centralized metrics collection
- Correlation IDs
- Distributed tracing

Monitoring becomes more complex as systems scale.

---

## 9. Common Backend Mistakes
- Alerting on everything
- No dashboards
- Ignoring monitoring in development
- No ownership of alerts
- No historical data retention

Bad monitoring leads to burnout and slow recovery.

---

## 10. Interview Questions You Should Expect
1. What is monitoring?
2. Why is monitoring important?
3. What are the golden signals?
4. Metrics vs logs vs traces?
5. How do you design alerts?
6. What causes alert fatigue?
7. How do you monitor microservices?
8. What should not trigger alerts?
9. How do you detect outages?
10. Monitoring vs observability?

---

## 11. Answer Template for Interviews

> “Monitoring is the practice of tracking system health and performance using metrics, logs, and alerts.
> It enables early detection of failures and supports reliable backend operations.
> Good monitoring focuses on user-impacting signals and actionable alerts.”

---

## 12. One-Minute Summary

- Monitoring observes system health
- Uses metrics, logs, and traces
- Enables early failure detection
- Critical for production reliability
- Essential for scalable backends
