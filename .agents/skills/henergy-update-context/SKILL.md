---
name: henergy-update-context
description: Keep the LOCAL copy of the Hypha context repo current (git pull) so the AI reads up-to-date context, not a stale snapshot. Use at session start, before starting a task, before merging, or on demand ("update my context", "pull latest", "am I up to date?"). Profile-aware — automatic fast-forward for non-technical users on a clean main; a reminder/offer for technical users (never clobbering work in progress). Distinct from henergy-board (which updates the GitHub board).
audience: general
---

# henergy-update-context — refresh the local context (git pull)

**Triggers:** "update my context", "pull latest", "am I up to date?", "refresh context", "get latest
changes". Also runs automatically inside `henergy-start`, `henergy-task-init`, and `henergy-task-resume`.

**Who it's for:** Everyone. Also a cross-cutting step that fires as part of other skills.

Keeps the **local context files** current — *not* the GitHub board (that's
[`henergy-board`](../henergy-board/SKILL.md)). Behavior depends on developer config:

- **Non-technical / `auto`:** fast-forward `git pull` on clean `main`, briefly confirm.
- **Technical / `remind`:** report ahead/behind + tree state, **offer** to pull — never silently
  over uncommitted work or a feature branch.
- **No git (ZIP download):** say local sync isn't possible.

After the pull, **if the developer is profile A or B**, run the satellite refresh from
[`workflow/satellite.md`](../../../workflow/satellite.md) (procedure 2) in each code repo that has a
`.henergy-context.local/`: update `personal.md`, `workflow.md`, `pointers.md`; 30-day cleanup of the specs files.

**Procedure:** [`workflow/sync-context.md`](../../../workflow/sync-context.md)
