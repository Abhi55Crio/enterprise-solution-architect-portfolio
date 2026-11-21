# ADR-003: Each service owns its persistence model

- Status: Accepted (reference design)
- Context: Shared database tables create release coupling and unclear ownership.
- Decision: Order, inventory and fulfilment own separate schemas/databases; cross-domain access uses APIs, events or purpose-built read models.
- Trade-offs: stronger ownership and independent evolution versus distributed queries and eventual consistency.
- Consequences: define data contracts, retention, reconciliation, backup and recovery per service.
