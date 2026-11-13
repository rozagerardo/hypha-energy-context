# AGENTS.md — `patterns/`

> Read the root [`AGENTS.md`](../AGENTS.md) first for the global frame.

## What this directory is

`patterns/` holds **candidate Pattern Cards** — crystallized, reusable knowledge distilled from
real development work, formatted for eventual inclusion in Hypha's **Skill Library** (V3
Knowledge Model, **Layer 1 — Canonical Knowledge**).

These are **pre-platform formalization**: they live here as `candidate-*.md` files until the
Hypha platform has a native home for them. "Candidate" means *not yet formalized in the
platform* — the knowledge is captured in the canonical format, but it hasn't been promoted into
the platform's Skill Library yet.

## Pattern Card format

Each card is compact (5–10 lines), **decision-rule style, not prose**:

```
Name:               [short name]
Trigger:            [observable signals that indicate this pattern applies]
Diagnosis:          [what those signals usually mean]
Move:               [the intervention / approach]
Why it works:       [causal logic — why this move addresses the diagnosis]
Risks:              [when it backfires or doesn't apply]
Default parameters: [thresholds, timelines, key variables]
Skill mapping:      [which Hypha Skill(s) this card belongs to]
Status:             Draft | Field-tested | Canonical | Deprecated
```

Write in `if–then–because` form: *if [context] + [signals] → [move], because [causal logic],
unless [constraint] → [alternative]*. Each card tags **at least one Skill** (prevents
wiki-sprawl). Lifecycle: `Draft → Field-tested → Canonical → Deprecated`.

## Conventions

- File naming: `candidate-<short-slug>.md`.
- Each card should carry a brief preamble: what the file is, what "candidate" means, and a
  reference to the ticket/work that produced it.
- A card earns promotion from `Draft` → `Field-tested` only after it has been used on real work
  beyond its author. Don't inflate Status.
- Cards are **worked data** — distilled from real cases, not speculative best-practices.

## How this relates to the rest of the repo

When work tracked in `progress/` produces a reusable, generalizable pattern, it gets distilled
into a card here — that's the "loop return" feeding learnings back into shared Coherence. The card
is the durable output; the ticket that produced it eventually closes.

## Personal context

If `AGENTS.local.md` exists in this directory, read it — it contains personal context scoped to
this area (gitignored, not committed).

## Note for this repo

No cards here yet. The SDD workflow card (`candidate-sdd-workflow.md`) lives in the Hypha DAO repo
(`hypha-context/patterns/`); this repo reuses that workflow. Add Hypha Energy cards as real lessons ripen.
