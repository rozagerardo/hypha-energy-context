---
name: henergy-task-resume
description: Resume an in-progress ticket in a new session — loads existing ticket context, assesses the current SDD phase, surfaces open threads and pending decisions, and briefs what's next. Use when continuing work from a prior session: "I'm continuing #X", "let's resume X", "I'm back on this ticket", "picking up where I left off", "continue #X", or when starting Phase B (implementation) after Phase A (spec) was completed in a prior session or tool.
audience: dev
---

# henergy-task-resume — resume an in-progress ticket

**Triggers:** "I'm continuing #X", "let's resume X", "I'm back on this ticket", "picking up where
I left off", "continue #X", starting Phase B after Phase A was completed in a prior session.

**Who it's for:** Developers resuming existing work in a new AI session.

Loads the existing ticket context, assesses the current SDD phase, surfaces open threads and pending
decisions, and briefs the session on what's next — **without re-scaffolding**. This is the session
opener for ongoing work:

| Skill | Role |
|---|---|
| `henergy-task-init` | First session — creates the scaffold |
| **`henergy-task-resume`** | **Every subsequent session — orients and resumes** |
| `henergy-checkpoint` | Every session close — records progress |

A long ticket: one `task-init` → many (`task-resume` → work → `checkpoint`) cycles.

**Procedure:** [`workflow/task-resume.md`](../../../workflow/task-resume.md)
