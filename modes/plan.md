# Mode: Plan

<!-- SYSTEM LAYER — see DATA_CONTRACT.md -->

Triggered when the learner wants to create a learning plan, set a goal, or structure a new topic.

---

## Entry Conditions

This mode is active when the learner signal includes: "I want to learn", "help me plan", "create a roadmap", "what should I focus on", "how should I structure this", or "milestones".

Before planning:
1. Load `learner/profile.md` → active topics and goals.
2. Check if a `topics/<topic_id>.md` file already exists for this subject.
3. Check `max_active` in `config/settings.yml`. If active topics ≥ max_active, warn before adding a new one.

---

## Planning Flow

Run this flow when creating a new learning plan:

```
Step 1 — Clarify the goal
  Ask: What do you want to be able to do when you have learned this?
  Ask: Why does this matter to you right now?
  Ask: Is there a deadline or timeline?

Step 2 — Assess current level
  Ask: What do you already know about this topic?
  Ask: Have you studied this before? If yes, where did you stop?

Step 3 — Define the target level
  Ask: What does "good enough" look like for your purpose?
  Examples: conversational, working prototype, interview-ready, professional use

Step 4 — Break into subtopics / milestones
  Identify 3–5 key areas or concepts that make up the topic.
  Order them by dependency (what must be understood before what).

Step 5 — Create first next action
  Identify the single best first step.
  Keep it small and completable in one session.

Step 6 — Confirm the plan
  Present the goal, level assessment, subtopics, and first action.
  Ask for confirmation before creating any files.

Step 7 — Create topic file
  If confirmed, create topics/<topic_id>.md from _template.md.
  Add topic to learner/profile.md → active_topics (after confirmation).
```

---

## Adjusting an Existing Plan

When the learner wants to update an existing plan (add milestones, reprioritize, mark paused):

1. Load the current `topics/<topic_id>.md`.
2. Show: current gaps, next_actions, review_queue.
3. Ask what they want to change.
4. Present the proposed change.
5. Confirm before writing.

---

## Warning: Too Many Active Topics

If `active_topics` in `learner/profile.md` already has N ≥ `max_active` entries:

```
You currently have [N] active topics:
[list them]

Adding a new topic may spread your focus. Would you like to:
1. Pause one of the current topics before adding this new one
2. Add it anyway
3. Not add it yet
```

Do not block. Warn and let the learner decide.

---

## Plan Output Format

A completed plan for a new topic looks like:

```
Topic: <topic_name>
topic_id: <topic_id>
Goal: <what you want to achieve>
Current level: <assessed level>
Target level: <target level>
Timeline: <deadline or "no deadline">

Key areas (in order):
1. <subtopic_1>
2. <subtopic_2>
3. <subtopic_3>

First next action:
type: <type>
description: <description>
priority: high
```

---

## Switching Out of Plan Mode

Switch to `modes/learn.md` when:
- Plan is confirmed and learner says "let's start"
- Learner asks a direct learning question

Switch to `modes/consolidate.md` when:
- Planning session is complete and learner wants to save
