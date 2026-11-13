---
name: henergy-checkpoint
description: Record progress at a checkpoint in this Hypha context repo — end of session/day, after a milestone or PR, at a phase transition, or before a context compaction. Use when a developer says "let's checkpoint", "wrap up for the day", "update the status and activity log", or "record where we are". Diffs against the last checkpoint and folds the work back into the shared SDD artifacts (spec header, decisions, activity log, platform area, pattern candidates). The wrap-up bookend to henergy-task-init and henergy-task-resume.
audience: dev
---

# henergy-checkpoint — record progress (wrap-up bookend)

**Triggers:** "let's checkpoint", "wrap up for the day", "update the status and activity log",
"record where we are". Also fires proactively at a phase transition, after a PR/milestone, or
before a context compaction.

**Who it's for:** Developers at the end of a session or milestone.

Diffs against the last checkpoint and folds the work back: updates the active ticket's `spec.md`
header, `decisions.md`, `analysis.md`; appends a dated entry to the activity log; updates the
platform view; flags Pattern-Card candidates; surfaces open threads. Offers to commit + push.
Heavy/private material stays in `.local/`.

The **closing bookend** of each session:

| Skill | Role |
|---|---|
| `henergy-task-init` | First session — creates the scaffold |
| `henergy-task-resume` | Every subsequent session — orients and resumes |
| **`henergy-checkpoint`** | **Every session close — records progress** |

**Procedure:** [`workflow/checkpoint.md`](../../../workflow/checkpoint.md)
