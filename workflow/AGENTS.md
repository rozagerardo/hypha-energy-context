# AGENTS.md — `workflow/`

> Read the root [`AGENTS.md`](../AGENTS.md) first for the global frame.

## What this directory is

The **reusable, tool-agnostic mechanisms that define *how* work happens** in this context — the
workflow layer. Where `solution/` is *what the platform is*, `progress/` is *what's been done*, and
`planning/` is *what's next*, `workflow/` is ***how* we work**: the SDD method, how each developer
personalizes it, and how a task gets initialized.

Everything here is written to be executed by **any AI tool** (Claude Code, Cursor, Copilot, …). The
logic lives in portable Markdown; vendor-specific wrappers (the Claude Code skills under `.claude/`)
are thin shims that just point back to these files.

## What's here

| File | Purpose |
|------|---------|
| [`get-started.md`](get-started.md) | The **front door** — the first-contact router for *anyone* (technical or not): greet, orient in a sentence, ask 1–2 questions, and dispatch to the right path below (or a guided, no-setup tour for non-technical folks). |
| [`sdd-method.md`](sdd-method.md) | The Spec-Driven Development method: levels, the per-ticket Phase A→D workflow, the proven (research-backed) rules, and the per-ticket checklist. The *what* the workflow enforces. |
| [`personal-config.md`](personal-config.md) | The **personal-setup mechanism** — the `AGENTS.local.md` schema (two scopes) + an AI-agnostic interview procedure to generate a developer's personal context. Run once per developer (re-run to update). |
| [`AGENTS.local.general.template.md`](AGENTS.local.general.template.md) | Template for the **general** personal config at the repo root (`/AGENTS.local.md`) — how a developer uses the whole repo. Committed; generated instances are gitignored (`*.local.md`). |
| [`AGENTS.local.developer.template.md`](AGENTS.local.developer.template.md) | Template for the **developer** config at a member scope (`progress/members/<username>/AGENTS.local.md`) — SDD working settings for tickets. Committed; instances gitignored. |
| [`task-init.md`](task-init.md) | The **per-task scaffolding flow** — reads the developer's config, classifies a ticket, picks the SDD level, and scaffolds its folder. Run when **starting** a unit of work (first session only). |
| [`task-resume.md`](task-resume.md) | The **session opener for in-progress tickets** — loads the existing ticket, assesses the current SDD phase, surfaces open threads, and briefs what's next. Run at the start of every subsequent session. |
| [`checkpoint.md`](checkpoint.md) | The **session / milestone wrap-up flow** — diffs against the last checkpoint and folds the work back into the shared artifacts (spec header, decisions, activity log, platform area, pattern candidates). Run to **record progress** (end of session/day, after a PR/milestone, before a compaction). |
| [`github-sync.md`](github-sync.md) | The **GitHub integration** — reading/updating issues, PRs, and the project board via the `gh` CLI (skills-over-MCP). **Gated** to technical opted-in developers; writes always confirm first. Runs ad-hoc *and* as a soft step inside `task-init` / `checkpoint`. |
| [`git-conventions.md`](git-conventions.md) | **Git branch naming** shared by every repo using this context — team `<type>/<description>` vs. personal `<username>/<type>/GH-<issue#>-<description>`, allowed types. Read before creating a branch. |
| [`sync-context.md`](sync-context.md) | **Keep the local context current** (`git pull`) so the AI reads fresh info, not a stale snapshot. A **cross-cutting step**: auto-refresh at session start for non-technical users, a reminder/offer for technical ones. Runs ad-hoc *and* inside `get-started` / `task-init` / `task-resume` / `checkpoint`. Distinct from `github-sync` (board, not files). |
| (skills moved to [`.agents/skills/`](../.agents/skills/)) | **Tool-agnostic skill definitions** live in `.agents/skills/<name>/SKILL.md`. Vendor wrappers (`.claude/skills/`, `.cursor/rules/`) are thin pointers to those files. |

