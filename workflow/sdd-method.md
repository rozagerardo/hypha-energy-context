# Spec-Driven Development (SDD) — the Method

> The development method this workflow enforces. Tool-agnostic. Read with
> [`task-init.md`](task-init.md) (how a ticket is scaffolded) and
> [`personal-config.md`](personal-config.md) (how a developer tunes it). Distilled into a Pattern
> Card at `hypha-context/patterns/candidate-sdd-workflow.md` (Hypha DAO repo).

## What it is

SDD is a development approach where a machine-readable **specification is written before code** (or
before a task is given to AI tools). The spec defines what the system should do — inputs, outputs,
constraints, edge cases, data contracts — and serves at once as:

- the AI's instruction set (what to implement),
- the review artifact (what to check the implementation against),
- the communication layer (what to share via PRs and issues).

It closes the gap specific to AI-augmented work: the AI implements **what you asked, not necessarily
what you meant**. The spec closes that gap structurally. SDD comes *before* tests — tests are one
output of a spec, which the AI can derive as part of implementation.

## Levels of specification (the constitution model)

SDD is fractal — it operates at several levels at once, each with the same structure (mission +
context + roadmap + spec) but different scope and lifespan. **Each level is written before the work
at that level begins.**

| Level | Scope | Lifespan | Contains |
|---|---|---|---|
| **Project constitution** | the whole repo/engagement | persistent, updated at replanning | mission/why, stack, guidelines, roadmap, global constraints |
| **Feature spec** | one ticket / deliverable | lives with the ticket | goal, contracts, acceptance criteria, phases, open questions |
| **Implementation brief** | one phase within a feature | ephemeral — consumed by the AI | scoped inputs, out-of-scope boundary, context pointers |

```
Constitution → [Feature: spec → implement → validate] → Replanning → [next Feature] → …
```

