# Personal Config — `AGENTS.local.md` Schema + Init Procedure

> The reusable, tool-agnostic mechanism for capturing **how a particular developer wants the AI to
> work with them**. It produces gitignored `AGENTS.local.md` files at two scopes (see below). Any AI
> tool can run the procedure here; the Claude Code `/henergy-dev-setup` skill is just a wrapper.

---

## When to run this

- A developer has **no `AGENTS.local.md`** (at the repo root and/or their member scope), or
- a developer asks to "set up my context / preferences", or
- preferences have changed and a file needs updating (these are **living documents** — re-run any
  time; you can update one scope without touching the other).

The root `AGENTS.md` instructs every AI to check for these files and offer to run setup when missing.

---

## Two scopes — general (root) + developer (member)

Personal context is split across the `AGENTS.md` **cascade**, so each file does one job:

| Scope | File | Holds |
|-------|------|-------|
| **General** | `/AGENTS.local.md` (repo root) | Who I am, how I use/read/write across the **whole repo** (solution / progress / planning / patterns / workflow), comms, tooling, cross-cutting boundaries. The broad personal frame, read first. |
| **Developer** | `progress/members/<username>/AGENTS.local.md` | How I work **on development tickets** — the SDD settings (review mode, decision posture, profile, levels), current tracks, dev-env quirks. Refines the general frame for dev work. |

Templates: [`AGENTS.local.general.template.md`](AGENTS.local.general.template.md) and
[`AGENTS.local.developer.template.md`](AGENTS.local.developer.template.md). Both carry a
**YAML frontmatter** block (machine-readable settings any AI parses deterministically) plus prose.
Globals (identity, comms, tooling) live in the **general** file and are *not* duplicated into the
developer file.

### General config — `/AGENTS.local.md`

Frontmatter: `developer`, `display_name`, `role`, `comms_languages`, `verbosity`,
`ai_tools`, `primary_areas` (the repo areas the developer works in most), `context_sync`
(`auto` | `remind` — how to keep the local clone fresh; default `auto` for non-technical profiles,
`remind` for technical), and — **for technical developers only** — an optional `github` block
(opt-in GitHub integration; see below).
Prose: **Who I am** · **How I use this repo** · **How I read / write / work with the info** ·
**Communication & working preferences** · **GitHub integration** (if enabled) · **Cross-cutting
boundaries**.

**`github` block (optional, technical opt-in)** — drives [`github-sync.md`](github-sync.md):

| Key | Meaning |
|-----|---------|
| `github.enabled` | `true` to let the AI read/update GitHub via the `gh` CLI (skills-over-MCP); absent/`false` = the feature is off and never offered |
| `github.cli` | the CLI used (`gh`) |
| `github.project` | the board this developer works against (team default: `Hypha-Energy/projects/13` — "hypha.energy website development") |
| `github.watch` | what this developer tracks — *personal* (e.g. "issues and PRs assigned to me", or a label/area) |

Only meaningful for the technical profiles (`disciplined_active`, `technical_low_discipline`); leave
the block out entirely for `conceptual_platform` / `strategic_consumer`.

### Developer config — `progress/members/<username>/AGENTS.local.md`

| Key | Allowed values | Meaning · grounding |
|-----|----------------|---------------------|
| `engagement_profile` | `disciplined_active` · `technical_low_discipline` · `conceptual_platform` · `strategic_consumer` | which of the four contributor profiles (root `AGENTS.md`); calibrates how much the AI scaffolds vs. expects the developer to maintain |
| `spec_review_mode` | `guided` · `independent_with_callouts` · `ai_suggested` | how spec review runs (Direction E/F): walk section by section · present 3–5 targeted callouts then wait · AI proposes a mode per task and says why |
| `review_callouts` | integer or range (default `3-5`) | how many targeted callouts after a spec draft (Direction E) |
| `decision_posture` | `human_decides` (default) · `ai_lean_when_asked` | Direction G. `human_decides` = AI surfaces findings + maps options + flags what matters, developer decides; a pre-filled recommendation is a red flag. `ai_lean_when_asked` = AI may give a clearly-flagged lean *when explicitly asked* |
| `discovery_default` | `explicit` · `inline` | Direction D. `explicit` = name a Discovery phase and use an `inputs/` subfolder when research precedes the spec |
| `default_sdd_level` | `0`–`4` · `by_trigger` (recommended) | starting file-structure level; `by_trigger` = escalate via the table in [`sdd-method.md`](sdd-method.md) |
| `iteration_cap` | integer (default `3`) | max consecutive AI-only iterations before a human review reset (SDD Rule 3) |

