# Observability, Performance and SLOs

## Three pillars plus context
- Metrics: rate, errors, duration, saturation, queue lag, DB pool usage.
- Logs: structured JSON with timestamp, service, environment, traceId, correlationId and safe business identifiers.
- Traces: OpenTelemetry spans across gateway, services, database and messaging.
- Events: audit/business lifecycle history for support and reconciliation.

## Example service dashboard
| Signal | Why it matters | Example alert trigger |
|---|---|---|
| HTTP 5xx rate | User-visible failures | Sustained error budget burn |
| p95/p99 latency | Slow experience | Above agreed SLO for 10 min |
| CPU / memory / throttling | Resource saturation | Sustained saturation |
| JVM heap / GC pause | Java runtime health | Abnormal GC or old-gen growth |
| Hikari active/pending connections | DB pool pressure | Pending threads sustained |
| DB query latency / locks | Persistence bottleneck | Regression against baseline |
| Kafka consumer lag | Async processing delay | Lag and age exceed workflow SLO |
| DLQ count | Poison events / contract errors | Any unexpected growth |

## Layer-wise performance diagnosis
1. Client/edge: DNS time, TLS handshake, CDN cache hit, WAF latency.
2. Gateway/LB: request rate, throttling, target health, connection saturation.
3. JVM/service: thread pools, CPU, heap, GC, lock contention, endpoint traces.
4. Database: slow queries, execution plans, indexes, locks, pool wait, connection leaks.
5. Messaging: producer errors, partition skew, consumer lag, retries and DLQ.
6. Platform/network: pod restarts, throttling, node pressure, DNS, packet loss and cross-zone latency.

Use one trace/correlation ID to follow a request end-to-end. Diagnose with evidence before scaling: adding replicas will not fix a saturated database or inefficient query.

## SLO operating model
- Define SLIs with product and operations teams.
- Agree SLOs, error budgets and alert policies.
- Prioritize customer impact and error-budget burn over noisy infrastructure-only alerts.
- Review incidents, MTTR, recurring causes and corrective actions.
