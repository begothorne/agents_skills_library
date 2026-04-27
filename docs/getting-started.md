# Getting Started

This guide covers how to run your first LearningOps session from the terminal, and what to do before the browser UI is added later.

---

## Prerequisites

- Claude Code CLI installed (`claude` command available), or Codex CLI, or another LLM tool
- An Anthropic API key (for Claude) or OpenAI API key (for Codex)
- This project directory open in your terminal

---

## Step 1 — Set up your API key

Copy `.env.example` to `.env` and fill in your key:

```bash
cp .env.example .env
```

**For Claude (default):**
```
ANTHROPIC_API_KEY=sk-ant-...
LEARNINGOPS_PROVIDER=anthropic
```

**For OpenAI / Codex:**
```
OPENAI_API_KEY=sk-...
LEARNINGOPS_PROVIDER=openai
```

You only need one provider key. The `.env` file is in `.gitignore` and will not be committed.

---

## Step 2 — Fill in your identity config (optional, fast)

Edit `config/profile.yml` and fill in your name and timezone. This is optional — the agent will ask you during the onboarding flow if it is blank.

---

## Step 3 — Start your first session

Open Claude Code in this directory:

```bash
cd /path/to/Learning-Ops
claude
```

The agent will:
1. Read `CLAUDE.md` → load `LEARNING_AGENT.md`
2. Check `learner/profile.md` for content
3. If blank → run the onboarding flow and ask you questions
4. If filled → proceed to the session

---

## Step 4 — Onboarding (first session only)

The agent will ask you:
- What do you want to learn?
- Why is that important now?
- What is your current level?
- What level do you want to reach?
- How do you prefer to learn?
- What kind of output matters to you?

Answer honestly. The agent will summarize and ask you to confirm before writing anything to `learner/profile.md`.

---

## Step 5 — Learn

Ask your question or start the topic:

```
Explain how AI agents work.
```

or

```
I want to learn German for work conversations.
```

or

```
Plan how I should learn Python automation.
```

---

## Step 6 — Consolidate at the end

When you are done for the day, say:

```
Consolidate session
```

or

```
Wrap up
```

The agent will:
1. Produce a structured session summary
2. Show you what it plans to write
3. Ask for your confirmation
4. Write to `sessions/` and update your topic file

**Do not skip this step.** Without it, the session produces no lasting memory.

---

## Step 7 — Next session

Start the next session normally. The agent will:
1. Load your profile
2. Load the relevant topic file
3. Load the last 3 sessions for that topic
4. Show any overdue review items
5. Show the open next action from last time

You resume exactly where you left off.

---

## Using Codex instead of Claude Code

Same workflow. Open Codex in this directory:

```bash
codex
```

It reads `AGENTS.md` → loads `LEARNING_AGENT.md`. The rules and file structure are identical.

---

## Browser UI (later)

The browser UI is not part of v0. When you are ready:
1. Read `server/README.md` for the backend interface spec
2. Build or run the local backend
3. Set `LEARNINGOPS_PORT` and `LEARNINGOPS_PROVIDER` in `.env`
4. Open the browser UI — it reads and writes the same `learner/`, `topics/`, and `sessions/` files

The terminal and browser share the same data. You can switch between them freely.

---

## Recommended first week

| Day | Action |
|-----|--------|
| Day 1 | Onboarding session — fill your profile, start one topic |
| Day 2 | Continue the topic, consolidate |
| Day 3 | Try a review session on Day 1 concepts |
| Day 4–5 | Add a second topic if the first is going well |
| Day 7 | Check your topic file — is it capturing what matters? |

Validate the loop works before adding automation or the browser UI.
