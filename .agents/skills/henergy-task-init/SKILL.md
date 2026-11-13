---
name: henergy-task-init
description: Scaffold an SDD ticket workspace when a developer STARTS a unit of work in this Hypha context repo. Use when they say "I'm starting #<issue>", "let's begin <ticket>", "new task", or "starting work on X". Reads their AGENTS.local.md, classifies the task, picks the SDD level, and scaffolds the ticket folder. (To resume an in-progress ticket in a new session, use henergy-task-resume instead. To wrap up / record progress, use henergy-checkpoint.)
audience: dev
---

# henergy-task-init — SDD ticket scaffolding (start of work)

**Triggers:** "I'm starting #X", "let's begin <ticket>", "new task", "starting work on X".

**Who it's for:** Developers starting a **net-new** unit of work (first session on a ticket). If the
ticket folder already exists and you're picking up from a prior session, use
[`henergy-task-resume`](../henergy-task-resume/SKILL.md) instead.

Classifies the task, picks the SDD level via the escalation trigger table, and scaffolds
`progress/members/<username>/tickets/<issue#>-<slug>/` with the right artifacts (`spec.md`,
`analysis.md`, `implementation-plan.md`, etc. as applicable). Logs it to the activity log. Also
fires a GitHub soft-step (if the developer is gated-in).

The **opening bookend** of a unit of work:
- `henergy-task-init` — first session (scaffold)
- `henergy-task-resume` — every subsequent session (orient + resume)
- `henergy-checkpoint` — every session close (record)

**Procedure:** [`workflow/task-init.md`](../../../workflow/task-init.md)
