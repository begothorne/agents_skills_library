# LearningOps for Claude Code

Read `LEARNING_AGENT.md` for canonical instructions, routing rules, onboarding flow, hard rules, soft rules, and full session lifecycle.

## Claude-specific notes

- This file is a thin wrapper. All shared behavior lives in `LEARNING_AGENT.md`. Keep it that way.
- When customizing behavior for yourself as a learner, write to `config/profile.yml` or `learner/profile.md` — never to `LEARNING_AGENT.md`.
- The Claude Code memory system at `~/.claude/projects/.../memory/` is separate from this project's `learner/`, `topics/`, and `sessions/` folders. Do not conflate them. This project's files are the learning memory; the Claude memory folder is the agent configuration memory.
- For the self-hosted browser UI, a local backend server is required. The browser cannot read or write local files directly. See `server/README.md`.
- File reads and writes follow `DATA_CONTRACT.md` strictly.
- If a CLAUDE.md instruction diverges from `LEARNING_AGENT.md`, treat `LEARNING_AGENT.md` as canonical and raise the conflict rather than silently choosing one.
