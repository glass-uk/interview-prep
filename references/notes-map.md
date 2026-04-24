# Notes Topic-to-Folder Mapping

Use this file to determine where to save session notes.

## Notes Root

Default: `~/.claude/skills/interview-prep/notes/`

Override by setting `notes_path` in `references/config.md`.

---

## Topic Mapping Table

| Question Topic | Subfolder |
|---|---|
| Behavioral (leadership, conflict, delivery, growth) | `behavioral/` |
| Java Core (collections, generics, streams, equals/hashCode) | `java/` |
| Java Concurrency (threads, locks, CompletableFuture, volatile) | `java/` |
| JVM (GC, memory model, class loading) | `java/` |
| Spring Boot (annotations, DI, auto-config, Actuator) | `java/springboot/` |
| Spring Cloud (Eureka, Feign, Circuit Breaker, Config) | `java/springboot/` |
| Spring Security (filter chain, JWT) | `java/springboot/` |
| Kafka (topics, consumers, producers, offsets, DLT, Streams) | `kafka/` |
| MongoDB (schema, indexes, aggregation, replica set) | `mongodb/` |
| System Design (general distributed systems, architecture) | `system-design/` |
| DSA (algorithms, data structures, LRU, sorting) | `dsa/` |

**Customise this table** to match your own tech stack — add, remove, or rename rows to match what's in `references/config.md`.

---

## File Naming Convention

Pattern: `[Topic] - [Subtopic].md`

Examples:
* `Kafka - Consumer Groups.md`
* `Kafka - Dead Letter Topic.md`
* `Kafka - Exactly Once Semantics.md`
* `MongoDB - Aggregation Pipeline.md`
* `MongoDB - Schema Design.md`
* `Java - CompletableFuture.md`
* `Java - Garbage Collection.md`
* `Spring Boot - Transactional Annotation.md`
* `Spring Boot - Security Filter Chain.md`
* `System Design - URL Shortener.md`
* `System Design - Rate Limiter.md`
* `Behavioural - Conflict Resolution.md`
* `DSA - LRU Cache.md`
* `DSA - Median from Stream.md`

---

## Append vs Overwrite Logic

* If the target file **does not exist**: create a new file with the note content.
* If the target file **already exists**: Read the existing file first, then Write the file with the existing content plus the new question block appended at the bottom. Never overwrite without reading first.
* If the same question was answered in a previous session (matching question text): replace that section rather than duplicate it.

---

## Note Format Template

Each saved note block must follow this exact format:

```
#### [Question asked verbatim]

[User's answer summary — 2–3 sentences condensed, not verbatim]

---

#### Model Answer

[Full model answer as a senior engineer would give it — include code blocks where relevant]

---

#### Key Gaps to Review

* [gap 1 with brief explanation]
* [gap 2 with brief explanation]

---

#### To Review Checklist

- [ ] [specific concept or documentation to study]
- [ ] [specific concept or documentation to study]
```

Format rules:
- No YAML frontmatter
- `####` for all section headers
- `*` for bullet points
- Triple backticks with language tag for code blocks (e.g. ` ```java `)
- Tables use markdown pipe syntax
- Leave one blank line between sections