The constitution guides features without being rewritten by them; **Replanning** is the explicit
moment to promote a completed feature's learnings back into the constitution. In this repo, the
constitution layer is the cascading `AGENTS.md` files; feature specs live in
`progress/members/<username>/tickets/<id>/spec.md`; implementation briefs are the per-phase prompts
(kept in a ticket's `.local/` when heavy).

## File structure — separate on lifecycle, not topic

The right axis to split files is **who can change this, and when** — not topic. A file that mixes
*frozen intent* with *living discovery* is the failure mode.

| Level | Layout |
|---|---|
| **0 — Monolithic** | everything in one file. Works solo, breaks under review. |
| **1 — Temporal split** | `spec.md` (frozen) + `analysis.md` (living). Minimum viable SDD. |
| **2 — Standard** | `spec.md` + `implementation-plan.md` (Phase B brief) + `analysis.md`. |
| **3 — Separate acceptance** | add `acceptance.md` when criteria are automated or a separate person signs off. |
| **4 — Full constitutional** | `mission` / `spec` / `acceptance` / `decisions` / `plan` / `analysis`. Large teams with distinct roles. |

Rather than memorize layouts, escalate by **trigger** — see the table in
[`task-init.md`](task-init.md).

## The pre–Phase A Discovery phase

Standard SDD presents Phase A→B→C, but there's an implicit **Discovery** phase before A. Name it:

```
Discovery → Phase A (frozen spec) → Phase B (implement) → Phase C (review)
```

Discovery gathers external sources/prior art into an `inputs/` subfolder and captures early thinking
in `analysis.md` (labelled provisional). Naming it prevents two failures: discovery notes polluting
the frozen spec, or the "why behind the why" being thrown away.

## The per-ticket workflow

### Phase A — write the spec (before the AI implements)
1. **Goal** — one sentence: what does this change accomplish and why?
2. **Data contracts** — inputs, outputs, schema changes (new columns/fields, nullable vs required).
3. **Flow states** — happy path + ≥2 edge cases + relevant error/failure cases.
4. **Acceptance criteria** — "done" in observable, verifiable terms. **Always write this, including
   for analysis tickets.** For analysis tickets the criteria look different but are equally
   definable: e.g. *"team confirms recommendation + impl ticket opened and linked + spec status
   updated to Complete."* Leaving acceptance implicit is the failure mode — it makes "analysis
   complete" and "done" look like the same moment.
5. **Integration boundaries** — external APIs, auth gates, DB writes, webhooks — anything crossing a
   system boundary.

Close Phase A with **3–5 targeted review callouts** (most debatable last), never just "please
review" — this is what stops review from degrading into rubber-stamping. Sign-off freezes the spec;
that sign-off is the **Commitment Gate** and requires human judgment.

> **Commitment Gate by ticket type:**
> - *Implementation ticket:* developer signs off on the spec → spec frozen → Phase B begins.
> - *Analysis ticket:* team (or decision-maker) confirms the recommendation → that confirmation is
>   the gate. "Analysis complete" (brief written) ≠ "done" — the gate is the human confirmation,
>   not the writing.

### Phase B — give the spec to the AI
- Feed the full spec as the implementation brief, not just a task description.
- Ask for **tests derived from the spec before** implementation code (test-first prompting).

### Phase C — review against the spec (not line-by-line first)
1. **Structure** — does the organization match what the spec implied?
2. **Acceptance map** — trace each criterion to where it's implemented; gaps are where bugs live.
3. **Boundary review (deepest focus)** — auth/authz on every endpoint and mutation; DB writes
   (schema, nullability, migration correctness); external API error handling; valid/invalid state
   transitions.
4. **Edge cases from the spec** — test manually (AI handles happy paths well, edges inconsistently).
5. **Dependency verification** — every new import/package verified to exist (Rule 6).

### Phase D — iterate within the 3-iteration rule
Cap consecutive AI-only iterations at **3** before a human review reset. Run static analysis
(TypeScript strict, ESLint) after each iteration, not just at the end.

## Proven rules (research-backed, 2024–2026)

These survived 3-vote adversarial verification in a 26-source, 117-claim research sweep.

1. **Test-first prompting improves AI output (+8–12%).** Provide test cases before generation.
   → [TGen — arXiv 2402.13521, ASE 2024](https://arxiv.org/abs/2402.13521)
2. **Human review gaps are real and persistent.** AI-generated PRs carry ~1.7× more issues; automated
   tools are a first filter, not a replacement for your review.
   → [arXiv 2601.19287 — Microsoft Research, Jan 2026, 3,177 PRs](https://arxiv.org/pdf/2601.19287)
3. **Cap consecutive AI iterations at 3.** Security surface compounds with unchecked iteration.
   → [arXiv 2506.11022 — IEEE-ISTAS 2025](https://arxiv.org/html/2506.11022v1)
4. **Prompt/spec quality shapes output but doesn't eliminate variance.** Invest in Phase A — the spec
   is the primary quality lever. → [arXiv 2603.25146 — systematic review, Mar 2026](https://arxiv.org/abs/2603.25146)
5. **Treat all AI-generated code as potentially vulnerable.** ~45% contains vulnerabilities (Veracode
   2025, 100+ LLMs); integration boundaries always get deep manual review.
   → [arXiv 2412.15004 — Dec 2024](https://arxiv.org/abs/2412.15004)
6. **Verify every new dependency (slopsquatting).** ~5% of commercial-LLM code references
   non-existent packages — a supply-chain surface. Verify new deps exist before installing.
   → [arXiv 2406.10279](https://arxiv.org/abs/2406.10279)

## Per-ticket checklist

**Phase A (before starting):** goal sentence · data contracts · flow states (happy + edges + errors)
· acceptance in observable terms · integration boundaries flagged.
**Phase B (before giving to AI):** acceptance tests / I-O assertions written or delegated from spec ·
full spec in the brief, not just a task.
**Phase D (during impl):** iteration count tracked, reset after each manual review · static analysis
after each iteration.
**Phase C (during review):** structure before line-by-line · each criterion mapped to code ·
auth/authz on every endpoint+mutation · DB writes (schema, migrations, nullability) · external API
error handling · new dependencies verified · spec edge cases tested manually.

## Coherence Loop mapping

| Dev phase | Coherence Loop | Frozen artifact |
|---|---|---|
| Discovery + Phase A (spec) | Coherence — shared understanding | `spec.md` |
| Spec sign-off / PR open | **Commitment Gate** (human judgment) | Phase A approval |
| Phase B (implement) | Action | `implementation-plan.md` → code |
| Phase C (review) + decisions | Loop return → Coherence | `decisions.md`, Pattern Card |
