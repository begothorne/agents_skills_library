# Mode: Consolidate

<!-- SYSTEM LAYER — see DATA_CONTRACT.md -->

Triggered at the end of every session. Converts the session into a structured learning record.

This is one of the most important modes. If consolidation is skipped, the session produces no lasting memory.

---

## Entry Conditions

Triggered when the learner says: "consolidate", "wrap up", "save session", "end session", "summarize what we did", or when the session is ending.

---

## Consolidation Steps

Follow these steps in order. Do not skip steps.

### Step 1 — Reconstruct the session

Review the full conversation for this session and identify:

- The main question or request that opened the session
- The key concepts that were introduced or discussed
- Any confusion signals the learner showed
- Any explicit understanding signals ("I see", "that makes sense", correct restatements)
- Any artifacts produced (code, explanation, quiz answers, summaries)
- Any questions that were raised but not resolved

### Step 2 — Fill the session fields

Produce output matching the schema in `sessions/_template.md`.

For each field, apply the following rules strictly:

| Field | Rule |
|-------|------|
| `user_asked` | The main question or request. One to three sentences. |
| `answer_summary` | Core of what was explained. Concise, not a transcript. |
| `likely_learned` | What the learner was probably exposed to. Inference allowed. |
| `user_confirmed` | **HARD-001**: Empty list unless learner explicitly stated understanding. |
| `evidence` | **HARD-002**: Empty list unless a real artifact was produced. Use correct type. |
| `still_unclear` | Specific gaps observed. Be precise. |
| `next_action` | Single most important next step. One item only. |
| `review_items` | New concepts introduced. Default `review_by` = today + `review.default_days`. |
| `open_questions` | Questions raised but unresolved. |

### Step 3 — Assign the session ID

Format: `YYYY-MM-DD-<topic_id>-<NNN>`

Check existing sessions for that topic and date to get the correct sequential number.

### Step 4 — Prepare topic file updates

Based on the session, prepare updates to `topics/<topic_id>.md`:

- Add new concepts to `known_concepts` at `confidence: low` (or `medium` if user_confirmed)
- Add specific gaps to `gaps` list
- Update or add `next_actions` (replace completed ones, add new ones)
- Add `review_items` to `review_queue`
- Update `updated` date
- Add session index entry to `sessions` list

### Step 5 — Present for confirmation

Show the learner:
1. The full session record (all fields)
2. The proposed topic file updates

Ask: "Does this look right? Should I adjust anything before saving?"

Do not write any files before this confirmation.

### Step 6 — Write after confirmation

After explicit confirmation:
1. Write `sessions/<session_id>.md`
2. Update `topics/<topic_id>.md`
3. Confirm: "Session saved as `<session_id>`."

---

## Evidence Check

Before finalizing, verify:

- Is `user_confirmed` based on something the learner actually said? If not, clear it.
- Is `evidence` based on something the learner actually produced? If not, clear it.
- Is `likely_learned` the only field with agent inferences? That is correct.

These three fields represent three different levels of certainty. Conflating them is the most common consolidation failure.

---

## Minimal Consolidation (Quick Save)

If the learner wants a quick save without review:

```
Learner: "Just save it quickly"
Agent: "I'll save a minimal record with what you asked and what was covered.
        user_confirmed and evidence will be empty — you can update them next session.
        OK to proceed?"
```

A minimal record is acceptable. A missing record is not.

---

## When Consolidation Is Declined

If the learner explicitly declines consolidation:

```
"Understood. This session will not be saved. When we next work on <topic>,
I will not have context from today's session. You can always consolidate
manually at the start of the next session by saying 'consolidate last session'."
```

Do not write anything. Do not save a partial record.

---

## Post-Consolidation: What Happens Next

After saving:

1. Show the single `next_action` from the session record.
2. Show any review items due in the next 2 days.
3. Ask: "Anything else for today, or are we done?"
