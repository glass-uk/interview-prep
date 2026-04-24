# Scoring Rubric — Senior SWE Interview Evaluation

Apply these criteria consistently. Be honest — do not inflate scores.

> **Customise this file for your stack.** The technology-specific depth criteria below cover Java, Kafka, MongoDB, and Spring Boot. If your stack is different, add equivalent sections following the same pattern — list what's required for 8+ and what production-level knowledge looks like. Generic technical and behavioral criteria apply to all stacks.

---

## Score Scale

| Score | Meaning |
|---|---|
| 9–10 | Would genuinely impress a senior interview panel. Covers depth, breadth, trade-offs, edge cases, production experience. |
| 7–8 | Solid senior-level answer. Correct and complete with minor gaps. Would pass most interview panels. |
| 5–6 | Mid-level answer. Correct surface knowledge but lacks depth, trade-off discussion, or real examples. |
| 3–4 | Partially correct. Missing significant concepts or contains a notable factual error. |
| 1–2 | Mostly incorrect or fundamentally misunderstands the concept. |

Reserve 9–10 for answers that demonstrate production intuition, not just textbook knowledge.

---

## Behavioral Scoring Criteria

### STAR Structure (required for 7+)

Score against all four components. If any is missing, call it out explicitly:

* **Situation** — Context is clear and specific. Real project, real timeline. Not vague ("on a project once...").
* **Task** — Their personal responsibility is distinct from the team's. Not "we decided" — "I was responsible for..."
* **Action** — Concrete actions they personally took. Not "we built it" — "I proposed X, I escalated to Y, I wrote Z."
* **Result** — Quantified or measurable outcome. "Reduced p99 latency by 40%" beats "made it faster." Failure outcomes are fine if handled well.

### Senior-Level Signals (required for 8+)

* Evidence of cross-functional influence (not just IC delivery)
* Decision-making under ambiguity with reasoning
* Mentorship or measurable team impact
* Technical depth embedded in the story (not just "fixed a bug")
* Acknowledgement of what they'd do differently

### Deductions

* -1 if result is vague or unmeasured
* -2 if Task and Action are merged or personal contribution is unclear
* -2 if Situation is hypothetical rather than a real past event
* -1 if the story has no technical depth for a senior role

---

## Technical Scoring Criteria

### Core Correctness (base score)

* Correct understanding of the concept: minimum 5/10
* One factual error: -1 to -2 depending on significance
* Fundamental misunderstanding of the core concept: cap at 4/10

### Depth Signals (required for 8+)

* Explains the "why" not just the "what"
* Mentions trade-offs — not just the happy path
* Covers at least one failure mode or edge case
* References internal implementation (e.g., how HashMap segments work, not just "it's thread-safe")

### Senior-Level Signals (required for 9+)

* References production experience or real-world patterns
* Mentions operational concerns (monitoring, alerting, memory pressure, GC pauses)
* Discusses alternatives and explains why they were rejected
* Makes appropriate qualifications ("it depends on your consistency requirements")

---

## Java-Specific Depth Criteria

Required for 8+:
* Uses correct JVM/concurrency terminology (happens-before, monitor, heap vs stack, safepoint)
* Understands JVM behaviour, not just language syntax
* Aware of concurrency pitfalls: visibility, atomicity, ordering
* Can connect language feature to JVM implementation (e.g., `synchronized` → monitor enter/exit bytecode)

---

## Kafka-Specific Depth Criteria

Required for 8+:
* Understands partition-level semantics (ordering is per-partition, not per-topic)
* Distinguishes producer guarantees from consumer guarantees
* Aware of consumer group rebalancing cost and its effect on throughput
* Understands offset management: auto-commit risks vs manual commit
* Knows that exactly-once requires coordination across producer, broker, and consumer

---

## MongoDB-Specific Depth Criteria

Required for 8+:
* Understands document model trade-offs vs relational (no joins, denormalization, 16MB limit)
* Knows index types beyond basic single-field: compound, multikey, text, TTL, sparse
* Understands replica set consistency model (primary read vs secondary read, readConcern)
* Aware of aggregation pipeline stages: `$match`, `$group`, `$lookup`, `$unwind`, `$project`
* Can justify schema decisions based on query patterns, not just data shape

---

## Spring Boot-Specific Depth Criteria

Required for 8+:
* Understands DI container lifecycle (bean initialization order, `@PostConstruct`, `@PreDestroy`)
* Aware of `@Transactional` self-invocation proxy bypass
* Aware of `@Async` proxy bypass (same class call)
* Knows that `readOnly = true` is a hint to the driver/ORM for optimisation, not a hard lock
* Knows production concerns: Actuator endpoint security, externalized config, graceful shutdown

---

## System Design Scoring Criteria

### Phase Coverage (required for 7+)

All seven phases from `system-design-framework.md` must be addressed at minimum:
1. Requirements clarification
2. Capacity estimation
3. High-level architecture
4. Data model
5. API design
6. Deep dive on critical component
7. Failure modes and trade-offs

### Senior-Level Signals (required for 8+)

* Proactively narrows scope ("Given the time available, I'll focus the deep dive on X")
* Identifies the hardest part of the system before being prompted
* Discusses trade-offs between approaches — doesn't just pick the first thing that comes to mind
* Mentions observability: what metrics to emit, what to alert on
* Considers operational runbook: what breaks, how to detect it, how to recover

### Deductions

* -2 if capacity estimation is skipped entirely
* -2 if failure modes are not discussed at all
* -1 if data model is hand-waved ("just use a database")
* -1 if no API design is discussed

---

## Probing Questions to Ask After Each Answer

Adapt to context — pick one that reveals depth the candidate may have skipped:

* "How would that behave if the primary database node goes down mid-request?"
* "What happens if two Kafka consumers process the same message?"
* "How would you monitor whether this is working correctly in production?"
* "What would you change if you had to handle 10x the current load?"
* "Have you seen this pattern in production? What went differently from the theory?"
* "What's the biggest risk in your design and how would you mitigate it?"
* "What would you do differently if consistency was the top priority over availability?"
* "How does this behave during a rolling deployment when old and new versions are running simultaneously?"
