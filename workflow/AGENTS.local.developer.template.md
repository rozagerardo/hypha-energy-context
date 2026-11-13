---
# Developer-progress config (machine-readable settings). Generated via workflow/personal-config.md.
# Lives at a member's work scope: progress/members/<username>/AGENTS.local.md.
# Gitignored (*.local.md), never committed. Identity, comms, and tooling live in the GENERAL config
# at the repo root (/AGENTS.local.md); this file refines how I work on development tickets.
developer: <username>                       # e.g. gerroza
display_name: <Full Name>
engagement_profile: disciplined_active      # disciplined_active | technical_low_discipline | conceptual_platform | strategic_consumer
spec_review_mode: independent_with_callouts # guided | independent_with_callouts | ai_suggested
review_callouts: 3-5
decision_posture: human_decides             # human_decides (default) | ai_lean_when_asked
discovery_default: explicit                 # explicit | inline
default_sdd_level: by_trigger               # 0 | 1 | 2 | 3 | 4 | by_trigger (recommended)
iteration_cap: 3                             # max consecutive AI-only iterations before a human review reset
---

# AGENTS.local.md — <Full Name> (<username>) · developer-progress config

> How I want the AI to work with me **on development tickets**. **Gitignored — not shared.**
> Refines the general personal config at the repo root (`/AGENTS.local.md`) for dev work.
> Read with the cascading `AGENTS.md` files. Living document — update freely.

## How I want you to work (dev)

- **Spec review:** <e.g. independent with 3–5 targeted callouts, most debatable last. State the mode
  you're using, don't pick silently.>
- **Decisions & analysis:** <e.g. surface findings, map the option space, flag what's load-bearing —
  I decide. No pre-filled recommendation unless I ask for a lean.>

## Current engagement context

<Active tracks, current focus, time constraints. The most frequently updated section.>

## Dev environment & tooling quirks

<Personal local-setup specifics worth remembering — own DB branch, debug config, etc. Shared
dev-setup belongs in the code repo's docs, not here.>
