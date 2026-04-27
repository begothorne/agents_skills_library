# LearningOps

**A personal learning continuity system for LLM-assisted study.**

LearningOps turns every LLM study session into a structured memory record. Instead of fragmented chat histories, it maintains a running model of what you know, what you still need to understand, and what you should do next — across any topic, across any session, across time.

Built on plain markdown files. Works from the terminal today. Supports a self-hosted browser UI when you need it.

---

## The problem it solves

When you use an LLM to learn something, the conversation ends and the context disappears. The next session starts from zero. Over time you end up with:

- Repeated explanations of the same concepts
- No record of what you actually understood vs what you just read
- No continuity between sessions on the same topic
- No tracking of where your gaps are
- No guidance on what to focus on next

LearningOps fixes this by treating every session as an input to a persistent learner model rather than a disposable conversation.

---

## What it can do

After a session, the system should be able to answer:

- What was I learning last session?
- What did I ask, and what did I understand?
- What am I still confused about?
- What should I do next?
- What should I review today, and why?
- What proof do I have that I actually learned something?

If those six questions are answered correctly, the system is working.

---

## How it works

The core loop:

```
ask → answer → extract → store → continue → review
```

Each session produces:
1. A **session record** — what was asked, likely learned, confirmed, and still unclear
2. An updated **topic file** — known concepts, gaps, next actions, review queue
3. No update to the learner profile unless explicitly requested

The LLM reads these files at the start of every session to restore context. The files are the memory.

---

## What "learning" means here

The system tracks three levels of learning evidence:

| Field | Meaning |
|-------|---------|
| `likely_learned` | Concepts the learner was exposed to — inference, not confirmation |
| `user_confirmed` | Concepts the learner explicitly stated they understood |
| `evidence` | Artifacts produced: explanation, quiz, code, project, recall, correction |

These three are always separate fields. Conflating them is the most common failure in AI-based learning tracking.

---

## Project structure

```
Learning-Ops/
│
├── LEARNING_AGENT.md          Canonical agent instructions — hard rules, soft rules,
│                              session lifecycle, ID format, multi-entry consistency
│
├── CLAUDE.md                  Thin wrapper for Claude Code → points to LEARNING_AGENT.md
├── AGENTS.md                  Thin wrapper for Codex / OpenAI → points to LEARNING_AGENT.md
├── DATA_CONTRACT.md           Which files agents may read freely vs write only with confirmation
├── CHANGELOG.md               Version history
│
├── config/
│   ├── settings.yml           All soft rule defaults (model, context, review, topics, evidence)
│   └── profile.yml            Identity shorthand: name, timezone, preferred entry point
│
├── learner/
│   └── profile.md             Full learner state: goals, style, strengths, active topics
│
├── topics/
│   └── _template.md           Copy for each new learning topic
│
├── sessions/
│   └── _template.md           Produced by consolidation at the end of each session
│
├── modes/
│   ├── _shared.md             Rules that apply in every mode
│   ├── learn.md               Explanation and teaching mode
│   ├── review.md              Recall and quiz mode
│   ├── plan.md                Goal setting and roadmap mode
│   └── consolidate.md         End-of-session write mode
│
├── docs/
│   ├── getting-started.md     First session walkthrough
│   └── learning-ops-v0-spec.md  Full v0 specification
│
├── server/
│   └── README.md              Browser UI backend interface spec (placeholder for v1)
│
├── scripts/
│   └── README.md              Future utility scripts spec
│
├── .env.example               API key template
└── .gitignore
```

---

## Three memory layers

Every session interacts with three separate files that answer three different questions.

| Layer | File | Question |
|-------|------|---------|
| Learner profile | `learner/profile.md` | Who is this person and how do they learn? |
| Topic state | `topics/<topic_id>.md` | What is the current state of this skill? |
| Session record | `sessions/<session_id>.md` | What happened in this specific session? |

The learner profile is written during onboarding and updated only on explicit request. Topic and session files are updated after every session.

---

## Getting started

### 1. Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed, or Codex / another LLM CLI
- An API key (Anthropic or OpenAI)

### 2. Clone and configure

```bash
git clone https://github.com/SillyAllen86/learning-ops.git
cd learning-ops
cp .env.example .env
```

Edit `.env`:

```bash
# For Claude
ANTHROPIC_API_KEY=sk-ant-...
LEARNINGOPS_PROVIDER=anthropic

# For OpenAI / Codex
OPENAI_API_KEY=sk-...
LEARNINGOPS_PROVIDER=openai
```

