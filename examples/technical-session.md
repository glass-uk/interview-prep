# Example — Technical Session (Spring Boot)

This example shows a technical exchange with a follow-up probing question and a model answer that references existing session notes.

---

**Interviewer:** How does `@Transactional` work in Spring Boot and what are the most common pitfalls at a senior level?

**User:** It wraps the method in a database transaction. If the method throws an exception the transaction rolls back. A common pitfall is calling a `@Transactional` method from within the same class — the proxy gets bypassed.

---

**Score: 7/10**

**What you did well:**
* Correct core explanation of the transactional wrapper behaviour
* Correctly identified the self-invocation proxy bypass — this is a trap that catches many mid-level candidates and shows real Spring internals knowledge
* Mentioned rollback on exception correctly

**What was missing or could be stronger:**
* Did not mention that by default only `RuntimeException` (unchecked) triggers rollback — checked exceptions do NOT roll back unless you add `rollbackFor = Exception.class`. This is a real source of production data corruption bugs.
* Did not mention propagation levels (`REQUIRED`, `REQUIRES_NEW`, `NESTED`) — interviewers frequently follow up here
* Did not mention `readOnly = true` and its performance implications for read-heavy operations

**Model answer:**
`@Transactional` instructs Spring to wrap the annotated method in a database transaction using AOP proxying. On method entry a transaction is started (or joined, depending on propagation); on successful return it commits; on an exception it rolls back — but with an important caveat.

The most dangerous pitfall in production is rollback behaviour: by default Spring only rolls back on `RuntimeException` (unchecked). If you throw a checked exception — say, an `IOException` or a custom checked exception — the transaction commits silently. This has caused real data corruption in production systems. The fix is either `@Transactional(rollbackFor = Exception.class)` or restructuring to use unchecked exceptions consistently.

The second pitfall is self-invocation. `@Transactional` works through a proxy object. If method A in class `OrderService` calls `@Transactional` method B within the same class directly — `this.methodB()` — it bypasses the proxy, so the transaction on B is silently ignored. The fix is to inject the bean via the application context or, better, restructure so transactional boundaries are always crossed from outside the class.

Propagation is worth knowing for senior roles. The default is `REQUIRED` — join an existing transaction or create a new one. `REQUIRES_NEW` suspends the outer transaction and starts a fresh one; this matters when you need an inner operation to commit independently of the outer (classic use case: audit logging that must persist even if the outer transaction rolls back). `NESTED` creates a savepoint within the outer transaction, which not all JDBC drivers support.

```java
// Example: audit log that must commit even if outer tx rolls back
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void saveAuditLog(AuditEvent event) {
    auditRepository.save(event);
}
```

---

**Follow-up question:** "If a `@Transactional` method calls another service over HTTP — does the transaction cover that remote call?"

**User:** No, transactions only apply to the local database connection.

**Score: 9/10** — Correct. The transaction boundary is the JDBC connection. Distributed consistency across an HTTP call requires a different pattern — Saga (choreography or orchestration), or a transactional outbox pattern if you need atomic write + event publish. Could have mentioned those for a perfect 10.

---

**Note that this answer cross-references an existing note** — if a note on `@Transactional` already exists, the new block is appended to it. Otherwise a new file is created.

**Saved to:** `~/.claude/skills/interview-prep/notes/java/springboot/Spring Boot - Transactional Annotation.md`
