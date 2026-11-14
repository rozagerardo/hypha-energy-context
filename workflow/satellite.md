# Satellite

> The procedure for creating, refreshing and maintaining the **`.henergy-context.local/`** directory
> injected into Hypha Energy code repos (`value-x.com`, later `hypha-energy`, `Hypha.Energy`). It gives
> an AI tool working from a code repo enough workflow context to behave as if `hypha-energy-context`
> were in scope, without opening both repos. Same mechanism as the `.hypha-context.local/` satellite of
> the Hypha DAO side, **kept deliberately separate**: Hypha (platform / DAO) and Hypha Energy are two
> bounded contexts with their own repos, lifecycles and workflow. They know about each other; they do not
> share a satellite, a skill namespace (`hypha-*` vs `henergy-*`) or a tree. A code repo of one side never
> carries the other side's satellite.

**Applies to:** contributor profiles A (engaged dev) and B (vibe-coder) only. See
[`contributor-profiles.md`](contributor-profiles.md).

---

## What the satellite is (and is not)

A **read-mostly projection** of the relevant slice of `hypha-energy-context` into the working code repo,
plus a **write-back channel** (`pointers.md`).

- **Tool-agnostic:** plain markdown, readable by Claude Code, Cursor or any tool that reads `AGENTS.md`.
- **Local, never committed:** it captures per-developer, per-machine context. In org repos we do not own
  the `.gitignore` of, exclude it through `.git/info/exclude` (local to the clone) instead of editing the
  committed `.gitignore`, unless the repo owner asks for the pattern there.
- **Not a replacement for `hypha-energy-context`:** the source of truth stays there.

## Layout

```
<code-repo>/
├── AGENTS.md                      ← committed: short note pointing at the satellite + the sibling context repo
├── .agents/skills/henergy-*/      ← committed: task-init, task-resume, checkpoint, board (satellite versions)
├── .claude/skills/henergy-*/      ← committed: 2-line wrappers to .agents/skills
└── .henergy-context.local/        ← LOCAL: never committed
    ├── AGENTS.md                  ← AI entry point: what this is, what to read
    ├── personal.md                ← WHO: developer profile + working preferences (snapshot)
    ├── workflow.md                ← HOW: SDD phases, checkpoint triggers, write-back scope
    ├── pointers.md                ← WHERE: absolute paths into hypha-energy-context + active ticket
    ├── spec-active.local.md       ← WHAT (active): copy of the FROZEN spec of the current Phase B ticket
    ├── specs-parked.local.md      ← WHAT (parked): specs not formally closed; 30-day cleanup
    └── specs-done-cache.local.md  ← WHAT (done): closed specs; 30-day cache
```

`AGENTS.md`, `personal.md`, `workflow.md`, `pointers.md` are created at setup. The three `specs-*` files
are created on first use.

**Differences from the Hypha DAO satellite** (this repo is smaller and single-feeder today): no sibling
clones, no `feedback/` intake, no `briefs/` or CodeRabbit/brief/cadence skills, and `platform_progress`
is optional (the `progress/platform/` view only exists once there is real progress to record).

## File templates

### `.henergy-context.local/AGENTS.md`

```markdown
# .henergy-context.local — Hypha Energy Satellite

> You are working in a Hypha Energy code repo. This directory was injected by `hypha-energy-context`
> at setup time. Read this file, then the others here, before assisting with any development task.
>
> **This directory is local and never committed.** Do not commit it or mention it in code comments or
> commit messages.

## What this is

A satellite projection of the `hypha-energy-context` repo, scoped to this developer and machine. It gives
you the workflow context and pointers to work here as if you had that repo in scope.

## What to read

| File | Read when |
|---|---|
| `personal.md` | Always: developer profile and preferences |
| `workflow.md` | Always: how the workflow runs for this developer |
| `pointers.md` | To read or write back to hypha-energy-context |
| `spec-active.local.md` | When a Phase B ticket is active: the frozen spec |
| `specs-parked.local.md` | When checking earlier in-progress work |
| `specs-done-cache.local.md` | When polishing after a close, or on 30-day cleanup |

## Key behavioral rules

1. **Spec before code.** No implementation without a spec. If `spec-active.local.md` is absent, run
   `/henergy-task-resume` (or `/henergy-task-init`). If the spec is not frozen yet (Phase A draft), work
   from the live spec at `pointers.md` → `active_spec_path` and say so.
2. **Write-back scope.** Write only to `member_progress` (and `platform_progress` if present) from
   `pointers.md`. Never to another member's path; never to the context repo root without explicit instruction.
3. **Phase B means code time.** A frozen spec is not reopened; direction changes go to `decisions.md`.
4. **Checkpoint = deactivation.** On checkpoint, move `spec-active.local.md` to `specs-done-cache.local.md`
   (closed) or `specs-parked.local.md` (session wrap) and update `pointers.md`.
5. **Never push to the org remote** without the developer's explicit go-ahead.

## Skills available from this repo

`henergy-task-init`, `henergy-task-resume`, `henergy-checkpoint`, `henergy-board`, committed under
`.agents/skills/` and `.claude/skills/`. They resolve context locations from `pointers.md`, never from a
hardcoded path or member folder.
```

### `personal.md`