### 3. Start your first session

```bash
# With Claude Code
claude

# With Codex
codex
```

On first use, the agent detects that `learner/profile.md` is blank and runs the **onboarding flow**. It will ask you what you want to learn, why, your current level, and how you prefer to study. It only writes to the profile after you confirm.

### 4. Learn

```
Explain how AI agents work.
```
```
I want to learn German for work conversations.
```
```
Plan how I should approach Python automation.
```

### 5. Consolidate at the end

```
Consolidate session
```

The agent produces a structured session summary, shows you what it plans to write, and waits for your confirmation before saving anything.

### 6. Next session

The agent loads your profile, the relevant topic file, and the last three sessions. You resume where you left off.

---

## Mode reference

| Mode | Trigger | What it does |
|------|---------|-------------|
| `learn` | Question, explanation request | Teaches at the right level, adapts to your style |
| `review` | "quiz me", "test me", "recall" | Tests retention, updates confidence |
| `plan` | "I want to learn X", "roadmap" | Creates topic files, milestones, first actions |
| `consolidate` | "wrap up", "save session" | Turns the session into a structured memory record |

There is also a **one-off explanation path** in learn mode: if you ask a quick question about a topic you're not tracking, the agent answers fully without creating or updating any files. Say "add this as a topic" to start tracking it.

---

## Hard rules

Twelve rules that cannot be overridden by any configuration or user instruction.

| Code | Rule |
|------|------|
| HARD-001 | `user_confirmed` requires an explicit learner statement of understanding |
| HARD-002 | `evidence` requires a real artifact: explanation, quiz, code, project, recall, correction |
| HARD-003 | User Layer files are never written without presenting the change and getting confirmation |
| HARD-004 | Always use stable IDs — never free-text references for sessions, topics, goals, concepts |
| HARD-005 | Never merge two topics into one session record |
| HARD-006 | Never claim understanding because an explanation was good — only because the learner demonstrated it |
| HARD-007 | Always offer consolidation before ending a session |
| HARD-008 | Never write to `.env` or expose API keys in any output |
| HARD-009 | Blank learner profile triggers the onboarding flow — never invent a profile |
| HARD-010 | `likely_learned`, `user_confirmed`, and `evidence` are always three separate fields |
| HARD-011 | Never save a raw transcript as a session record — always distill to the schema |
| HARD-012 | Never load sessions from one topic to answer questions about a different topic |

---

## Soft rules (configuration)

All defaults live in `config/settings.yml` and can be changed without editing any agent instructions.

| Setting path | Default | What it controls |
|--------------|---------|-----------------|
| `context.sessions_loaded` | 3 | Past sessions loaded at session start |
| `review.default_days` | 4 | Days before a new concept enters the review queue |
| `review.surface_overdue_in_all_modes` | true | Show overdue reviews at the start of every session |
| `topics.max_active` | 5 | Warn before adding a sixth active topic |
| `topics.auto_archive_threshold` | 10 | Sessions before suggesting archival |
| `session.consolidation_required` | true | Warn if session ends without consolidation |
| `session.evidence_required_for_mastery` | true | Concept confidence cannot reach `high` without evidence |
| `explanation.default_style` | "structured with examples" | Fallback style if not in learner profile |

---

## ID format

All IDs use stable, human-readable formats.

```
session_id  →  YYYY-MM-DD-<topic_id>-<NNN>     2026-04-27-ai_agents-001
topic_id    →  snake_case                        ai_agents, german_b2
goal_id     →  snake_case, descriptive           ai_portfolio_2026
concept_id  →  <topic_id>_c<NNN>                ai_agents_c001
action_id   →  <topic_id>_a<NNN>                ai_agents_a001
review_id   →  <topic_id>_r<NNN>                ai_agents_r001
```

---

## Evidence types

| Type | When to use |
|------|-------------|
| `explanation` | Learner explained the concept in their own words |
| `quiz` | Learner answered practice questions correctly |
| `project` | Learner built a working artifact |
| `code` | Learner wrote code demonstrating the concept |
| `summary` | Learner wrote notes or a written summary |
| `recall` | Learner recalled a concept correctly after a delay |
| `correction` | Learner identified and fixed their own earlier mistake |
| `application` | Learner used the concept in a real task or project |

---

## Entry points

LearningOps works the same way regardless of how you access it. The markdown files are always the source of truth.

