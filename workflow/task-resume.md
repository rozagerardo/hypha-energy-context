# Task Resume — In-Progress Ticket Session Opener

> The procedure an AI runs when a developer **resumes work on an existing ticket** in a new session.
> Unlike [`task-init.md`](task-init.md) (which creates the scaffold), this reads the existing ticket,
> orients the AI on current phase and status, surfaces open threads, and briefs the session on what
> comes next — so work continues smoothly without rebuilding context from scratch. Tool-agnostic;
> the Claude Code `/henergy-task-resume` skill wraps it. Its session-close bookend is
> [`checkpoint.md`](checkpoint.md) (`/henergy-checkpoint`).

---

## When to run this

A developer says *"I'm continuing #2296"* / *"let's resume my work on this ticket"* / *"picking up
where I left off on X"* / *"continue #X"* — or starts a **Phase B** (implementation) session after
Phase A (spec writing) was completed, possibly in a different tool or session.

*(If this is a brand-new ticket with no folder yet, run [`task-init.md`](task-init.md) instead.)*

## The procedure

### 1. Refresh context
Run [`sync-context.md`](sync-context.md) to ensure the local copy is current. For a technical dev
with work in progress, offer don't force; for non-technical, auto fast-forward if on clean `main`.

### 2. Load developer config
Read the developer's cascading `AGENTS.local.md` (general → member scope): review mode, decision
posture, engagement profile, verbosity. *If missing, offer to run
[`personal-config.md`](personal-config.md) (`/henergy-dev-setup`) before continuing.*

### 3. Load the ticket
Locate the ticket folder under `progress/members/<username>/tickets/<id>-<slug>/`. Read:
- **`spec.md`** — full file: the goal, acceptance criteria, Status header, SDD phase, Last touched.
- **Last entry in `activity-log.md`** — the watermark: what was last recorded.
- **`decisions.md`** (if present) — what's already been decided.
- **`implementation-plan.md`** (if present, and Phase B is active or upcoming).
- **`analysis.md`** (if present) — open questions, findings still in play.

If the ticket folder doesn't exist, offer to run `task-init` instead.

### 4. Assess the current phase
From the `spec.md` Status header, determine which SDD phase the ticket is in:

| Phase | Status in spec header | What it means |
|---|---|---|
| **Phase A — Coherence** | In Discovery / Spec Draft | Spec is still being written / refined. |
| **Commitment Gate** | Spec frozen / Pending sign-off | Spec is locked; implementation hasn't started. |
| **Phase B — Action** | In Progress / Implementing | Code is being written against the spec. |
| **Phase C — Loop return** | In Review / Wrapping up | Implementation done; reviewing + extracting lessons. |
| **Done** | Complete | Ticket closed (run only if there's unrecorded follow-up). |

### 4b. Satellite: Phase B spec copy
If the ticket is entering or in Phase B and the code repo has a satellite, follow
[`satellite.md`](satellite.md) procedure 3: copy the **frozen** spec to `spec-active.local.md` (parking
any other active one), or, if the spec is not frozen yet, point `pointers.md` at the live spec. Skip for
Phase A tickets with no code work.

### 5. Brief the session
State clearly and concisely:
- **Where we are:** current phase, spec status, what was last done (from the activity-log watermark).
- **Open threads:** unresolved questions in `analysis.md`, pending decisions, items flagged at last
  checkpoint.
- **What's next:** the logical next step — e.g., *"Continue spec callouts"* / *"Spec is frozen — begin
  Phase B: read the implementation plan and open the code repo"* / *"Implementation step 3 of 5 per
  the plan"* / *"Review pass + record decisions before closing."*
- **If Phase B:** explicitly remind to bring the code repo into scope — open in the same workspace
  (`/add-dir` for Claude Code, multi-root for Cursor). The implementation plan lives here; the code
  changes happen in the relevant repo under `../` (`hypha-energy`, `Hypha.Energy`, `value-x.com`).

### 6. State the mode
Always state the **review mode** and **decision posture** from the developer's `AGENTS.local.md` —
never silently. Example: *"Using independent-with-callouts + human-decides per your config."*

### 7. Proceed or ask
- If the next step is clear and the developer said "let's continue", start on it.
- If there's ambiguity (multiple open threads, unclear priority), surface them and ask.
- **Do not re-scaffold** — the ticket folder already exists. Only add new artifacts if the ticket
  escalates to a higher SDD level (e.g., discovery is now needed → add `analysis.md`).

---

## Relationship to the other bookends

| Skill | When | What it does |
|---|---|---|
| [`task-init.md`](task-init.md) / `/henergy-task-init` | First session on a ticket | Classifies, picks SDD level, creates the scaffold |
| **[`task-resume.md`](task-resume.md) / `/henergy-task-resume`** | **Every subsequent session** | **Orients the AI, surfaces what's next, resumes** |
| [`checkpoint.md`](checkpoint.md) / `/henergy-checkpoint` | End of every session | Records what happened, folds into shared artifacts |

A long ticket: one `task-init` → many (`task-resume` → work → `checkpoint`) cycles.
