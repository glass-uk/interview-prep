# Example — System Design Session

This example shows the 7-phase framework applied to a Kafka-based design question, including a probing follow-up on the hardest component.

---

**Interviewer:** Design a Kafka-based event processing pipeline for an e-commerce order system. Orders must be processed exactly once. Downstream services handle inventory, payments, and notifications.

---

## Phase 1 — Requirements (what a senior candidate asks)

"Before I start — a few questions:
- What's the expected order volume at peak? Are we talking hundreds per second or millions?
- What's the consistency requirement — is it acceptable for inventory to be updated before payment confirms, or must they be atomic?
- What happens if a downstream service is down for 5 minutes — should orders queue up or fail fast?
- Is exactly-once required across the Kafka boundary and the database, or just within Kafka?"

**Score signal:** Asking about exactly-once scope is a senior-level question — most candidates assume it means within Kafka only.

---

## Phase 2 — Capacity Estimate

"At 1,000 orders/sec peak, each event ~2KB: that's 2MB/sec write throughput. With 7-day retention and 3 replicas: ~3.6TB storage. Manageable on a 6-broker cluster with appropriate partition count."

---

## Phase 3 — High-Level Architecture

```
[Order Service]
    │
    ▼  (produce to)
[Kafka: orders topic, partitioned by orderId]
    │
    ├──► [Inventory Consumer Group]  → Inventory DB (MongoDB)
    ├──► [Payment Consumer Group]    → Payment DB (PostgreSQL)
    └──► [Notification Consumer Group] → Email/Push/SMS gateway
         │
         ▼ (on failure)
    [Dead Letter Topics: orders.inventory.dlt, orders.payment.dlt, orders.notification.dlt]
```

Partition key = `orderId` — ensures all events for a given order land in the same partition, preserving order per order.

---

## Phase 4 — Data Model

Orders topic schema:
```json
{
  "eventId": "uuid",
  "orderId": "uuid",
  "eventType": "ORDER_CREATED | ORDER_UPDATED | ORDER_CANCELLED",
  "occurredAt": "ISO8601",
  "payload": { ... }
}
```

Each consumer stores `eventId` in its own database to enable idempotency checks.

---

## Phase 5 — API Design

Producer side (Order Service REST API):
```
POST /orders         → creates order, publishes ORDER_CREATED event
PUT  /orders/{id}    → updates order, publishes ORDER_UPDATED event
DELETE /orders/{id}  → cancels order, publishes ORDER_CANCELLED event
```

Events are published within the same transaction as the DB write using the **transactional outbox pattern** (see Phase 6 deep dive).

---

## Phase 6 — Deep Dive: Exactly-Once Across DB + Kafka

"The hardest part is guaranteeing exactly-once processing when the consumer reads from Kafka and writes to a database — the two are different systems and you can't use a distributed transaction."

**Approach: Idempotent Consumer**

Each consumer checks for the `eventId` in its own store before processing:

```java
@KafkaListener(topics = "orders", groupId = "inventory-consumer")
public void handleOrder(OrderEvent event) {
    if (processedEventRepository.existsById(event.getEventId())) {
        log.info("Duplicate event {}, skipping", event.getEventId());
        return;
    }
    // process in a transaction: update inventory + record eventId atomically
    inventoryService.processInTransaction(event);
}
```

This gives idempotency at the application level. Kafka handles at-least-once delivery; the consumer ensures the effect is applied at most once.

**For the Order Service (producer side): Transactional Outbox**

```
INSERT order INTO orders table
INSERT event INTO outbox table
COMMIT (single DB transaction)

Outbox relay process reads outbox → publishes to Kafka → marks as published
```

This guarantees the event is published if and only if the DB write committed, eliminating the dual-write problem.

**Trade-off acknowledged:** "The outbox pattern adds latency (relay polling) and operational complexity. The alternative is Kafka transactions with `read_committed` isolation, but that only helps within Kafka — it doesn't solve the DB boundary problem."

---

## Phase 7 — Failure Modes

| Failure | Detection | Recovery |
|---|---|---|
| Consumer crashes mid-processing | Consumer lag metric spike on partition | Restart consumer; idempotency check prevents duplicate effect |
| Kafka broker down | Producer `send()` throws; broker health metric | Retry with backoff; ISR ensures no data loss if replication factor ≥ 2 |
| Downstream DB down | Consumer `save()` throws | Retry with exponential backoff; after N retries → DLT |
| DLT grows | DLT consumer lag alert | Ops investigation; replay after fix |
| Outbox relay falls behind | Outbox table row count metric | Scale relay instances; investigate DB contention |

---

**Score: 9/10**

**What you did well:**
* Asked scope-clarifying question about exactly-once before designing — exactly right
* Identified the DB boundary problem independently without being prompted
* Transactional outbox pattern is the correct production answer
* Failure mode table is comprehensive

**What was missing:**
* No mention of schema registry (Avro/Protobuf) for event contracts — at scale, schema evolution without a registry causes consumer breaks
* No mention of consumer group monitoring (consumer lag dashboard)

**Saved to:** `~/.claude/skills/interview-prep/notes/system-design/System Design - Kafka Order Pipeline.md`
