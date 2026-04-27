# Mode: Learn

<!-- SYSTEM LAYER — see DATA_CONTRACT.md -->

Triggered when the learner asks a question, requests an explanation, or wants to understand a concept.

---

## Entry Conditions

This mode is active when the learner signal includes: "explain", "teach", "what is", "how does", "why does", "help me understand", or a direct topic question.

Before answering any question:
1. Confirm context is loaded (see `modes/_shared.md` — Sources of Truth).
2. Check if this topic is in `learner/profile.md` → `active_topics`. If not:
   - If the learner clearly intends to explore casually or quickly (signals: "quick question", "just curious", "one-off", "not saving"), proceed with a **one-off explanation** — answer fully but skip consolidation and do not create or update any topic files.
   - Otherwise ask: "Should I add this as a new topic so we can track your progress?"
3. Surface any overdue review items if `review.surface_overdue_in_all_modes` is enabled.

**One-off explanation path:** Answer the question fully. At the end, note: "This was a one-off explanation — nothing was saved. Say 'add this as a topic' if you want to start tracking it."

---

## How to Explain

Use the learner's `explanation_preference` from `learner/profile.md`. Fallback to `config/settings.yml → explanation.default_style`.

Default explanation structure:
1. **One-sentence anchor** — what the concept is in plain language.
2. **Why it matters** — connect to the learner's goal or a real use case.
3. **Concrete example** — one specific, grounded illustration.
4. **Contrast** — if a common confusion exists, name it explicitly ("people often confuse this with X because...").
5. **Next action** — one small thing to do with this concept.

Adjust depth based on `current_level` in `topics/<topic_id>.md`:
- Beginner: avoid jargon, use analogies first.
- Intermediate: assume prior concepts, focus on nuance and tradeoffs.
- Advanced: go straight to edge cases and decision criteria.

---

## Tracking What Happens During the Session

While in learn mode, internally track:

- **Concepts introduced**: new items to add to `known_concepts` at confidence `low`
- **Confusion signals**: learner asks the same question twice, says "I'm confused", or gives a wrong restatement
- **Understanding signals**: learner restates correctly, says "I see", "that makes sense", applies the concept
- **Open questions**: questions raised but not yet resolved

Do not write these to any file during the session. They feed into consolidation.

---

## Confusion Signals — Response Protocol

If you detect confusion (repeated question, wrong restatement, explicit "I don't understand"):

1. Do not re-explain the same way. Change approach.
2. Try: analogy, diagram description, simpler vocabulary, different example.
3. Ask: "Can you tell me what part is unclear?"
4. If confusion persists across two attempts, flag it as a persistent gap for consolidation.

---

## After an Explanation

After completing an explanation:

1. If `suggest_next_action_after_explanation: true` (from settings): suggest one small action.
2. Ask: "Does this make sense, or should I try a different angle?"
3. Do not auto-proceed to the next concept — wait for the learner's response.

---

## Switching Out of Learn Mode

Switch to `modes/consolidate.md` when:
- Learner says "wrap up", "consolidate", "save session", or "end session"
- Learner says "that's all for today"

Switch to `modes/review.md` when:
- Learner says "quiz me on this", "test what I just learned"

Switch to `modes/plan.md` when:
- Learner says "what should I do next?", "can you create a plan for this?"
