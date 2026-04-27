# Data Contract

This document defines which files belong to the **user layer** (never auto-modified) and which belong to the **system layer** (safe to update).

---

## User Layer — NEVER auto-modified without explicit confirmation

These files contain your personal learning data. Agents and scripts may **read** them freely during normal session operation. No process may **modify or delete** them without presenting the proposed change and receiving explicit learner confirmation in the current session.

| File / Path | Purpose |
|-------------|---------|
| `config/profile.yml` | Identity shorthand: name, timezone, goals summary |
| `learner/profile.md` | Full learner profile: goals, style, strengths, difficulties, active topics |
| `topics/*.md` | Per-topic learning state: known concepts, gaps, next actions, review queue |
| `sessions/*.md` | Per-session records: what was asked, likely learned, confirmed, evidence, next step |
| `archive/*.md` | Archived sessions and topics (read-only after archival) |

**The rule: Agents and scripts may read User Layer files at any time. They may only write, modify, or delete them after presenting the proposed change and receiving an explicit "yes" from the learner in the current session.**

---

## System Layer — Safe to read; updatable by system releases

These files contain agent instructions, mode logic, templates, and configuration defaults. They can be safely replaced or updated without losing learner data.

| File / Path | Purpose |
|-------------|---------|
| `LEARNING_AGENT.md` | Canonical agent instructions |
| `CLAUDE.md` | Claude Code thin wrapper |
| `AGENTS.md` | Codex thin wrapper |
| `DATA_CONTRACT.md` | This file |
| `CHANGELOG.md` | Version history |
| `modes/_shared.md` | Shared rules across all modes |
| `modes/learn.md` | Learning session mode |
| `modes/review.md` | Review and recall mode |
| `modes/plan.md` | Goal planning mode |
| `modes/consolidate.md` | End-of-session consolidation mode |
| `config/settings.yml` | Soft rule defaults |
| `topics/_template.md` | Topic file template |
| `sessions/_template.md` | Session file template |
| `docs/*.md` | Documentation |
| `scripts/*` | Utility scripts |
| `server/*` | Browser UI backend |
| `.env.example` | API key template (never the real `.env`) |

---

## The Rule (short form)

```
User Layer  →  never touch without confirmation
System Layer  →  safe to read and update
```

---

## Browser UI note

The browser UI accesses these files via a local backend server (not directly). The same contract applies: the backend may read any file, but may only write to User Layer files after the learner confirms the proposed change through the UI.

---

## What never goes in any learning file

- API keys or credentials
- Passwords or tokens
- Sensitive personal health data
- Financial details
- Private third-party information
