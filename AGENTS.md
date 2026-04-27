# LearningOps for Codex / OpenAI Agents

Read `LEARNING_AGENT.md` for canonical instructions, routing rules, onboarding flow, hard rules, and session lifecycle.

## OpenAI / Codex-specific notes

- This file is a thin wrapper. All shared behavior lives in `LEARNING_AGENT.md`.
- Use `OPENAI_API_KEY` from the `.env` file. See `.env.example` for setup.
- All file reads and writes follow `DATA_CONTRACT.md`. Do not create parallel data structures.
- Reuse the existing `learner/`, `topics/`, and `sessions/` folders. Do not create a Codex-only parallel structure.
- The self-hosted browser UI uses the same files via a local backend. See `server/README.md`.
- If a Codex-specific instruction diverges from `LEARNING_AGENT.md`, treat `LEARNING_AGENT.md` as canonical.
