# Checkpoint — Session / Milestone Wrap-up Flow

> The procedure an AI runs to **record progress** — at the end of a session or day, after a
> milestone or PR, at a phase transition, or before a context compaction. It captures everything
> worked **since the last checkpoint** into the shared, living artifacts so organizational knowledge
> accumulates instead of evaporating when the session closes. The symmetric bookend to
> [`task-init.md`](task-init.md): task-init *opens* the SDD workspace, checkpoint *folds the work
> back in*. Tool-agnostic; the Claude Code `/henergy-checkpoint` skill wraps it. Grounds in
> [`sdd-method.md`](sdd-method.md).

---

## When to run this

A developer says some variant of *"let's checkpoint"* / *"wrap up for the day"* / *"update the
status and activity log"* / *"record where we are"*. Also run it proactively when:

- a unit of work crosses a **phase boundary** (Discovery → spec frozen → implementing → reviewed),
- a **PR is opened/merged** or a milestone lands,
- the session is **about to be compacted** or is ending, or
- enough has happened that the shared docs no longer reflect reality.

This is the **Phase C "loop return"** of the Coherence Loop — worked data fed back into Coherence.
It is *not* the Commitment Gate (that's the spec sign-off in task-init / Phase A).

## The procedure

### 1. Establish the watermark — what's changed since last checkpoint
- Read the **last dated entry** in `progress/members/<username>/activity-log.md` — that's the
  watermark. Everything after it is uncaptured.
- Reconstruct what happened since: the conversation this session, the working-tree / branch state of
  the relevant code repo (`git status`/`git log` if available), open threads, decisions made.
- Read the active ticket folder(s) under `progress/members/<username>/tickets/<id>-<slug>/` so
  updates extend the existing artifacts rather than duplicating them.

### 2. Verify acceptance criteria (before updating status)
For any ticket whose status is about to move to **Complete / Done**:
- Re-read the `spec.md` acceptance criteria section.
- Confirm each criterion is verifiably met. If any are unmet, record them as open threads (step 6)
  rather than marking complete.
- **Analysis tickets:** the gate is team/stakeholder confirmation of the recommendation, not just
  "brief written." Confirm that confirmation happened before updating spec status to Complete.

### 3. Update the ticket artifacts (worked data, not raw notes)
For each active ticket, update only what actually moved:

| Artifact | Update when |
|---|---|
| `spec.md` **header** | Status / SDD phase / **Last touched** changed (always touch this) |
| `spec.md` **body** | only if the spec is **still pre-Commitment-Gate** — a frozen spec is not edited; new direction goes to `decisions.md`/`analysis.md` instead |
| `decisions.md` | one or more **non-obvious decisions** were made since the last checkpoint (the "why", the alternatives, what it commits us to) |
| `analysis.md` | discovery/investigation continued — extend the living findings + open questions |
| `inputs/` · `.local/` | new research material / heavy artifacts arrived (raw → `.local/` or `inputs/`, never inlined into the shared docs) |

Don't fabricate progress. If a phase didn't advance, leave its status and say so.

### 3. Append to the activity log
Add a **dated entry** (newest first) to `progress/members/<username>/activity-log.md` summarizing
what was done since the watermark — terse, factual, linking into the ticket's `spec.md`/`decisions.md`
and any GitHub issue/PR. This is the chronological spine; keep it skimmable.

### 4. Update the platform view (if platform-relevant)
If the work advanced a platform area, update the matching `progress/platform/` file
(`features/<menu-area>/…`, `mobile`, `dev-infrastructure`, `web-runtime`) and keep the
member↔platform cross-links intact. Only touch areas with real movement.

### 4b. Sync GitHub (soft step, if gated-in)
If the developer is GitHub-gated-in (`github.enabled` + technical profile), *offer* to reflect the
session on GitHub — a progress comment, label/Priority/Target-date updates — mirroring the status
you just wrote into the ticket's `spec.md` header. Follow [`github-sync.md`](github-sync.md);
**reads are free, every write asks first**. Don't paste private `.local/` material into an issue.
Skip silently if not gated-in.

**Board column moves (explicit):**
- Ticket spec status → Complete + acceptance criteria met → move board item to **Done**
- PR opened → keep in **In Progress** (the board has no "In Review" column); mention the PR in a comment
- Blocked state confirmed → move to **Blocked**
Always mirror the spec status; don't leave the board stale.

### 5. Distil reusable lessons toward patterns/
If a genuinely reusable lesson emerged (a technique, a gotcha, a workflow refinement), **flag it as a
Pattern-Card candidate** for `patterns/` — draft it if it's ripe, or just note the candidate in
`decisions.md` so it isn't lost. (V3 Knowledge Model: worked data promoted to a reusable Skill.)

### 6. Surface open threads + persist
- State a short **"where we are / what's next"** — open questions, the next phase, anything blocked.
- **Run `git status` before closing.** Flag any unstaged or uncommitted changes — they are
  part of this session's work and must be accounted for: either staged + committed now, or explicitly
  acknowledged as intentionally deferred. Do not wrap up while the working tree has unresolved
  changes.
- These artifacts are the **shared, recoverable record**. Heavy/private material stays in `.local/`
  and is **not** committed.
- **Commit/push behavior:**
  - **Explicit invocation** (`/henergy-checkpoint` typed by the developer): **commit and push
    directly** — do not ask for confirmation. The explicit invocation is the go-ahead. Only pause
    if something is genuinely off (unexpected files, `.local/` accidentally staged, sensitive
    content, working tree conflicts).
  - **Proactive / automatic firing** (phase transition, pre-compaction, etc.): offer first — the
    developer may not expect a commit at that moment.
- **Before committing/pushing/merging, make sure the local context isn't behind** `origin` — if it
  is, pull/rebase first (see [`sync-context.md`](sync-context.md)) so you're recording onto current
  context, not a stale base.

### 7. Honor the personal config
Read the developer's `AGENTS.local.md` (cascade: general → member scope) and calibrate:
- **`verbosity`** — keep the summary at their preferred density.
- **`engagement_profile`** — for `disciplined_active`, expect them to confirm/refine the updates;
  for `technical_low_discipline`, do the upkeep yourself and just show the result; for
  `conceptual_platform` / `strategic_consumer`, capture the outcome without requiring repo discipline.
- **`decision_posture`** — surface what changed and let them decide; don't silently rewrite intent.

---

## What this updates (reference)

```
progress/members/<username>/
├── activity-log.md                         ← dated entry appended (step 3)
└── tickets/<id>-<slug>/
    ├── spec.md         (header always; body only if pre-gate)   ← step 2
    ├── decisions.md    (new non-obvious calls)                  ← step 2
    └── analysis.md     (continued discovery)                    ← step 2
progress/platform/<area>.md                 ← if platform-relevant (step 4)
patterns/<candidate>.md                      ← if a reusable lesson ripened (step 5)

```

## Relationship to task-init

[`task-init.md`](task-init.md) and this file are the two bookends of a unit of work:
**task-init** classifies, picks the SDD level, and scaffolds the ticket (Phase A kickoff);
**checkpoint** records what happened and folds it back into the shared context (Phase C loop return).
Between them is the SDD lifecycle in [`sdd-method.md`](sdd-method.md). A long ticket has one
task-init and **many** checkpoints.
