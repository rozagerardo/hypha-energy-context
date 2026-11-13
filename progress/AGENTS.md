# AGENTS.md — `progress/`

> Read the root [`AGENTS.md`](../AGENTS.md) first for the global frame.

## What this directory is

`progress/` tracks **what is actually getting done and how it is evolving**: the record of work and its
context.

```
progress/
├── AGENTS.md       ← you are here
└── members/        ← per-member progress (what each person has been doing, and why)
    └── <username>/     SDD workspace: activity-log.md + tickets/<slug>/ folders
```

A **`platform/`** view (progress by area: website, platform/EMOS, ValueX, workspace stack) is added when
there is real progress to record by area, as in `hypha-context`. Don't scaffold it empty.

This is **progress, not a to-do board.** The "what's active now" view lives in `planning/`.

## Conventions

- **Date entries** and keep them factual. GitHub issues/PRs are the source of truth for detail; link
  rather than duplicate.
- **Keep GitHub in step (technical, opted-in).** With `github.enabled` in their `AGENTS.local.md`,
  recording progress pairs with updating the board/issue, via [`../workflow/github-sync.md`](../workflow/github-sync.md)
  (reads free, writes ask first). Note: the only known org board (#13) covers the website; many tasks
  (agreement blocks, workspace research) have no issue. Their source of truth is the agreement in
  `ger-hypha-energy/agreements/`, and their ticket slug is `b<block>-<slug>`.
- Neutral voice. Personal, relationship or negotiation notes belong in `AGENTS.local.md`, `.local/`
  or the personal repo `ger-hypha-energy`.
- Read the `AGENTS.md` of the specific view for its conventions.

## Personal context

If `AGENTS.local.md` exists in this directory, read it (gitignored).
