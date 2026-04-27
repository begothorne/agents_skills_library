---
topic_id: ""
topic_name: ""
status: active
goal_id: ""
created: YYYY-MM-DD
updated: YYYY-MM-DD
current_level: ""
target_level: ""
---

<!--
  USER LAYER — see DATA_CONTRACT.md
  Copy this file to topics/<topic_id>.md before first use.
  Replace all placeholder values. Remove this comment block.

  status options: active | paused | completed
  current_level / target_level: free text — e.g. "beginner", "intermediate", "conversational"
-->

## Known Concepts

<!--
  What the learner has been exposed to and likely understands.

  concept_id    : stable ID, format <topic_id>_c<NNN>, e.g. ai_agents_c001
  concept       : plain-language statement of what is understood
  confidence    : low | medium | high
                  low    = introduced, no confirmation yet
                  medium = learner confirmed verbally
                  high   = learner produced evidence (see session evidence)
  introduced    : session_id when first introduced
  last_reviewed : session_id of most recent review, or null
  evidence_session : session_id where evidence was produced, or null

  Confidence can only reach high when session.evidence_required_for_mastery
  is true (see config/settings.yml) AND evidence_session is set.

  Example:
  - concept_id: ai_agents_c001
    concept: RAG retrieves external information at query time
    confidence: medium
    introduced: 2026-04-27-ai_agents-001
    last_reviewed: null
    evidence_session: null

  - concept_id: ai_agents_c002
    concept: Memory stores user and session context across turns
    confidence: high
    introduced: 2026-04-27-ai_agents-001
    last_reviewed: 2026-05-01-ai_agents-002
    evidence_session: 2026-05-01-ai_agents-002
-->

known_concepts: []

---

## Gaps

<!--
  What the learner has not yet understood, or understands weakly.
  Add gaps during consolidation. Remove them when confidence reaches high.

  Example:
  - When to use a workflow instead of an agent
  - How to evaluate agent output quality
-->

gaps: []

---

## Next Actions

<!--
  What should happen next in this topic.
  Generated during consolidation. Mark completed by removing the item.

  action_id : stable ID, format <topic_id>_a<NNN>, e.g. ai_agents_a001
  type      : read | explain | build | review | compare | apply | publish
  priority  : high | medium | low
  source    : session_id that generated this action

  Example:
  - action_id: ai_agents_a001
    type: build
    description: Build a simple agent that uses one tool and one memory file
    priority: high
    source: 2026-04-27-ai_agents-001

  - action_id: ai_agents_a002
    type: explain
    description: Explain the difference between RAG and memory in your own words
    priority: medium
    source: 2026-04-27-ai_agents-001
-->

next_actions: []

---

## Review Queue

<!--
  Concepts that should be revisited on or before the review_by date.
  Populated automatically during consolidation.
  Remove items once they have been reviewed and confidence has been updated.

  review_id  : stable ID, format <topic_id>_r<NNN>, e.g. ai_agents_r001
  concept_id : links to known_concepts[].concept_id
  review_by  : date calculated from session date + review.default_days (config/settings.yml)
  status     : pending | done | overdue

  Example:
  - review_id: ai_agents_r001
    concept_id: ai_agents_c001
    item: Difference between RAG and memory
    review_by: 2026-05-01
    reason: New concept, not yet confirmed by learner
    status: pending
-->

review_queue: []

---

## Session History (index)

<!--
  Auto-maintained index of session files for this topic.
  Full session content is in sessions/<session_id>.md.

  Example:
  - session_id: 2026-04-27-ai_agents-001
    date: 2026-04-27
    summary: Covered RAG vs memory vs tools vs agents. Still unclear on evaluation.
-->

sessions: []
