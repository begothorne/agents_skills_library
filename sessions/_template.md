---
session_id: YYYY-MM-DD-<topic_id>-<NNN>
topic_id: ""
goal_id: ""
date: YYYY-MM-DD
---

<!--
  USER LAYER — see DATA_CONTRACT.md
  This file is created by the consolidation process (modes/consolidate.md).
  One file per session. Filename = session_id.
  Remove this comment block from real session files.

  session_id format: YYYY-MM-DD-<topic_id>-<NNN>
  Example: 2026-04-27-ai_agents-001
-->

## What the Learner Asked

<!--
  The main question or request that started the session.
  One to three sentences. Do not summarize the full conversation here.

  Example:
  What is the difference between RAG, memory, tools, and agents?
-->

user_asked: ""

---

## Answer Summary

<!--
  The core of what was explained or worked through.
  Concise — this is a reference record, not a full transcript.

  Example:
  RAG retrieves external information at query time. Memory stores user and session
  context across turns. Tools allow the model to perform actions in the world.
  Agents combine reasoning, memory, tools, and multi-step execution.
-->

answer_summary: ""

---

## What Was Likely Learned

<!--
  What the learner was probably exposed to and may have internalized.
  This is an inference — based on what was covered, not what was confirmed.
  Use this field freely. Do not confuse it with user_confirmed or evidence.

  Example:
  - Agents are not just chatbots — they plan, call tools, and complete multi-step tasks
  - RAG and memory serve different purposes and are not interchangeable
-->

likely_learned: []

---

## User Confirmed

<!--
  HARD RULE (HARD-001): Only populate this field if the learner explicitly
  stated understanding during the session.
  Valid signals: "yes I understand", "that makes sense", "I get it now",
  or the learner successfully restated the concept.

  Leave as empty list if nothing was explicitly confirmed.

  concept_id : links to topics/<topic_id>.md → known_concepts[].concept_id
               this is how topic confidence moves from low → medium
  statement  : what the learner actually said or did

  Example:
  - concept_id: ai_agents_c001
    statement: "Yes, RAG retrieves from external sources, memory is for within-session context"
-->

user_confirmed: []

---

## Evidence

<!--
  HARD RULE (HARD-002): Only populate this field if the learner produced
  a real artifact during the session.
  Valid types: explanation, quiz, project, code, summary, recall, correction, application

  Leave as empty list if no artifact was produced.

  concept_id  : links to topics/<topic_id>.md → known_concepts[].concept_id
                this is how topic confidence moves to high (evidence_session is set to this session_id)
  type        : see evidence_types in config/settings.yml
  date        : date evidence was produced (usually = session date)
  description : what the learner produced or demonstrated
  artifact    : file path or URL if a tangible artifact exists, else null

  Example:
  - concept_id: ai_agents_c001
    type: explanation
    date: YYYY-MM-DD
    description: Learner explained agent vs RAG distinction unprompted and correctly
    artifact: null

  - concept_id: ai_agents_c003
    type: code
    date: YYYY-MM-DD
    description: Wrote a minimal agent loop in Python using tool calls
    artifact: "scripts/agent_demo.py"
-->

evidence: []

---

## Still Unclear

<!--
  What the learner did not fully understand, or where confusion was visible.
  Be specific — this feeds directly into the topic gap list.

  Example:
  - When a workflow automation is better than an agent
  - How to evaluate agent output quality in production
-->

still_unclear: []

---

## Next Action

<!--
  The single most important thing the learner should do next.
  One item only. If there are multiple, choose the highest priority.

  type options: read | explain | build | review | compare | apply | publish

  Example:
  type: build
  description: Create a minimal agent that uses one tool and stores session memory
  priority: high
-->

next_action:
  type: ""
  description: ""
  priority: ""

---

## Review Items

<!--
  Concepts from this session that should be revisited.
  review_by is auto-calculated: session date + review.default_days (config/settings.yml).
  These are copied to topics/<topic_id>.md → review_queue during consolidation.

  review_id  : stable ID, format <topic_id>_r<NNN>, assign sequentially
  concept_id : links to known_concepts[].concept_id

  Example:
  - review_id: ai_agents_r001
    concept_id: ai_agents_c001
    item: Difference between RAG and memory
    review_by: 2026-05-01
    reason: New concept, not yet confirmed
-->

review_items: []

---

## Open Questions

<!--
  Questions that were raised but not resolved in this session.
  These carry forward to the next session on this topic.

  Example:
  - When does agent overhead outweigh the benefit?
  - Is there a standard way to evaluate agent chains?
-->

open_questions: []
