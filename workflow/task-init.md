# Task Init — Per-Ticket Scaffolding Flow

> The procedure an AI runs when a developer **starts a unit of work**. It reads their personal
> config, classifies the task, picks the right SDD level, and scaffolds the ticket folder — so the
> SDD workflow is applied consistently without the developer needing to remember the structure.
> Tool-agnostic; the Claude Code `/henergy-task-init` skill wraps it. Grounds in
> [`sdd-method.md`](sdd-method.md). Its wrap-up bookend is [`checkpoint.md`](checkpoint.md)
> (`/henergy-checkpoint`).

---

## When to run this

A developer says some variant of *"I'm starting work on #2296"* / *"let's begin the off-ramp
ticket"* / *"new task: …"*.

## The procedure

### 1. Load context
- **Refresh first:** make sure the local context is current — run [`sync-context.md`](sync-context.md)
  (`/henergy-update-context`); for a technical dev with work in progress, just remind/offer, don't pull over it.
- Read the cascading `AGENTS.md` files and the developer's
  [`AGENTS.local.md`](personal-config.md) (review mode, decision posture, discovery default,
  default SDD level, verbosity, language).
- Read the GitHub issue (and any linked ones). If it's not a ticket yet, note that. *If the developer
  is GitHub-gated-in (`github.enabled` + technical profile), use the `gh` CLI to read the issue/board
  item directly — see [`github-sync.md`](github-sync.md).*

### 2. Classify the task (2–4 questions, skip any the config/issue already answers)
- **Type:** analysis · feature · bug · process.
- **Reviewer:** will another contributor review the spec or implementation before merge?
- **Discovery:** is research needed before a spec can be written, or is the area already understood?
- **Phasing:** one implementation pass, or multiple distinct phases?

### 3. Pick the SDD level — escalation trigger table
Start from `default_sdd_level` (usually `by_trigger`) and add files only as a condition fires:

| Add this artifact | When this condition is true |
|---|---|
| `spec.md` | **always** — the frozen goal + contracts + flows + acceptance + boundaries |
| `analysis.md` | Discovery happened — living findings, options, open questions (provisional) |
| `inputs/` subfolder | Discovery research material exists (external sources, prior art, conversation notes) |
| `brief.md` | an analysis ticket producing a team-facing recommendation |
| `implementation-plan.md` | Phase B uses AI-assisted implementation (i.e. ~always for code tickets) |
| `decisions.md` | more than 2 non-obvious decisions get made |
| `acceptance.md` | criteria will be automated as tests, or a separate person owns sign-off |
| `plan-phase-N.md` | the ticket has multiple distinct implementation phases with separate AI briefs |
| `.local/` | heavy/private working material exists (raw sources, scratch, copies) — gitignored |

Outputs live at the ticket root; inputs in `inputs/` (Direction H).

### 4. Scaffold the folder
Create `progress/members/<username>/tickets/<issue#>-<slug>/` and seed:
- `spec.md` from the spec shape in [`sdd-method.md`](sdd-method.md) (header: Issue · Status · SDD
  phase · Track · Last touched · Platform area).
- the additional artifacts the triggers selected (empty-but-structured, ready to fill).
- `inputs/` and/or `.local/` if discovery/heavy material applies.
- add a dated entry to `progress/members/<username>/activity-log.md` and link the relevant
  `progress/platform/` area.
- **GitHub (soft step, if gated-in):** read the linked issue (free). Then:
  - If the work has **no issue** but belongs on the board, *offer* to create one (correct
    `type(scope):` title + labels) — per [`github-sync.md`](github-sync.md) and
    [`../planning/issue-guidelines.md`](../planning/issue-guidelines.md).
  - **Whether the issue was just created or already existed, add it to project #13** if it isn't
    there yet (`gh project item-add 13 --owner Hypha-Energy --url <issue-url>`). This is the step
    most likely to be missed — make it explicit. Default status: Backlog.
  - Optionally set Priority / Target date fields if known.
  - Reads are free; **any write asks first**. Skip silently if not gated-in.

### 5. State the mode, then work the loop
- **Say which review mode and decision posture you're using and why** (from the config — never
  silently). Example: *"Using independent-with-callouts and human-decides per your config; this spec
  is short so that fits."*
- Work the SDD lifecycle (maps onto the Coherence Loop):
  - **Discovery** (if needed) → gather into `inputs/`, capture provisional thinking in `analysis.md`.
  - **Phase A — Coherence:** draft `spec.md`; close with **3–5 targeted callouts** (most debatable
    last) — never just "please review".
  - **Commitment Gate:** developer signs off → freeze the spec. *This gate requires human judgment;
    the AI cannot cross it.*
  - **Phase B — Action:** implement against the spec (test-first prompting; cap AI-only iterations at
    `iteration_cap`; deep review at every integration boundary — see `sdd-method.md`).
  - **Phase C — Loop return:** review against the spec; record non-obvious calls in `decisions.md`;
    distil reusable lessons toward `patterns/`. Run [`checkpoint.md`](checkpoint.md)
    (`/henergy-checkpoint`) at the end of each session and at milestones to fold this back into the
    activity log and shared artifacts — don't wait until the ticket closes.

### 6. Honor the engagement profile
Calibrate to `engagement_profile`: for a `disciplined_active` developer, keep artifacts current and
expect them to feed back; for `technical_low_discipline`, do more of the scaffolding/upkeep yourself
and surface the structure rather than expecting them to maintain it; for `conceptual_platform` /
`strategic_consumer`, capture their input without requiring repo discipline.

---

## Output shape (reference)

```
progress/members/<username>/tickets/<issue#>-<slug>/
├── spec.md                  ← always
├── analysis.md              ← if Discovery
├── brief.md                 ← if analysis ticket with a team-facing output
├── implementation-plan.md   ← if AI-assisted code
├── decisions.md             ← if >2 non-obvious decisions
├── inputs/                  ← if Discovery research material
└── .local/                  ← if heavy/private material (gitignored)
```
