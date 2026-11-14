---
name: henergy-dev-setup
description: One-time setup of a developer's personal process config (the AGENTS.local.md files) for this Hypha context repo. Use when a developer says "set up my context/preferences", "init my config", "onboard me", "dev setup", when no AGENTS.local.md exists at their scope, or when their working preferences changed and need updating.
audience: dev
---

# henergy-dev-setup — set up a developer's personal config

**Triggers:** "set up my context/preferences", "init my config", "onboard me", "dev setup", no
`AGENTS.local.md` exists at the developer's scope.

**Who it's for:** Technical developers. One-time (re-run only to update preferences — uncommon).

Sets up the developer's two personal `AGENTS.local.md` files via a short interview:
- **General config** at the repo root (`/AGENTS.local.md`) — how they use the whole repo
- **Developer config** at their member scope (`progress/members/<username>/AGENTS.local.md`) — SDD
  working settings for tickets

Both files are gitignored. After setup, any AI session reads them in cascade order and knows exactly
how to work with this developer (review mode, decision posture, SDD level, tone, language).

## Steps

1. Read [`workflow/personal-config.md`](../../../workflow/personal-config.md) (schema + init procedure) and
   both templates: [`AGENTS.local.general.template.md`](../../../workflow/AGENTS.local.general.template.md) and
   [`AGENTS.local.developer.template.md`](../../../workflow/AGENTS.local.developer.template.md).
2. Read the root [`AGENTS.md`](../../../AGENTS.md) for the contributor profiles and the Coherence
   Loop frame. Identify which profile (A–F) this developer fits —
   see [`workflow/contributor-profiles.md`](../../../workflow/contributor-profiles.md).
3. Follow the **init procedure** in `personal-config.md`: run the short interview, then fill and
   write **both** files — confirm they're gitignored, and invite corrections. (When updating, touch
   only the scope that changed.)
4. **If `github.enabled: true` is in the developer's general config:** confirm `gh` is installed and
   authenticated, and point the developer to [`workflow/github-sync.md`](../../../workflow/github-sync.md).

Do not duplicate the schema or steps here: `workflow/personal-config.md` is the source of truth.
(No satellite step: this repo has no satellite mechanism yet; code repos are cloned read-only under
`../`, see `solution/tech/codebases.md`.)
