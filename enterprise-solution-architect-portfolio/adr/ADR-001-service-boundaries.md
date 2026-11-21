# ADR-001: Align service boundaries to business capabilities

- Status: Accepted (reference design)
- Context: Order, inventory, fulfilment and notification have different business ownership, data lifecycles and scaling patterns.
- Decision: Separate these capabilities into independently deployable services only where team ownership and operational maturity justify the distributed-systems cost.
- Options considered:
  1. Modular monolith: simpler deployment and transactions; can be the right starting point.
  2. Microservices: independent scaling/release and fault isolation; adds network, observability, data consistency and platform complexity.
- Consequences: Service-owned data, explicit APIs/events, eventual consistency and stronger operational discipline are required.
- Revisit when: domain boundaries are unclear, traffic/team scale changes, or service overhead exceeds business value.
