```
  _       _                  _               
 (_)_ __ | |_ ___ _ ____   _(_) _____      __
 | | '_ \| __/ _ \ '__\ \ / / |/ _ \ \ /\ / /
 | | | | | ||  __/ |   \ V /| |  __/\ V  V / 
 |_|_| |_|\__\___|_|    \_/ |_|\___| \_/\_/  
                                              
                          
 _ __  _ __ ___ _ __     
| '_ \| '__/ _ \ '_ \    
| |_) | | |  __/ |_) |   
| .__/|_|  \___|_.__/    
|_|                      
```

**A [Claude Code](https://claude.ai/code) skill for software engineering interview preparation.**  
Structured mock interviews with per-answer scoring, session notes, spaced repetition, and progress tracking — all running locally inside your Claude Code session.

---

## Contents

- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Session Types](#session-types)
- [Modes](#modes)
- [JD & CV Tailoring](#jd--cv-tailoring)
- [Mid-Session Commands](#mid-session-commands)
- [Configuration](#configuration)
- [Session Notes & Progress Tracking](#session-notes--progress-tracking)
- [Customising for Your Stack](#customising-for-your-stack)
- [File Structure](#file-structure)
- [Contributing](#contributing)

---

## Features

| Category | What it does |
|---|---|
| **Session types** | Behavioral · Technical · System Design · Mixed · Post-interview debrief |
| **Modes** | Standard (live feedback) · Pressure (debrief at end) |
| **Tailoring** | JD-based · CV-based · JD + CV combined with gap analysis |
| **Company mode** | Style adjustments for Amazon, Google, Meta, Microsoft, Apple, Stripe, Netflix |
| **Warm-up** | Optional "Tell me about yourself" opener with structure and timing feedback |
| **Scoring** | Honest per-answer scores with model answers and gap analysis |
| **Retry** | Say "let me try again" — get a delta score on your second attempt |
| **Spaced repetition** | Questions scored ≤ 5 automatically resurface as variants in future sessions |
| **Code evaluation** | Correctness · complexity · edge cases · style |
| **Time awareness** | Estimates speaking time per answer · flags answers that are too long or too brief |
| **Session log** | Auto-written after every session — tracks questions asked, scores, dates |
| **Progress tracking** | Per-topic score trends across sessions · weak area ranking · "show my progress" |
| **Mid-session commands** | Hint · skip · harder/easier · retry · "what are they testing for?" · export |

---

## Installation

```bash
git clone https://github.com/glass-uk/interview-prep ~/.claude/skills/interview-prep
cp ~/.claude/skills/interview-prep/references/config.example.md \
   ~/.claude/skills/interview-prep/references/config.md
```

Then edit `references/config.md` with your stack and preferences (see [Configuration](#configuration)).

---

## Quick Start

In any Claude Code session:

```
Interview me
Start a mock interview — Kafka focus, pressure mode
Practice behavioral questions
Give me a system design question
Post-interview debrief
```

The skill reads your config, scans for weak areas from previous sessions, and asks a single setup question before starting.

---

## Session Types

### 1. Behavioral
STAR structure enforced on every answer. Missing components called out explicitly.  
Scoring requires: specific situation, personal responsibility (not "we"), concrete actions, quantified result.

### 2. Technical
Questions drawn from your configured stack. Probing follow-up after every answer before scoring.  
Pre-loaded questions for Java · Spring Boot · Kafka · MongoDB · DSA.

### 3. System Design
7-phase structured framework: requirements → capacity → architecture → data model → API → deep dive → failure modes.  
Guided through each phase. Skipped phases prompted before scoring.

### 4. Mixed
Rotates types: behavioral → technical → system design → behavioral.

### 5. Post-Interview Debrief
Describe a real interview you just had. Get model answers and gap analysis for every question asked.  
Identifies patterns across your answers ("your technical answers lack production depth") and recommends what to revise before the next round.

---

## Modes

**Standard** — feedback, score, and model answer after each question. Best for learning.

**Pressure** — interviewer responds as a real interviewer would (brief acknowledgement, no feedback). Full debrief delivered at the end. Best for simulation.

---

## JD & CV Tailoring

Paste either or both during session setup:

**JD only** — question weighting shifts toward what the role requires. Model answers framed to the role context. Company-specific style applied if the company is recognised.

**CV only** — questions drawn from your actual stated experience. Claimed skills held to the depth you claimed. Behavioral questions prompt you to draw from specific roles listed on your CV.

**JD + CV** — overlap identified (skills you claimed that the role requires) and treated as the highest-priority questions. CV gaps vs JD requirements surfaced upfront:
> "The role lists Kubernetes as a requirement but it doesn't appear on your CV. Want to include a question on that?"

### Company Style Adjustments

| Company | What changes |
|---|---|
| Amazon | Behavioral questions weighted toward Leadership Principles. Expect STAR with metrics. |
| Google | Algorithmic depth required. Time/space complexity discussion expected. |
| Meta | Product thinking + "why this over alternatives?" follow-ups. |
| Microsoft | Balanced technical and behavioral. Growth mindset framing. |
| Apple | Technical depth and correctness emphasis. |
| Stripe | API design and reliability focus. |
| Netflix | Autonomy, ownership, and trade-off reasoning signals. |

---

## Mid-Session Commands

Say any of these at any point during a Standard session:

| Command | What happens |
|---|---|
| `give me a hint` | One directional nudge. Score penalised by 1 point. |
| `how am I doing?` | Running score table: question · topic · score · session average. |
| `skip this question` | Move on. Not scored. |
| `harder` / `easier` | Adjusts difficulty of the next question. |
| `what are they testing for?` | Explains the intent and common traps, then re-asks. |
| `let me try again` | Second attempt on the same question. Shows `Attempt 1: X → Attempt 2: Y (Δ +Z)`. |
| `export session` | Writes full session to `notes/exports/[date]-session.md`. Session continues. |
| `save notes` | Saves model answers and checklists to topic notes files. |
| `end session` | Triggers session summary and auto-writes session log. |

> In Pressure mode all mid-session commands are ignored — the interviewer responds: "Let's keep going."

---

## Configuration

Copy the example and fill it in:

```bash
cp references/config.example.md references/config.md
```

```
tech_stack: Java, Spring Boot, Kafka, MongoDB
target_level: senior
notes_path: ~/.claude/skills/interview-prep/notes
time_tracking: on
warmup: ask
```

| Field | Options | Description |
|---|---|---|
| `tech_stack` | comma-separated list | Technologies to practise. Used for question selection, dynamic generation, and scoring calibration. |
| `target_level` | `mid` · `senior` · `staff` · `principal` | Sets difficulty framing and feedback expectations. |
| `notes_path` | any path | Where session notes are saved. Default: `notes/` inside this directory. |
| `time_tracking` | `on` · `off` | Estimates speaking time per answer and flags if outside the target window. |
| `warmup` | `ask` · `always` · `off` | Controls whether the "Tell me about yourself" opener is included each session. |

`config.md` is gitignored — it stays local.

---

## Session Notes & Progress Tracking

### Notes
Saved per topic to `notes/[topic]/[Topic] - [Subtopic].md`. Each note contains:
- The question (verbatim)
- Your answer summary
- The full model answer
- Key gaps
- A to-review checklist

Say `save notes` or `end session and save` to trigger saving.

### Session Log
`notes/session-log.md` is written automatically after every session. Tracks questions asked, scores, and dates. Powers:
- Cross-session question deduplication
- Spaced repetition (questions scored ≤ 5 resurfaced as variants)
- Score trend analysis

### Progress Check
Say `show my progress` or `what have I covered?` at any time to get:
- Topics practiced
- Per-topic score trends (improving / declining / stable)
- Ranked weak areas
- Recommended focus for next session

---

## Customising for Your Stack

The pre-loaded question bank covers Java · Spring Boot · Kafka · MongoDB · System Design · DSA · Behavioral.

To adapt it:

1. **`references/config.md`** — set `tech_stack` to your technologies
2. **`references/question-bank.md`** — replace or add technology sections using the `[M/H/VH]` format
3. **`references/scoring-rubric.md`** — add tech-specific depth criteria (see existing Java/Kafka/MongoDB sections as examples)
4. **`references/notes-map.md`** — update the topic → subfolder mapping to match your stack

The skill generates questions dynamically when the bank runs out, using your configured stack.

---

## File Structure

```
interview-prep/
├── SKILL.md                          # Skill definition — read by Claude Code at runtime
├── README.md                         # This file
├── .gitignore                        # Excludes config.md and notes from the repo
├── references/
│   ├── config.example.md             # Config template — copy to config.md and edit
│   ├── config.md                     # Your personal config (gitignored)
│   ├── question-bank.md              # Question library — customise for your stack
│   ├── scoring-rubric.md             # Scoring criteria per question type
│   ├── system-design-framework.md    # 7-phase system design framework
│   └── notes-map.md                  # Topic-to-folder mapping for saved notes
├── examples/
│   ├── behavioral-session.md         # Example: behavioral exchange with STAR feedback
│   ├── technical-session.md          # Example: technical exchange with follow-up
│   └── system-design-session.md      # Example: system design using the 7-phase framework
└── notes/                            # Your saved notes and session log (gitignored)
    ├── session-log.md                # Auto-written after every session
    ├── exports/                      # Full session exports
    ├── debrief/                      # Post-interview debrief notes
    └── [topic]/                      # Per-topic question notes
```

---

## Contributing

PRs welcome for:

- **Question bank sections** — Go, Python, Rust, TypeScript, React, PostgreSQL, Redis, Kubernetes, etc.
- **Scoring rubric sections** — tech-specific depth criteria for other stacks
- **Example sessions** — additional worked examples showing the skill in action
- **Company style entries** — additions to the company mode lookup table
- **System design framework improvements** — new phases, better prompts, additional failure mode patterns

---

*Built as a [Claude Code](https://claude.ai/code) skill. Runs entirely in your local Claude Code session — no external services, no data leaves your machine.*
