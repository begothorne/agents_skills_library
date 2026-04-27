# Mode: Review

<!-- SYSTEM LAYER — see DATA_CONTRACT.md -->

Triggered when the learner wants to test recall, revisit past learning, or check what they remember.

---

## Entry Conditions

This mode is active when the learner signal includes: "review", "quiz me", "test me", "what do I know about", "remind me", or "recall".

Before starting:
1. Load `topics/<topic_id>.md` → `review_queue` for overdue items.
2. Load `topics/<topic_id>.md` → `known_concepts` for concepts to test.
3. Load the last 1–2 sessions for context on what was recently covered.
4. State briefly: "I'll review [N] concepts from your last session on [topic]. Ready?"

---

## Review Question Types

Use these question types in order of increasing difficulty:

| Type | Example |
|------|---------|
| Recall | "What is the difference between RAG and memory?" |
| Application | "In what situation would you choose an agent over a workflow?" |
| Error detection | "Here is a description of how memory works. What is wrong with it?" |
| Comparison | "Compare tools and agents — what can tools do that agents cannot, and vice versa?" |
| Teaching back | "Explain to me how RAG works as if I have never heard of it." |

Start with recall. Move to application and teaching-back if the learner answers correctly.

---

## Scoring Recall

Do not assign numeric scores. Instead, after each answer:

| Learner response | Action |
|-----------------|--------|
| Correct and complete | Note as `recall` evidence. Increase confidence if appropriate. |
| Partially correct | Note gap. Ask a follow-up to test the missing part. |
| Incorrect | Do not immediately correct. Ask: "What do you think that means?" then gently redirect. |
| "I don't know" | Give a one-sentence hint. If still blank, move on and flag for re-review. |

---

## Updating Topic State After Review

After the review session ends, prepare an update for `topics/<topic_id>.md`:

- Move correctly recalled concepts toward `confidence: medium` or `high` (with `recall` as evidence type)
- Add items answered incorrectly to `gaps` if not already there
- Remove items from `review_queue` if they were successfully recalled
- Extend `review_by` dates for items that still need reinforcement

Present these updates to the learner and ask for confirmation before writing.

---

## Evidence from Review Sessions

A successful recall is valid evidence. Use `type: recall` in the session record.

Example:
```
evidence:
  - type: recall
    description: Correctly explained agent vs RAG distinction without prompting, 5 days after initial session
```

---

## Switching Out of Review Mode

Switch to `modes/consolidate.md` when:
- Review is complete and learner says "wrap up" or "save"
- Learner says "that's enough for today"

Switch to `modes/learn.md` when:
- Learner says "explain that again" or "I want to learn more about X"
