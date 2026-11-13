---
# General personal context (machine-readable globals). Generated via workflow/personal-config.md.
# Lives at the REPO ROOT as /AGENTS.local.md. Gitignored (*.local.md), never committed.
# The broad "how I work with this whole repo" frame; scope-specific AGENTS.local.md files
# (e.g. progress/members/<username>/) refine it for particular work.
developer: <username>                       # e.g. gerroza
display_name: <Full Name>
role: <short role, e.g. "Hypha contributor — backend/systems; mobile/dev ownership">
comms_languages: [en]                        # e.g. [en, es]
verbosity: concise                           # terse | concise | detailed
ai_tools: [claude_code, cursor]
primary_areas: []                            # areas you work in most, e.g. [progress, workflow, solution/tech]
context_sync: remind                         # auto (pull silently — typical for non-technical) | remind (offer; never over WIP — typical for technical)
# github: (technical opt-in — omit the whole block if you don't want GitHub integration)
#   enabled: true
#   cli: gh
#   project: Hypha-Energy/projects/13           # team board "hypha.energy website development"
#   watch: issues and PRs assigned to me     # personal — what you track
---

# AGENTS.local.md — <Full Name> (<username>) · general personal context

> My general, cross-cutting context and preferences for working with this **whole repo**.
> **Gitignored — not shared.** Read first as the broad personal frame; scope-specific
> `AGENTS.local.md` files (e.g. in `progress/members/<username>/`) refine it for particular work.
> Living document — update freely.

## Who I am

<Role in Hypha, background, what I'm responsible for. What should the AI assume I know vs. explain?
e.g. "Backend/systems engineer; strong on APIs, data, infra; newer to Web3 specifics — explain
on-chain mechanics, assume backend fluency.">

## How I use this repo

<What I open this repo for, and which areas I rely on. e.g. how I use solution/ (orient on the
platform), progress/ (track work), planning/ (priorities), patterns/ (reusable lessons), workflow/
(how we work). What am I usually looking for?>

## How I read / write / work with the info

<Reading preferences (structure/summary-first, or deep?). Where and how I update things (living docs
as work happens?). How I engage with others' areas. What "good" looks like when the AI helps me
read or write here.>

## Communication & working preferences

<Tone, language(s), verbosity. When to ask vs. proceed. How to surface uncertainty and options.>

## GitHub integration

<Only if `github.enabled` above. Which board you work against and what you want watched (e.g. issues
& PRs assigned to you). Reminder: the AI reads freely but **asks before any write** (issues, comments,
labels, PRs, board moves). See workflow/github-sync.md and planning/issue-guidelines.md. Delete this
section if you don't use GitHub integration.>

## Cross-cutting boundaries

<What stays private/local vs. shared, and any norms that apply across all my work in this repo.>
