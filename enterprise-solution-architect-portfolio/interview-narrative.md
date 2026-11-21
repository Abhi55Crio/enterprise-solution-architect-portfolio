# 2-Minute Solution Architect Portfolio Walkthrough

"I approach solution architecture by starting with business outcomes and non-functional requirements rather than selecting technology first.

For this reference platform, I decomposed the business into customer, order, inventory, fulfilment and notification capabilities. I designed a secure API edge with DNS, WAF, API Gateway and load balancing, with stateless Java Spring Boot services running across availability zones.

For the order journey, the API validates the request and uses an idempotency key to protect against duplicate submissions. The Order Service persists the order and uses the transactional outbox pattern to publish an event reliably. Inventory and fulfilment process events asynchronously, which reduces synchronous coupling and allows independent scaling.

I make trade-offs explicit. A modular monolith may be preferable when domain boundaries or team maturity are not ready for microservices. Kafka is justified when event volume, replay, ordering and consumer independence warrant its operational cost; otherwise a managed queue may be simpler.

For production readiness, I cover OAuth/OIDC, least privilege, encryption, secrets management, observability with metrics/logs/traces, SLOs, multi-zone resilience, tested recovery and cost per business transaction. I use CI/CD quality gates, automated security checks, progressive delivery and architecture decision records.

My role is to connect product priorities with engineering execution: clarify requirements, challenge assumptions, communicate options and risks, guide teams, review implementation, and continuously improve reliability, performance and cost."

## Questions to expect
1. Why microservices instead of a modular monolith?
2. How do you prevent duplicate orders?
3. What happens if Kafka or inventory is unavailable?
4. How do you achieve 99.9% availability, and what does it cost?
5. How do you identify a performance bottleneck layer by layer?
6. How do you choose AWS vs Azure vs OCI?
7. How do you balance security, delivery speed and cost?
8. How do you govern architecture across multiple teams?

## Be ready to defend
- Workload assumptions and peak traffic
- Data consistency and transaction boundaries
- Failure modes and recovery
- Security threat model and data classification
- Cloud cost estimates based on actual usage
- SLOs, RTO/RPO and operational ownership

Do not present this reference design as a production implementation or claim measured outcomes unless you have evidence.
