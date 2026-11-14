# AGENTS.md — Hypha Energy Context

> **You are reading the root AI context file for `hypha-energy-context`.**
> This is the sole, tool-agnostic AI entry point. Any AI assistant (Claude Code, Cursor, Copilot,
> etc.) working in this repo, or working on Hypha Energy code with this repo as context, should read
> this file first, then the `AGENTS.md` of whatever subdirectory the task touches.

---

## What this repo is

`hypha-energy-context` is the **context layer for work on Hypha Energy**: an AI-legible, structured
store of the shared understanding of the organisation and its platform, per-member progress,
planning, and candidate patterns distilled from real work. It is the Hypha Energy counterpart of
`hypha-context` (the Hypha DAO team context repo) and deliberately mirrors its structure and workflow.

It is **not** a documentation site, a code repo, or a personal notebook. It is the *intelligence layer
around the work*: shared memory that AI tools read at the start of a task and update as part of doing
it, so no session rebuilds context from scratch.

**Status: personal for now.** This repo currently belongs to Ger Roza (`rozagerardo/hypha-energy-context`),
not to the Hypha-Energy GitHub org. It is written in a neutral, team-facing voice so it can move to the
org later if the team wants it. Personal material goes in `*.local.md` files, in `.local/`, or in the
sibling personal repo `ger-hypha-energy`.

### How it relates to the sibling repos

| Repo | Role |
|---|---|
| `hypha-energy-context` (this) | Technical/organisational context, progress, planning, workflow for Hypha Energy |
| `ger-hypha-energy` | Ger's **personal** involvement: agreements, call prep, comms drafts, personal notes |
| `hypha-context` | Same idea for **Hypha DAO** (shared with that team). Source of the workflow used here |
| `ger-hypha` | Ger's personal Hypha DAO repo |
| Org repos (`Hypha-Energy/*`) | The code. Read-only clones in `../` (`hypha-energy`, `Hypha.Energy`, `value-x.com`) |