| Entry point | How it works |
|------------|-------------|
| Claude Code CLI | Open `claude` in the project directory. Reads and writes files directly. |
| Codex CLI | Open `codex` in the project directory. Same rules, same files. |
| Third-party app | App loads the same files as context before each API call. |
| Browser UI | Local backend reads files, passes as context to API, writes after confirmation. See `server/README.md`. |

The browser UI is not included in v0. It requires a small local backend (Node, Python, or Bun) to bridge the browser and the local file system. The interface spec is in `server/README.md`.

---

## Data contract

Files are split into two layers.

**User Layer** — read freely, never written without confirmation:
- `config/profile.yml`, `learner/profile.md`
- `topics/*.md`, `sessions/*.md`, `archive/*.md`

**System Layer** — safe to update:
- `LEARNING_AGENT.md`, `CLAUDE.md`, `AGENTS.md`, `DATA_CONTRACT.md`
- `modes/*.md`, `config/settings.yml`, templates, scripts

---

## Customization

This system is designed to be made yours.

- **Change your learning style**: edit `learner/profile.md` or tell the agent "update my profile"
- **Add a new topic**: say "I want to learn X" — the agent will run the planning flow
- **Pause a topic**: edit `topics/<topic_id>.md` → `status: paused`
- **Adjust defaults**: edit `config/settings.yml`
- **Change the LLM**: update `model.primary` in `config/settings.yml`
- **Switch providers**: change `LEARNINGOPS_PROVIDER` in `.env`

---

## What not to build first

The system is intentionally minimal in v0.

Not included by design:
- Vector database
- Multi-agent architecture
- Automatic session detection
- Calendar or notification integration
- A2A protocol
- Complex scoring algorithms
- Browser UI (placeholder spec only)

Add these only after the core loop (ask → answer → extract → store → continue → review) is working reliably for you.

---

## Inspired by

LearningOps was directly inspired by **[career-ops-remix](https://github.com/SillyAllen86/career-ops-remix)** — a public, markdown-first job search workflow system for AI coding agents.

career-ops-remix proved that a markdown-file-based, LLM-readable system with a clear data contract, canonical agent instructions, and a user/system layer split is a practical and durable pattern for personal productivity tooling.

LearningOps takes the same architectural pattern and applies it to learning continuity:

| career-ops | learning-ops |
|-----------|-------------|
| `CAREER_OPS_AGENT.md` | `LEARNING_AGENT.md` |
| `DATA_CONTRACT.md` | `DATA_CONTRACT.md` |
| `cv.md` (master profile) | `learner/profile.md` |
| `data/experience-bank.md` | `topics/<topic_id>.md` |
| `modes/oferta.md` | `modes/learn.md`, `modes/review.md` |
| Application tracker | Session history index |

The research basis for the use case also draws on work in intelligent tutoring systems, long-term conversational memory, and adaptive learning:

- A2A protocol community discussion on [multi-session learning agents](https://github.com/a2aproject/A2A/issues/893)
- [Haystack: memory in conversational agents](https://haystack.deepset.ai/blog/memory-conversational-agents)
- [Ordo Research: AI education and intelligent tutoring systems](https://ordoresearch.ai/blog/ai-education-intelligent-tutoring-systems-2025)
- [MeraTutor: AI learning outcome tracking](https://www.meratutor.ai/blog/ai-learning-outcome-tracking/)
- [Student modeling in adaptive systems](https://papersflow.ai/research/topics/intelligent-tutoring-systems-and-adaptive-learning/student-modeling-in-adaptive-systems)
- [Long-term memory in LLM conversations (arXiv 2402.17753)](https://arxiv.org/abs/2402.17753)

---

## Contributing

This is an early-stage personal tool. Contributions are welcome.

If you use LearningOps and find that a schema field is missing, a rule is unclear, or a template doesn't capture what you need — open an issue or a PR.

Keep changes consistent with the design principles:
- Markdown-first, no database required
- User Layer / System Layer separation
- Hard rules stay in `LEARNING_AGENT.md`
- Soft rules stay in `config/settings.yml`
- No parallel data structures

---

## License

MIT — see [LICENSE](LICENSE).

---

## Status

`v0.1.0` — scaffolding complete, terminal-first loop ready for manual validation.

The first real quality check is one complete end-to-end session: onboarding → topic creation → learning session → consolidation → next session with context restored. Run that before adding any automation.
