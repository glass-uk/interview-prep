# Question Bank — Senior SWE Interview Questions

All questions calibrated for senior/staff SWE roles.
Difficulty: [M] = Medium, [H] = Hard, [VH] = Very Hard

> **Customise this file for your stack.** The questions below are pre-loaded for a Java / Spring Boot / Kafka / MongoDB stack. Add, remove, or replace sections to match your configured tech stack in `references/config.md`. The skill will also generate questions dynamically if the bank runs out for a given topic.

---

## Behavioral Questions

### Leadership & Ownership
* [M] Tell me about a time you took ownership of a project that was at risk of failing.
* [M] Tell me about a time you mentored a junior developer. What was your approach and what was the outcome?
* [H] Tell me about a time you made a significant technical decision that turned out to be wrong. How did you handle it?
* [H] Describe a time you had to push back on a deadline or scope from a stakeholder.
* [H] Describe a situation where you had to influence a technical direction without having direct authority.

### Conflict & Collaboration
* [M] Describe a time you disagreed with a technical decision made by your team. What did you do?
* [H] Tell me about a time you had to work with a difficult teammate or stakeholder to deliver a project.
* [H] How have you handled a situation where two senior engineers had strongly opposing technical views?

### Delivery Under Pressure
* [M] Describe a time you had to deliver something with incomplete requirements. How did you proceed?
* [H] Tell me about the most complex technical incident you were involved in resolving. Walk me through it.
* [H] Tell me about a time you identified a critical production bug before it caused an outage. What did you do?

### Growth & Learning
* [M] What's the most significant technical concept you've had to learn on the job? How did you approach it?
* [M] Tell me about a time feedback changed how you work.

---

## Technical — Java Core

### Collections & Internals
* [M] Explain the difference between `HashMap` and `ConcurrentHashMap`. When would you use each?
* [H] Walk me through what happens internally when you call `new HashMap()` and then `put("key", value)`.
* [M] What is the contract between `equals()` and `hashCode()`? What happens if you override one but not the other?
* [M] What is the difference between `ArrayList` and `LinkedList`? When is each appropriate?

### JVM & Memory
* [H] Explain how the Java garbage collector works. What are the main GC algorithms and their trade-offs?
* [H] What is the Java Memory Model? How does `volatile` affect visibility between threads?
* [VH] What is false sharing in the context of CPU caches and Java threads? How do you mitigate it?

### Functional & Modern Java
* [M] What are functional interfaces? Give three examples from `java.util.function` and explain when you'd use each.
* [H] What is a `CompletableFuture`? How does it differ from `Future`? Give an example of chaining async operations.
* [M] What are the differences between `checked` and `unchecked` exceptions? When should you use each?

### Concurrency
* [H] Explain the difference between `synchronized`, `ReentrantLock`, and `StampedLock`. When is each appropriate?
* [H] Implement the producer-consumer pattern using a `BlockingQueue`. What are the thread-safety guarantees?
* [VH] What is the ABA problem in lock-free programming? How does `AtomicStampedReference` address it?
* [H] When would you choose a thread pool over virtual threads (Project Loom)? What are the limitations of virtual threads?

---

## Technical — Spring Boot

### Core Spring
* [M] What is the difference between `@Component`, `@Service`, `@Repository`, and `@Controller`?
* [M] Explain Spring's dependency injection. What's the difference between constructor, setter, and field injection? Which is preferred and why?
* [H] What is a `BeanFactory` vs `ApplicationContext`? When would you use each?
* [M] How does `@Transactional` work? What are the propagation levels and when do they matter?
* [H] What is the difference between `@Transactional(readOnly = true)` and a regular `@Transactional`? What optimisations does it enable?
* [H] Explain Spring's `@Async`. What are the pitfalls? What happens if an async method is called from within the same class?
* [M] What is Spring Boot auto-configuration? How does `@ConditionalOnClass` work?
* [M] What does Spring Boot Actuator provide? How would you secure the `/actuator` endpoints in production?
* [H] What is `@ConfigurationProperties` and how does it differ from `@Value`? Which is preferred for structured config?

