---
name: breadcrumb
disable-model-invocation: true
description: >-
  When explicitly invoked, summarize the session's goal, task hierarchy,
  progress, stopping point, and next action for the user's return.
---

# Breadcrumb

Only on explicit user invocation, leave a chat summary of the preceding work
that lets the user resume without rereading the conversation.

Write exactly five bullets. Aim for one or two sentences each, expanding only
to preserve constraints, task hierarchy, or an unambiguous stopping point.

- **Goal:** Overall task and intended outcome.
- **Current focus:** Active subtask, or the goal itself if none. For detours,
  explain why and preserve the nested task chain. Include user constraints
  and the agreed working method needed to resume.
- **Progress:** Completed work, key decisions, blockers, and unresolved issues.
  Distinguish proposed, applied, and verified changes; include relevant check
  outcomes and whether later changes made that evidence stale.
- **Stopped at:** Exact item and location; whether examining, editing, checking,
  or awaiting an answer. For sequential work, distinguish the last completed
  item from the current one.
- **Next:** First concrete action, respecting agreed order and pending decisions
  or prerequisites. For detours, identify the immediate return point and its
  conditions.

If complete, identify the final outcome and say there is no agreed next action
unless follow-up was already agreed. Do not invent tasks or relationships.

When useful, append a short excerpt with its source location and enough surrounding
context to identify the stopping point. Quote faithfully and mark omissions.

Use conversation context and only targeted read-only lookups needed to recover
the stopping point. Mark unknowns; never fabricate quotations. Distinguish
last-observed from freshly checked status, including running work, without
implying it continues after the session ends. Do not rerun checks just for this
summary.

Output in chat, then stop. Do not advance the task or write files unless requested.
