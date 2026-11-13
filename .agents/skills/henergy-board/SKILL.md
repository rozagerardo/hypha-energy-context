---
name: henergy-board
description: Read or update Hypha's GitHub issues, PRs, and project board (#14 "Hypha Platform — Active Work") via the gh CLI. Use when a technical developer (github.enabled in their config) says "update the board", "sync this to GitHub", "move #X to In Review", "open the PR", "what's assigned to me", or "check GitHub". Reads are free; writes ALWAYS confirm first. Also fires as a soft step inside henergy-task-init and henergy-checkpoint.
audience: dev
---

# henergy-board — GitHub board & issue sync (via gh CLI)

**Triggers:** "update the board", "sync this to GitHub", "move #X to In Review", "open the PR",
"what's assigned to me", "check GitHub".

**Who it's for:** Technical developers with `github.enabled: true` in their config. **Skip silently**
for others.

Reads/writes GitHub issues, PRs, and the project board (#14 "Hypha Platform — Active Work") using
the `gh` CLI — skills-over-MCP, not a GitHub MCP server. Reads are free; every write states the
exact change and asks for a yes first (honoring the developer's `decision_posture`).

Also fires as a soft step inside `henergy-task-init` and `henergy-checkpoint` (when gated-in).

**Gating:** Check the gate first — `github.enabled: true` in `AGENTS.local.md` + technical
profile. Confirm `gh` is installed + authenticated (`gh auth status`).

**Procedure:** [`workflow/github-sync.md`](../../../workflow/github-sync.md)
