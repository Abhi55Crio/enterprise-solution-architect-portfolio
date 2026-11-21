# ADR-002: Use event-driven integration for long-running workflows

- Status: Accepted (reference design)
- Context: Inventory and fulfilment should process independently; order acceptance should not wait for every downstream operation.
- Decision: Publish versioned domain events through Kafka using an outbox relay; consumers are idempotent.
- Options: synchronous REST chaining, broker-based messaging, managed queue/event service.
- Trade-offs: asynchronous decoupling and replayability versus eventual consistency, schema governance and operational complexity.
- Controls: outbox, consumer deduplication, bounded retries, DLQ, schema compatibility, lag monitoring and replay runbooks.