Do not mix Hypha DAO material into this repo or Hypha Energy material into the DAO repos. The only
bridge is the white-label iframe embed (Hypha DAO's platform, shown to Hypha Energy partners).

> **Confidentiality:** the Hypha-Energy org repos are proprietary / internal use only. Summarise and
> point; do not paste their source or data into public places (issues on other orgs, Discord, etc.).

> **On timeframe:** this context starts on **2026-11-13**. Earlier events are referenced only where
> known from other sources.

---

## Connection to the Coherence Loop

Hypha Energy runs its governance on the Hypha platform and so on the same **V3 Coherence Model**:
**Coherence** (shared understanding) and **Action** (execution) bound by **Commitments**. The workflow
maps onto it the same way as in `hypha-context`:

| Phase | Coherence Loop phase | Artifact |
|-------|---------------------|----------|
| Research, spec writing (Phase A) | **Coherence** | `spec.md`, `analysis.md` |
| Spec sign-off | **Commitment Gate** (requires human judgment; an AI cannot cross it) | frozen `spec.md` |
| Implementation (Phase B) | **Action** | the code, in its own repo |
| Review, learnings (Phase C+) | **Loop return** | `decisions.md`, pattern candidates |

For the engagement itself, the Commitment Gate is also the **agreement** (proposal accepted in the
Hypha Energy Space); those live in `ger-hypha-energy/agreements/`.

---

## Contributor engagement profiles

Six profiles (A to F) calibrate how much the workflow asks of a person: see
[`workflow/contributor-profiles.md`](workflow/contributor-profiles.md). Today the only active feeder
is Ger (profile A); keep the design usable by the others.

---

## Folder structure

```
hypha-energy-context/
├── AGENTS.md            ← you are here: root AI entry point (tool-agnostic)
├── CLAUDE.md            ← minimal pointer for Claude Code → defers to this file
├── README.md            ← human landing card
├── solution/            ← shared understanding of Hypha Energy, by dimension
│   ├── overview.md          what Hypha Energy is: start here
│   ├── tech/                architecture, repo map (codebases), deploy facts
│   └── product/             the energy-community model (settlement, pricing, tokens)
├── progress/            ← what's getting done
│   └── members/             per-member SDD workspace: activity-log + tickets/<id>/ folders
├── planning/            ← near-term priorities, board conventions
├── patterns/            ← candidate pattern cards
└── workflow/            ← how we work: SDD method + tool-agnostic procedures
```

- **`solution/`**: working knowledge of *what Hypha Energy is and how it works*. Start at
  [`solution/overview.md`](solution/overview.md).
- **`progress/`**: `members/<username>/` is each person's SDD workspace: an `activity-log.md` plus a
  `tickets/<slug>/` folder per substantial work item (`spec.md`, `decisions.md`, `analysis.md`).
  A `platform/` view (progress by area: website, platform, ValueX, workspace stack) is added when
  there is real progress to record there.
- **`planning/`**: the "what's active now" view. Source of truth for tickets is the GitHub board
  (org project #13) and the agreement blocks.
- **`patterns/`**: reusable lessons, staged here. The SDD workflow pattern card lives in `hypha-context`.
- **`workflow/`**: the SDD method and procedures. Canonical skill definitions in `.agents/skills/`;
  thin wrappers for Claude Code (`.claude/skills/`) and Cursor (`.cursor/skills/`).

---

## How to navigate

1. **Start here** for the global frame.
2. **Read the `AGENTS.md` of the subdirectory your task touches.**
3. Org/platform/architecture → `solution/`. Repo map → `solution/tech/codebases.md`.
4. "What has X been working on" → `progress/members/<username>/`.
5. "What's active" → `planning/`.

---

## Working here — the workflow entry points

Tool-agnostic procedures live in [`workflow/`](workflow/AGENTS.md) and are **self-triggering**. The
trigger phrases are the portable contract; tools with skills auto-detect them (Claude Code from each
skill's `description`; Cursor from `.cursor/skills/henergy-*/` and `.cursor/rules/henergy-workflow.mdc`).
Skills use the **`henergy-`** prefix (the Hypha DAO equivalents use `hypha-`, so both can be loaded
side by side).

- **Get started**: "hi", "let's get started", "what is this". [`workflow/get-started.md`](workflow/get-started.md). `/henergy-start`.
- **Personal setup**: "set up my context/preferences", or no `AGENTS.local.md` at the developer's scope.
  Check two scopes (general at repo root, developer at `progress/members/<username>/`), read in cascade
  order, adopt their settings, and *state the review mode and decision posture, never silently*. If
  missing, offer [`workflow/personal-config.md`](workflow/personal-config.md). `/henergy-dev-setup`.
- **Task init**: "I'm starting X", "new task". [`workflow/task-init.md`](workflow/task-init.md). `/henergy-task-init`.
- **Task resume**: "continuing X", "picking up where I left off". [`workflow/task-resume.md`](workflow/task-resume.md). `/henergy-task-resume`.
- **Checkpoint**: "let's checkpoint", "wrap up for the day", "record where we are".
  [`workflow/checkpoint.md`](workflow/checkpoint.md). `/henergy-checkpoint`.
- **GitHub board sync (technical, gated)**: "update the board", "move #X", "what's assigned to me".
  Via `gh` against the Hypha-Energy org. Only when `github.enabled: true`. **Reads are free; every write
  asks first.** [`workflow/github-sync.md`](workflow/github-sync.md). `/henergy-board`.
- **Update context**: "update my context", "pull latest", and automatically at session start.
  [`workflow/sync-context.md`](workflow/sync-context.md). `/henergy-update-context`.

Not carried over from `hypha-context` (add when needed): CodeRabbit triage, brief generator, feedback
intake, team-cadence input.

The method itself is [`workflow/sdd-method.md`](workflow/sdd-method.md).

## Cross-repo work: start here, change code there

This repo holds the *why / what / progress / how-we-work*; the **code lives in the org repos**, cloned
read-only-by-default as siblings of this repo, under `C:\Users\rozag\projects\hypha-energy\src\`. Each carries its own context
(`value-x.com/CLAUDE.md`, `Hypha.Energy/.github/copilot-instructions.md`, the website's
`Guidelines.md`). The workflow composes the two: ground the task here, bring the code repo into scope
(`/add-dir`, multi-root workspace, or launch from the parent folder), make code changes **there**
following **its** rules and PR flow, and record back here (`checkpoint`). Never push to an org repo
without Ger's explicit go-ahead.

### Satellite: working from a code repo alone

Profile A/B developers get a **satellite**, a local `.henergy-context.local/` directory in the code repo
(today `value-x.com`), plus committed `henergy-*` skills there (`task-init`, `task-resume`, `checkpoint`,
`board`). It projects personal config, workflow profile, absolute pointers and the active spec, so an AI
opened on the code repo alone has the context, and writes progress back here. It is local, never
committed, and **separate from the Hypha DAO satellite** (`.hypha-context.local/`, `hypha-*` skills):
two bounded contexts, no shared tree. Setup: `/henergy-dev-setup`. Refresh: `/henergy-update-context`.
Phase B kickoff: `/henergy-task-resume`. Close: `/henergy-checkpoint`.
Full procedure: [`workflow/satellite.md`](workflow/satellite.md).

## Personal context files (`AGENTS.local.md`)

`AGENTS.local.md` files are **gitignored** personal context. **When working in any directory, check
whether `AGENTS.local.md` exists there and read it.** No tool auto-loads them; this instruction does.
A **`.local/` directory** (gitignored) can sit at the root or inside any ticket folder for drafts
(date-prefixed `YYYYMMDD-<slug>.md`) and heavy/private material that does not belong in the shared docs.

---

## Conventions

- **Voice:** committed files are written for *any Hypha Energy contributor*, not one person.
- **Naming:** outputs at directory root, inputs in `inputs/` subfolders. Standard artifacts: `spec.md`,
  `brief.md`, `decisions.md`, `implementation-plan.md`, `analysis.md`.
- **Terminology:** Energy Community, Orchestrator, Initiator, eParts, Energy Credits. The website brand
  book (`Guidelines.md` in the `hypha-energy` repo) is canonical for voice. No overpromising.
- **Founders decide** scope, tier, hours and compensation. Never invent them; list as open points.
- **Living documents; worked data, not raw data.**
