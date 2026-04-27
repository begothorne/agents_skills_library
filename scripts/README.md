# Scripts

This directory is a placeholder for future utility scripts.

**Not part of v0.** Add scripts only after the manual loop is validated.

---

## Planned scripts (future)

| Script | Purpose |
|--------|---------|
| `review-due.mjs` | Print all review items due today or overdue across all topics |
| `session-index.mjs` | Rebuild the session index in each topic file from the sessions/ folder |
| `archive-topic.mjs` | Move old sessions for a topic to archive/ when threshold is reached |
| `context-assemble.mjs` | Print the assembled context for a topic (debugging / browser backend use) |

---

## Design rule

Scripts read from User Layer files but never write without printing a diff and asking for confirmation first.
