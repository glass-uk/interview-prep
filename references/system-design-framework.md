# System Design Interview Framework

Use this structured framework for all system design questions. Guide the user through each phase. Do not score until they have attempted all phases (or explicitly asked for help on a phase).

---

## Phase 1 — Requirements Clarification (2–3 min)

Prompt the user to ask clarifying questions before designing anything. A senior candidate never starts designing without understanding the problem.

Prompt if they skip this phase:
> "Before you start designing, what questions would you ask the interviewer to understand the requirements?"

Expected questions to establish:

**Functional requirements:**
* What are the core user actions? (read, write, stream, subscribe?)
* What entities does the system manage?
* Are there latency SLAs for reads vs writes?

**Non-functional requirements:**
* Consistency vs availability priority (CAP trade-off for this problem)
* Data durability requirements (can we lose data on crash?)
* Global vs single-region deployment
* Read-heavy vs write-heavy workload ratio
* Expected peak scale (users, requests/sec, data volume)

Scoring signal: a senior candidate proactively asks about failure modes and consistency requirements, not just happy-path functionality.

---

## Phase 2 — Capacity Estimation (2–3 min)

Prompt: "Let's do a quick back-of-envelope calculation before we design."

Guide through:
* **Writes/sec**: daily write volume / 86,400
* **Reads/sec**: if read-heavy, typically 10–100x writes
* **Storage**: record size × daily writes × retention period (in bytes → GB → TB)
* **Bandwidth**: request payload size × RPS
* **Cache size**: hot data percentage × total dataset size

Round aggressively — order-of-magnitude is the goal, not precision.

Common reference numbers:
* 1M users/day active → ~12 req/sec average, ~100 req/sec peak
* 1KB record × 1M records/day × 365 days × 3 replicas → ~1TB/year
* HTTP request overhead ~1KB, Kafka message ~1–10KB

If the candidate skips this phase entirely, deduct 2 points from system design score.

---

## Phase 3 — High-Level Architecture (5 min)

Prompt: "Walk me through the major components of your system."

Expect to see these components called out with justification:
* **Client layer** — web, mobile, or service consumer
* **API layer** — REST, gRPC, or event-driven (with justification for the choice)
* **Application services** — what services exist and what are their responsibilities
* **Data stores** — primary DB, cache layer, search index, blob storage (justified by access pattern)
* **Message queue / event stream** — Kafka if applicable, with topic design
* **Load balancer / CDN** — if globally distributed or high read volume

Senior-level signal: the candidate identifies the **critical path** (the sequence of operations that must be fast and reliable) and calls it out explicitly before going deeper.

---

## Phase 4 — Data Model (3–4 min)

Prompt: "Walk me through your data model. What are the primary entities and how are they stored?"

Evaluate:
* Choice of data store is **justified by access patterns**, not just familiarity
* Schema reflects how the data will be **queried**, not just how it exists
* Indexes are considered early (not as an afterthought)
* Partitioning or sharding strategy is mentioned if operating at scale

MongoDB-specific: look for embedding vs reference justification based on read/write ratio and document update frequency.

Kafka-specific: look for topic naming, partition key choice (and why), and retention policy.

Relational-specific: look for normalization decisions and index strategy.

---

## Phase 5 — API Design (2–3 min)

Prompt: "Sketch the key API endpoints or events your system exposes."

Expect:
* REST: resource-oriented URLs, correct HTTP verbs (GET/POST/PUT/PATCH/DELETE), sensible response shapes
* Events: event schema with all fields, what triggers the event, and consumer contract
* Pagination on list endpoints (cursor-based preferred over offset for large datasets)
* Authentication/authorisation consideration (who can call what?)
* Versioning consideration for public APIs

---

## Phase 6 — Deep Dive on Critical Component (5–7 min)

This phase separates senior from mid-level candidates. The candidate should:

1. Identify the hardest component: "The trickiest part of this system is X because..."
2. Propose a solution with explicit trade-off discussion
3. Consider at least one alternative and explain why it was rejected

Prompt if they don't self-direct:
> "Which part of this system do you think is the hardest to get right? Let's go deep on that."

Common deep-dive areas:
* **Consistency**: how do two services agree on state after a network partition?
* **Idempotency**: what if a request is retried due to a timeout?
* **Fan-out**: how do you notify 1M subscribers in near-real-time?
* **Hot partitions**: one Kafka partition or DB shard receiving disproportionate traffic
* **Cache invalidation**: how do you keep a cache consistent with the source of truth?
* **Distributed transactions**: how do you atomically update two systems without 2PC?

For Kafka-heavy designs: probe on exactly-once semantics and the transactional outbox pattern.

For MongoDB-heavy designs: probe on read concern during replica set failover.

---

## Phase 7 — Failure Modes & Trade-offs (3 min)

Prompt: "What breaks in your system and how would you detect and recover from it?"

Expect:
* At least 2–3 concrete failure scenarios identified proactively
* Detection mechanism for each: specific metric, alert threshold, or log pattern
* Recovery path: retry with backoff, fallback to degraded mode, circuit breaker, human escalation
* Explicit trade-off acknowledgement: "I chose X which means I sacrifice Y in exchange for Z"

Common failures to probe if the candidate doesn't raise them:
* Database primary node failure
* Kafka broker failure during message production
* Downstream service timeout during synchronous call
* Cache miss storm (thundering herd) during cache warm-up
* Schema migration running on a live system

---

## Scoring Reference

| Score | What it represents |
|---|---|
| 9–10 | Covers all 7 phases with depth. Identifies the hardest part independently. Demonstrates production intuition ("I've seen this fail in production in X way"). |
| 7–8 | Covers phases 1–5 well plus a solid deep dive. Discusses trade-offs. Minor gaps in failure mode analysis. |
| 5–6 | Gets architecture right but skips capacity estimation, hand-waves data model, or doesn't reach failure modes. |
| 3–4 | Jumps straight to naming technologies without clarifying requirements. Design has structural flaws. |
| 1–2 | Cannot identify core entities or doesn't understand the distributed system challenge involved. |
