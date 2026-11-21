# Security and Resilience Architecture

## Security controls
| Layer | Controls |
|---|---|
| Edge | WAF, DDoS protection, TLS, bot/rate controls |
| Identity | OAuth 2.0 / OIDC, short-lived tokens, service identity |
| API | Scope/role authorization, schema validation, quotas |
| Service | Least privilege, input validation, secure defaults |
| Data | Encryption at rest, TLS in transit, managed keys, restricted network paths |
| Secrets | Central secrets manager, rotation, no secrets in Git |
| Operations | Immutable audit events, privileged access review, vulnerability scanning |

Threat-model STRIDE categories for public APIs, partner integrations, event consumers, secrets, administrative paths and supply chain. Map controls to the applicable regulatory framework after jurisdiction and data classification are known.

## Resilience patterns
- Set connection, request and downstream timeouts explicitly.
- Retry only transient failures; use exponential backoff + jitter and bounded attempts.
- Apply circuit breakers and bulkheads around unstable dependencies.
- Use bounded queues and backpressure; avoid unbounded in-memory buffering.
- Route poison messages to a DLQ with alerting and controlled replay.
- Use idempotency at API and consumer boundaries.
- Prefer graceful degradation for non-critical capabilities.
- Use multi-zone deployment and tested backup/restore.

## Failure scenario: inventory is unavailable
1. Order Service persists an accepted/pending order and outbox event.
2. Kafka retains the event within configured durability/retention limits.
3. Inventory consumers retry transient errors with bounded backoff.
4. Circuit breaker protects the inventory database and downstream calls.
5. DLQ captures messages that exceed retry policy; operations are alerted.
6. Customer status remains explicit; do not claim inventory is reserved until confirmed.

## Disaster recovery
Business owners must define RTO/RPO. Validate backup restoration, database point-in-time recovery, broker recovery, secrets/configuration restoration and DNS failover through scheduled exercises. Multi-region active-active is not automatically the best choice: assess consistency, data residency, operational complexity and cost first.