## The entry points

**Front door — [`get-started.md`](get-started.md) (`/henergy-start`).** The friendly first-contact
router for *anyone*, technical or not: greets, orients in a sentence, asks 1–2 questions, and
dispatches to the right path below — or, for non-technical folks, a guided tour with **no setup**.
Start here when there's no specific goal yet.

The working entry points it dispatches to:

1. **Personal setup (once per developer).** A new contributor — or their AI — runs
   [`personal-config.md`](personal-config.md) to produce their two `AGENTS.local.md` files: a
   **general** one at the repo root (how they use the whole repo) and a **developer** one at their
   member scope (SDD working settings). After that, any AI session reads them in cascade order and
   knows exactly how to work with them (how they read/write here, review mode, decision posture,
   tooling, language, engagement profile). *Claude Code:* `/henergy-dev-setup`.
2. **Task init (start of a unit of work).** When starting a ticket for the first time, the AI runs
   [`task-init.md`](task-init.md): it reads the personal config, classifies the work, recommends the
   SDD level, and scaffolds the ticket folder under `progress/members/<username>/tickets/`.
   *Claude Code:* `/henergy-task-init`.
3. **Task resume (subsequent sessions).** At the start of any session after the first, the AI runs
   [`task-resume.md`](task-resume.md): it loads the existing ticket, assesses the current SDD phase,
   surfaces open threads, and briefs the session on what's next. *Claude Code:* `/henergy-task-resume`.
4. **Checkpoint (recording progress).** At the end of a session/day, after a milestone or PR, at a
   phase transition, or before a context compaction, the AI runs [`checkpoint.md`](checkpoint.md): it
   diffs against the last checkpoint and updates the activity log, ticket artifacts, platform area,
   and pattern candidates. *Claude Code:* `/henergy-checkpoint`.

Task init (first session) and checkpoint (session close) are the **session bookends**; task resume
opens every subsequent session. A long ticket: one task-init → many (task-resume → work →
checkpoint) cycles.

The root `AGENTS.md` carries the **tool-agnostic trigger phrases** that make these self-triggering.
The canonical skill definitions live in [`skills/`](skills/) — Claude Code and Cursor wrappers are
thin pointers to those files (see "Standard mechanisms" below).

## Standard mechanisms it leans on (nothing custom)

- **`AGENTS.md` cascade** — the cross-vendor context standard already used across this repo;
  `AGENTS.local.md` is its gitignored personal variant.
- **YAML frontmatter** — a vendor-neutral, machine-parseable settings block, so any AI reads a
  developer's preferences deterministically (no guessing from prose).
- **`*.local.md` gitignore** — instances never persist; only the template and procedures do.
- **Vendor wrappers** — thin shims for the team's tools that point to the canonical skill
  definitions in [`.agents/skills/`](../.agents/skills/). **Claude Code:** `.claude/skills/henergy-*/SKILL.md` (one per
  skill, each a 2-line pointer to the matching `.agents/skills/` file). **Cursor:**
  `.cursor/skills/henergy-*/SKILL.md` (slash-command wrappers with `disable-model-invocation: true`,
  same pointer pattern) plus `.cursor/rules/henergy-workflow.mdc` (natural-language trigger routing).
  Trigger phrases in the root `AGENTS.md` are the portable fallback for any other AI tool.

## Relationship to the rest of the repo

- This workflow is inherited from the Hypha DAO shared context (`hypha-context`, issue #2299 there); the
  Pattern Card lives there too.
- It produces artifacts into `progress/` (ticket folders) and maps onto the V3 Coherence Loop exactly
  as the root `AGENTS.md` describes (Phase A = Coherence, sign-off = Commitment Gate, Phase B =
  Action, Phase C = loop return).

## Personal context

If `AGENTS.local.md` exists in this directory, read it — gitignored, not committed.
