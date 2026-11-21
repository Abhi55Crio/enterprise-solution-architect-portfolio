# Low-Level Design (LLD)

## Order API
`POST /api/v1/orders`

Headers:
- `Authorization: Bearer <token>`
- `Idempotency-Key: <unique-client-key>`
- `X-Correlation-ID: <traceable-request-id>`

Example request:
```json
{
  "customerId": "C-10042",
  "items": [
    {"sku": "SKU-100", "quantity": 2}
  ],
  "channel": "WEB"
}
```

Example accepted response (`202`):
```json
{
  "orderId": "O-90001",
  "status": "ACCEPTED",
  "correlationId": "c-7f8a"
}
```

## Validation and error model
- `400`: malformed payload / schema violation
- `401`: missing or invalid authentication
- `403`: authenticated but not authorized
- `409`: idempotency key reused with a different request payload
- `429`: quota/rate limit exceeded
- `5xx`: unexpected server/dependency failure

Return a stable error contract with `code`, safe `message`, `correlationId` and optional field-level validation errors. Never expose stack traces or secrets.

## Service layering
```text
API Controller
  -> Application Service (use-case orchestration)
      -> Domain Model (business invariants)
      -> Repository Port
          -> PostgreSQL Adapter
      -> Event Publisher Port
          -> Outbox / Kafka Adapter
```

## Data ownership
- Order DB: order header, lines, lifecycle and idempotency key/hash.
- Inventory DB: SKU/location stock and reservation ledger.
- Fulfilment DB: shipment and fulfilment state.
- No cross-service direct database writes. Read models or APIs/events provide cross-domain views.

## Idempotency
Persist `(client_id, idempotency_key)` under a unique constraint with request hash and response reference. On retry with the same key and same hash, return the original outcome. If the payload differs, return `409`. Define retention based on client retry windows and business requirements.

## Event envelope
```json
{
  "eventId": "evt-123",
  "eventType": "OrderCreated",
  "eventVersion": 1,
  "occurredAt": "2026-11-22T10:00:00Z",
  "aggregateId": "O-90001",
  "correlationId": "c-7f8a",
  "payload": {"orderId": "O-90001"}
}
```
Consumers must be idempotent. Use schema compatibility checks and a defined event evolution policy.
