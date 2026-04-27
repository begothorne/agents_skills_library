# LearningOps v0 — Specification

## Purpose

A personal learning continuity system that converts LLM study sessions into structured memory, enabling a learner to resume any topic with full context, gap awareness, and a clear next step.

## v0 Goal

Prove exactly one thing: **can the system remember what was learned last time and continue from there?**

Nothing else is in scope until that is working.

---

## Core File Types

| File | Layer | Purpose |
|------|-------|---------|
| `LEARNING_AGENT.md` | System | Canonical agent instructions |
| `CLAUDE.md` | System | Claude Code thin wrapper |
| `AGENTS.md` | System | Codex thin wrapper |
| `DATA_CONTRACT.md` | System | User vs system layer rules |
| `config/settings.yml` | System | Soft rule defaults |
| `config/profile.yml` | User | Identity shorthand |
| `learner/profile.md` | User | Full learner state |
| `topics/<topic_id>.md` | User | Per-topic learning state |
| `sessions/<session_id>.md` | User | Per-session structured record |

---

## Three Memory Layers

Each layer answers a different question. They are always separate.

| Layer | File | Question answered | Updated when |
|-------|------|------------------|-------------|
| Learner profile | `learner/profile.md` | Who is the learner and how do they learn? | Onboarding, or explicit learner request only |
| Topic state | `topics/<topic_id>.md` | What is the current state of this skill? | Every session (during consolidation) |
| Session record | `sessions/<session_id>.md` | What happened in this specific session? | Every session (during consolidation) |

The learner profile is **not** updated on every session. It is written during onboarding and updated only when the learner explicitly asks to change goals, style, or active topics. Forcing profile updates every session increases confirmation burden and risks polluting long-term state with temporary observations.

---

## Session Lifecycle

```
1. Start        Load: settings → profile → topic → last N sessions
2. Orient       Show open next_actions and overdue reviews
3. Learn/Work   Respond using the appropriate mode
4. Monitor      Track concepts, confusion, understanding, artifacts
5. Signal       Learner says "consolidate" → switch to consolidate mode
6. Review       Present full session summary to learner
7. Confirm      Wait for explicit confirmation
8. Write        Write session file; update topic file
```

---

## Hard Rules Summary

| Code | Rule |
|------|------|
| HARD-001 | `user_confirmed` requires explicit learner statement |
| HARD-002 | `evidence` requires a real artifact |
| HARD-003 | User Layer writes require confirmation |
| HARD-004 | Always use stable IDs |
| HARD-005 | No topic mixing in one session |
| HARD-006 | No claiming understanding from explanation quality |
| HARD-007 | Consolidation before session end |
| HARD-008 | No API key exposure |
| HARD-009 | Blank profile → run onboarding |
| HARD-010 | likely_learned / user_confirmed / evidence are three separate fields |
| HARD-011 | No raw transcript as session record |
| HARD-012 | No cross-topic session loading |

---

## Soft Rules Summary

Keys use YAML path notation matching `config/settings.yml`.

| Setting path | Default |
|--------------|---------|
| `review.default_days` | 4 |
| `topics.max_active` | 5 |
| `context.sessions_loaded` | 3 |
| `topics.auto_archive_threshold` | 10 |
| `session.consolidation_required` | true |
| `session.evidence_required_for_mastery` | true |

---

## Terminal-First Workflow (v0)

```
1. Open Claude Code or Codex in Learning-Ops/
2. Agent reads CLAUDE.md / AGENTS.md → loads LEARNING_AGENT.md
3. Context loaded: profile + topic + last N sessions
4. Learning session proceeds
5. Learner says "consolidate"
6. Agent produces structured session summary
7. Learner confirms
8. Files written
```

No automation in v0. Manual consolidation validates the schema before any scripting.

---

## Later Browser Workflow

```
Browser chat window
      ↓
Local backend (Node / Python / Bun — see server/README.md)
  GET  /context?topic=<id>    → concatenated .md files for context
  POST /chat                  → Claude or OpenAI API call with context, streamed
  POST /session               → write session file after confirmation
  PATCH /topic/:id            → update topic file after confirmation
  GET  /topics                → active topics list
  GET  /review                → overdue review items
      ↓
Same learner/, topics/, sessions/ files
```

The browser does not introduce a separate database. The `.md` files remain the single source of truth.

---

## ID Format

```
session_id  →  YYYY-MM-DD-<topic_id>-<NNN>    e.g.  2026-04-27-ai_agents-001
topic_id    →  snake_case                       e.g.  ai_agents, german_b2
goal_id     →  snake_case, descriptive          e.g.  ai_portfolio_2026
```

---

## Evidence Types

| Type | Meaning |
|------|---------|
| `explanation` | Learner explained concept in own words |
| `quiz` | Learner answered practice questions |
| `project` | Learner built a working artifact |
| `code` | Learner wrote code |
| `summary` | Learner wrote notes or summary |
| `recall` | Learner recalled correctly after delay |
| `correction` | Learner found and corrected own mistake |
| `application` | Learner applied concept in real task |

---

## Risks and Mitigations

| Risk | Mitigation |
|------|------------|
| LLM overclaims learning | Three-field distinction enforced in consolidate mode |
| Context too large | Topic file trimmed; auto-archive threshold in settings |
| Schema drift | Fixed templates; no freeform fields in session records |
| Topic confusion | topic_id required on every session record |
| ID collision | Sequential NNN per topic per date |
| Browser write without backend | Do not build browser UI until CLI loop is validated |

---

## What Not to Build in v0

- Vector database
- Multi-agent architecture
- A2A protocol integration
- Calendar / notification integration
- Automatic session detection
- Complex scoring algorithms
- Knowledge graph
- Complex UI

---

## Validation Criteria

v0 is successful when the system can reliably answer:

- What was I learning last session?
- What did I ask about?
- What is still unclear?
- What should I do next?
- What should I review today?

If those five questions can be answered correctly from the file state, v0 works.