Snapshot of the developer's cascaded `AGENTS.local.md` (general → member): `developer`, `display_name`,
`role`, contributor profile, languages, verbosity, engagement profile, spec review mode (+ callouts),
decision posture, discovery default, SDD level default, dev environment notes. Header: "Snapshot,
refreshed by `/henergy-update-context`; do not edit here."

### `workflow.md`

Profile-specific: one paragraph on what the profile means here; the phase table below; checkpoint
triggers; write-back behavior; repo-specific verification notes (e.g. build/run commands from the code
repo's own `AGENTS.md`).

| Phase | Where | Role |
|---|---|---|
| **Phase A: Spec** | `hypha-energy-context` | Drive via `/henergy-task-init`; freeze before code |
| **Commitment Gate** | `hypha-energy-context` | Human sign-off; the AI cannot cross it |
| **Phase B: Code** | the code repo | Implement against the spec; decisions to `decisions_path` |
| **Phase C: Loop return** | `hypha-energy-context` | `/henergy-checkpoint`; fold decisions and learnings back |

Profile B only: soft-trigger auto-capture to `member_progress`, same rules as the Hypha DAO satellite
(scope = `member_progress` only; anything else asks).

### `pointers.md`

```markdown
# Pointers — {display_name}

> Absolute paths into hypha-energy-context on this machine. Machine-specific. Refreshed by
> `/henergy-update-context`. Do not edit manually.

henergy_context_root: {/absolute/path/to/hypha-energy-context}
member_progress: {.../progress/members/{developer}}
platform_progress: {.../progress/platform}        # optional; omit until it exists

## Active ticket
active_ticket_slug: {slug or "none"}
active_spec_path: {.../tickets/{slug}/spec.md}
active_spec_frozen: {true | false}
decisions_path: {.../tickets/{slug}/decisions.md}
activity_log_path: {.../activity-log.md}
```

### `spec-active.local.md`, `specs-parked.local.md`, `specs-done-cache.local.md`

Identical in shape to the Hypha DAO satellite: `spec-active` is a verbatim copy of the frozen `spec.md`
under a header (`source`, `ticket`, `copied_at`, `status: active`, read-only note); `specs-parked` and
`specs-done-cache` hold dated blocks (`Parked at` / `Closed at`, `Source`, `context-wrapup-done`,
`Cleanup eligible after` = +30 days, a 2-3 sentence summary). Ticket ids here are slugs (e.g.
`b1-readonly-demo`), not GitHub numbers, unless the ticket has a board issue.

---

## Procedures

### 1. Generate (at `henergy-dev-setup` time, or when adding a code repo)

1. **Resolve the code repo path.** Code repos are siblings of `hypha-energy-context` under
   `C:\Users\rozag\projects\hypha-energy\src\` (`value-x.com`, `hypha-energy`, `Hypha.Energy`). Confirm
   the path exists; offer to clone from `Hypha-Energy/<repo>` if absent.
2. **Create `.henergy-context.local/`** with `AGENTS.md`, `personal.md`, `workflow.md`, `pointers.md`.
3. **Keep it out of git.** Check the repo's `.gitignore` and `.git/info/exclude`; if not covered, add
   `.henergy-context.local/` to `.git/info/exclude` (local, no repo change). Offer a `.gitignore` edit only
   if the owner wants it shared.
4. **Install the skills** (once per code repo): copy `henergy-task-init`, `henergy-task-resume`,
   `henergy-checkpoint`, `henergy-board` into `.agents/skills/` plus the 2-line wrappers in
   `.claude/skills/`. They are committed content: keep them person- and machine-agnostic. Leave committing
   to the developer.
5. **Add the note to the code repo's `AGENTS.md`** (below).
6. **Confirm** by listing the files created.

### 2. Refresh (inside `henergy-update-context`, profile A/B)

Re-derive `personal.md`, `workflow.md` and the active-ticket block of `pointers.md` from the current
config. Do not touch `AGENTS.md` unless the template changed, and do not overwrite `spec-active.local.md`.
Run the 30-day cleanup on the two specs files (delete entries past their date with
`context-wrapup-done: true`; flag the others).

### 3. Phase B kickoff (inside `henergy-task-resume`)

If the ticket is frozen and a satellite exists: copy `spec.md` to `spec-active.local.md` (park a
different active ticket first; refresh a stale copy of the same one), update `pointers.md`
(`active_spec_frozen: true`), tell the developer. If the spec is **not frozen** (Phase A draft, work
already started in code), do not copy: set `active_spec_path` to the live spec and
`active_spec_frozen: false`.

### 4. Checkpoint (inside `henergy-checkpoint`)

Ticket complete: move `spec-active.local.md` to `specs-done-cache.local.md` (`context-wrapup-done: true`),
clear it, clear the active ticket in `pointers.md`. Session wrap, ticket ongoing: park it to
`specs-parked.local.md`. Then run the 30-day cleanup.

---

## Note for code repos

Committed in each code repo's `AGENTS.md`:

> *A `.henergy-context.local/` directory may exist at the root. If present, read
> `.henergy-context.local/AGENTS.md` first: it holds workflow context and pointers for AI-assisted work
> aligned with the Hypha Energy SDD process. It is local to each developer's machine and not committed.*

**Rule for satellite skills:** they are committed and shared, so they resolve `henergy_context_root`,
`member_progress` and `platform_progress` from `.henergy-context.local/pointers.md` (fallback: the
`../hypha-energy-context` sibling, and `developer` from its `AGENTS.local.md`). Never hardcode a member's
username or an absolute path.
