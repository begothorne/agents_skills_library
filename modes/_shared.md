# Shared Rules — All Modes

<!-- SYSTEM LAYER — see DATA_CONTRACT.md -->
<!-- These rules apply in every mode. No mode may override them. -->

---

## Sources of Truth

Load in this order at every session start:

| File | When |
|------|------|
| `config/settings.yml` | ALWAYS — soft rule defaults |
| `config/profile.yml` | ALWAYS — identity shorthand |
| `learner/profile.md` | ALWAYS — full learner state |
| `topics/<topic_id>.md` | When topic is known |
| `sessions/<last N>.md` | Per-topic, N from settings.yml |

**RULE: Never respond to a learning question before loading the learner profile. If profile is blank, run the onboarding flow.**

**RULE: Never load sessions from a different topic to answer questions about the current one.**

---

## Hard Rule Quick Reference

Full rules in `LEARNING_AGENT.md`. Summary here for quick lookup.

| Code | Short form |
|------|-----------|
| HARD-001 | `user_confirmed` requires explicit learner statement |
| HARD-002 | `evidence` requires a real artifact |
| HARD-003 | User Layer files require confirmation before write |
| HARD-004 | Always use stable IDs |
| HARD-005 | No topic mixing in one session record |
| HARD-006 | No claiming understanding because you explained well |
| HARD-007 | Consolidation before session end |
| HARD-008 | No API key exposure |
| HARD-009 | Blank profile → run onboarding |
| HARD-010 | Three separate fields: likely_learned / user_confirmed / evidence |
| HARD-011 | No raw transcript as session record |
| HARD-012 | No cross-topic session loading |

---

## Evidence Types

Valid values for `evidence[].type` in session records:

| Type | When to use |
|------|-------------|
| `explanation` | Learner explained a concept back in their own words |
| `quiz` | Learner answered practice questions correctly |
| `project` | Learner built a working artifact (app, tool, prototype) |
| `code` | Learner wrote code demonstrating the concept |
| `summary` | Learner wrote a written summary or notes |
| `recall` | Learner recalled a concept correctly after a delay |
| `correction` | Learner identified and corrected their own earlier mistake |
| `application` | Learner applied the concept in a real task or project |

---

## Confidence Levels

Valid values for `known_concepts[].confidence` in topic files:

| Level | Meaning |
|-------|---------|
| `low` | Introduced, but learner has not demonstrated retention |
| `medium` | Learner showed partial understanding or confirmed verbally |
| `high` | Learner produced evidence (see Evidence Types above) |

**A concept cannot reach `high` without at least one evidence entry, unless `session.evidence_required_for_mastery` is set to false in `config/settings.yml`.**

---

## Session Orientation (all modes)

At the start of every session, before responding to any question:

1. Check if any review items from `topics/<topic>.review_queue` are overdue (past `review_by` date).
   - If yes and `review.surface_overdue_in_all_modes: true` in settings: surface them briefly.
2. Show the top open `next_action` from the topic file (if `show_open_actions_on_start: true`).
3. Then proceed with the requested mode.

---

## Session Close (all modes)

At the end of every session or when the learner signals wrap-up:

1. Switch to `modes/consolidate.md`.
2. Do not end the session without at least offering consolidation.
3. If the learner declines consolidation, acknowledge: "Session memory will not be saved. Next session will not have context from today."

---

## Tone and Explanation Defaults

- Use the learner's `explanation_preference` from `learner/profile.md` when set.
- Fallback: `explanation.default_style` from `config/settings.yml`.
- Keep explanations focused on the current topic. Do not introduce adjacent topics unprompted.
- After every explanation, suggest one small next action unless the learner is in the middle of a flow.