Prose: **How I want you to work (dev)** · **Current engagement context** · **Dev environment &
tooling quirks**.

---

## The init procedure (what the AI does)

1. **Frame.** Read the root [`AGENTS.md`](../AGENTS.md) (the four engagement profiles, the Coherence
   Loop mapping) and both templates.
2. **Interview.** Ask a short, structured set of questions — don't dump the whole schema. Cover both
   scopes (≈7–8 questions, adapt to answers):
   - *General:* name + username + role (assume vs. explain?); how you use this repo and what you're
     usually looking for; reading/writing preferences across the areas; comms (tone, languages,
     verbosity); cross-cutting boundaries.
   - *Developer:* engagement profile; spec review mode; decision posture; discovery default / SDD
     level; current focus + dev-env quirks.
   - *GitHub (ask only if the developer is **technical**):* "Want the AI to read/update GitHub issues,
     PRs, and the project board on your behalf via the `gh` CLI?" If yes — confirm `gh` is installed
     and authenticated (`gh --version`, `gh auth status`; the board needs `gh auth refresh -s
     project`); if it's missing, **offer the install command for them to run — don't auto-install**.
     Then capture which board (default `Hypha-Energy/projects/13` — "hypha.energy website development";
     web view: `https://github.com/orgs/Hypha-Energy/projects/13/views/1`) and what they want watched
     (e.g. issues/PRs assigned to them). Share the board URL so they can verify they're looking at
     the right one. If no/non-technical, omit the `github` block entirely. See
     [`github-sync.md`](github-sync.md).
   When *updating*, only ask about the scope(s) that changed.
3. **Generate both files.** Fill each template from the answers. Default unstated settings to the
   recommended values (`decision_posture: human_decides`, `default_sdd_level: by_trigger`,
   `iteration_cap: 3`, `verbosity: concise`, and `context_sync` by profile — `auto` for
   non-technical, `remind` for technical). Keep globals (identity/comms/tooling) in the general
   file only.
4. **Place them.**
   - **General** → `/AGENTS.local.md` (repo root).
   - **Developer** → `progress/members/<username>/AGENTS.local.md`.
   Confirm both are gitignored (`*.local.md`) and **will not be committed**. (A developer may also
   add an `AGENTS.local.md` inside a single ticket folder for task-scoped notes — same convention.)
5. **Confirm + iterate.** Show the files, invite corrections, and note they're living documents —
   update either scope whenever things shift, or re-run this procedure.

---

## How the configs are used afterwards

Every AI session reads the cascading `AGENTS.md` files, then the developer's `AGENTS.local.md`
files **in cascade order — general (root) first, then the scope-specific one** — and adopts their
settings: it states the review mode it's using and why (never silently), honors the decision posture
at the Commitment Gate, scaffolds to the right SDD level, matches tone/language, and follows the
developer's stated way of reading/writing across the repo. [`task-init.md`](task-init.md) consumes
the same configs when starting a ticket.

> Distinct from `.local/` **directories** (heavy/private *ticket* material under
> `progress/members/<username>/tickets/<id>/.local/`). `AGENTS.local.md` is personal context *about
> the developer*; `.local/` is bulk material *about a ticket*. Both are gitignored, different jobs.
