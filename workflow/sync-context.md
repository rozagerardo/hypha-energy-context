# Sync Context — keep the local copy of the context current

> Keeping the **local clone of this repo up to date** (a `git pull`) so the AI is always reading the
> *current* shared context, not a stale snapshot. This is a **cross-cutting step** in the workflow,
> not a one-off: it runs at session start, before starting a task, and before merging — and on
> demand. Profile-aware: **automatic** for non-technical users, **a reminder/offer** for technical
> ones (who may have work in progress). Tool-agnostic; the Claude Code `/henergy-update-context` skill wraps it.
>
> **Distinct from [`github-sync.md`](github-sync.md):** that one updates the *GitHub board/issues*;
> this one refreshes the *local context files* you're reading from.

---

## Why this exists

The shared context only helps if it's current. Anyone working from a local copy drifts the moment
someone else pushes. So "pull the latest context" is a defined step in the process — surfaced at the
natural moments rather than left to memory.

## When to run

- **Session start** (the front door — [`get-started.md`](get-started.md)) — the main moment.
- **Before starting a unit of work** ([`task-init.md`](task-init.md)) — so the spec is grounded in
  current context.
- **Before merging / at wrap-up** ([`checkpoint.md`](checkpoint.md)) — pull/rebase if behind.
- **On demand** — "update my context", "pull latest", "am I up to date?".

## The procedure

### 1. Detect the situation (don't assume a clean clone)
- **Is it a git clone at all?** If there's no `.git` (e.g. the person **downloaded a ZIP**), local
  `git` sync isn't possible — say so: they'd re-download for a fresh copy, or (better, especially for
  non-technical members) use the **live hosted surface** once it exists (a shared Q&A AI), where
  freshness doesn't depend on local git. Then skip the git steps.
- Otherwise `git fetch` and compare local vs `origin/main`: **up-to-date / behind / ahead / diverged**.
- Check the working tree: **uncommitted changes?** **on `main` or a feature branch?**

### 2. Act by profile (`context_sync`: `auto` | `remind` — default: `auto` for non-technical
profiles, `remind` for technical)
- **`auto` (non-technical, clean `main`):** if behind with a clean tree on `main`, **fast-forward
  `git pull`** and briefly note you refreshed the context. (If you unexpectedly find local changes or
  a non-`main` branch, fall back to `remind` — don't touch it.)
- **`remind` (technical):** report ahead/behind and the tree state, then **offer** to pull. **Never
  pull over uncommitted work or a feature branch silently** — flag it and let them decide (stash /
  commit / rebase is their call). Prompt especially at session start, when starting a task, and
  before merging.

### 3. Safety — never destroy work
No discarding uncommitted changes, no force, no clobbering a branch. If a pull would conflict or
isn't a clean fast-forward, **stop and surface it** — a human resolves it.

### 4. Proceed
Once current (or once you've flagged that it can't be), continue with whatever the session is for.

---

## Note for non-technical members

For someone who isn't going to manage a git clone, this step is a stopgap. The real answer to "always
current without effort" is the **live hosted surface** being explored (a shared NotebookLM / Claude
project fed from this repo) — there, the context updates on the hosting side and they just ask. Until
that exists, an `auto` pull on a clone is the closest thing.