### Security & Microservices
* [H] Explain the Spring Security filter chain. How would you add a custom JWT validation filter?
* [H] How would you implement a circuit breaker in a Spring Boot service calling an external API? Walk me through Resilience4j configuration.
* [VH] Explain the Saga pattern. When would you choose choreography-based vs orchestration-based Saga?
* [H] How does Spring Cloud Config work? How do you force all services to refresh config without restarting?
* [H] What is service discovery? Explain how Eureka works and what happens if the Eureka server goes down.

---

## Technical — Kafka

### Core Concepts
* [M] Explain the difference between a Kafka topic, partition, and offset.
* [M] What is a consumer group? How does Kafka assign partitions to consumers within a group?
* [H] What happens when a consumer crashes mid-processing? How does Kafka handle redelivery?
* [H] Explain at-least-once vs exactly-once semantics in Kafka. How do you achieve exactly-once?
* [M] What is `auto.offset.reset` and when does it apply? What are the risks of `earliest` vs `latest`?
* [H] How do you handle a slow consumer that is falling behind on its partition? What are the risks and mitigations?
* [H] What is log compaction? When would you use a compacted topic over a regular retention topic?
* [VH] Explain Kafka's leader election. What happens to in-flight messages during a leader failover?

### Spring Kafka & Patterns
* [M] How does `@KafkaListener` work? How do you configure concurrency for a listener?
* [H] How do you implement a dead-letter topic (DLT) pattern in Spring Kafka? Walk through the full configuration.
* [H] Explain idempotent producers. What configuration enables this and what guarantees does it provide?
* [VH] Design a pattern where producing to a Kafka topic and writing to a database must be atomic. What are the limitations?

---

## Technical — MongoDB

### Core Concepts
* [M] What is a document-oriented database? How is it fundamentally different from a relational database?
* [M] What is the `_id` field in MongoDB? How does the default ObjectId work and what does it encode?
* [H] Explain MongoDB's indexing model. What index types are available and when would you use each?
* [H] What is the aggregation pipeline? Write a pipeline that groups orders by customer and sums their total value.
* [H] Explain read concern vs write concern. When would you use `majority` for each?
* [VH] MongoDB is eventually consistent by default in a replica set. How do you ensure a read returns the latest write?
* [H] Explain the WiredTiger storage engine. How does document-level locking improve concurrency over earlier MongoDB versions?

### Schema Design
* [H] When would you embed a document vs use a reference? What are the concrete trade-offs?
* [VH] Design a MongoDB schema for an e-commerce system with orders, products, and customers. Justify every embedding/reference decision.
* [H] What is the 16MB document size limit and how does it affect schema design when you have large arrays?

---

## System Design

### Distributed Systems
* [H] Design a URL shortening service (like bit.ly). Handle 100M URLs, 10B reads/month.
* [H] Design a notification system that sends push, email, and SMS notifications at scale.
* [VH] Design a distributed rate limiter that works across multiple API gateway nodes without a central bottleneck.
* [VH] Design a real-time leaderboard for a gaming platform with 50M active users.
* [H] Design a Kafka-based event processing pipeline for an e-commerce order system. How do you handle out-of-order events?
* [H] Design a microservices architecture for a parcel tracking system. What are the service boundaries?
* [VH] Design a globally distributed data store. How do you handle consistency across regions while meeting low-latency SLAs?

### Deep-Dives (diagnostic style)
* [H] Walk me through what happens when a Spring Boot service sends a message to Kafka and a downstream service consumes it. Include every failure mode at each step.
* [H] Your MongoDB collection has slow queries under production load. Walk me through your full diagnostic and optimisation process.
* [VH] Your Kafka consumer lag is growing under peak load. Walk me through your full investigation and remediation steps.

---

## DSA — Senior Level

* [M] Implement a LRU Cache with O(1) get and put. What data structures do you use and why?
* [H] Given a stream of integers, return the running median after each insertion in O(log n). How?
* [M] Implement a thread-safe singleton in Java. What are the three approaches and which is preferred?
* [H] Design a rate limiter using the token bucket algorithm. Implement the core logic.
* [H] Given a directed graph of service dependencies, detect circular dependencies. What algorithm and why?
* [M] Implement `merge(List<Interval> intervals)` — merge all overlapping intervals.
* [H] Write both a recursive and iterative implementation of binary tree in-order traversal.
