# Browser UI — Backend Interface Spec

This directory is a placeholder for the self-hosted browser UI backend.

**Not part of v0.** Build this after the terminal-first learning loop is validated.

---

## Why a backend is needed

The browser cannot read or write local files directly. A small local server bridges the browser UI and the markdown files.

The file layer (`learner/`, `topics/`, `sessions/`) does not change. The backend is a thin adapter only.

---

## Required endpoints

| Method | Path | Purpose |
|--------|------|---------|
| `GET` | `/context?topic=<topic_id>` | Returns concatenated markdown files for LLM context: profile + topic + last N sessions |
| `POST` | `/chat` | Sends a message to the LLM with assembled context; streams response |
| `POST` | `/session` | Writes a confirmed session summary to `sessions/<session_id>.md` |
| `PATCH` | `/topic/:topic_id` | Updates a topic file after consolidation confirmation |
| `GET` | `/topics` | Returns the list of active topics from `learner/profile.md` |
| `GET` | `/review` | Returns review items due today or overdue from all active topic files |

---

## Context assembly rule

The `/context` endpoint must assemble context in the same order as the terminal flow:

```
1. config/settings.yml
2. config/profile.yml
3. learner/profile.md
4. topics/<topic_id>.md
5. sessions/<last N sessions for topic>
```

N = `context.sessions_loaded` from `config/settings.yml`.

---

## Write confirmation rule

The `/session` and `/topic` endpoints must only write after the learner confirms in the browser UI. The backend should not auto-save without a confirmation flag in the request body.

---

## Implementation options

| Option | When to use |
|--------|-------------|
| Node.js + Express | Simplest, no dependencies beyond node |
| Python + FastAPI | If you prefer Python |
| Bun + Hono | Fastest startup, modern alternative |

All three options produce the same file output. Choose based on preference.

---

## Security note

This backend is local-only. Do not expose it to the internet. API keys are loaded from `.env` on the server side only. The browser never receives or handles API keys directly.
