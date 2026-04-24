# Interview Prep — User Configuration

Edit the values below to personalise the skill. The skill reads this file at the start of every session.

tech_stack: Java, Spring Boot, Kafka, MongoDB
target_level: senior
notes_path: ~/.claude/skills/interview-prep/notes
time_tracking: on
warmup: ask

---

## Field reference

**tech_stack** — comma-separated list of technologies to practice. Used for question selection, dynamic question generation, and scoring rubric calibration. Examples: `Python, Django, PostgreSQL, Redis` or `Go, gRPC, Kubernetes, Postgres`.

**target_level** — seniority you are targeting. Options: `mid`, `senior`, `staff`, `principal`. Affects question difficulty framing and feedback tone.

**notes_path** — where session notes are saved. Defaults to the `notes/` folder inside this skill directory. Change this to redirect notes elsewhere (e.g. an Obsidian vault, a Dropbox folder, a local wiki).

**time_tracking** — whether to estimate speaking time and flag answers that are too long or too brief. Options: `on` (default), `off`.

**warmup** — whether to offer a 'Tell me about yourself' opener at the start of each session. Options: `ask` (prompt each time, default), `always` (always include without asking), `off` (never include).
