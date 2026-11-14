# hypha-energy-context

Context layer for work on **Hypha Energy**: the AI-legible working memory of what the organisation and
its platform are, what is being done, and where it is headed. AI tools read it at the start of a task
and update it as the work happens, so context accumulates instead of being rebuilt every session.

It mirrors `hypha-context` (the Hypha DAO team repo), with one difference: **this one is personal for
now** (`rozagerardo/hypha-energy-context`, not in the Hypha-Energy org) and can be moved to the org
later if the team wants it.

> **Local path:** `C:\Users\rozag\projects\hypha-energy\src\hypha-energy-context`
> **Org:** https://github.com/Hypha-Energy
> **Personal sibling:** `ger-hypha-energy` (agreements, comms, personal notes)
> **Code clones (read-only, siblings of this repo):** `C:\Users\rozag\projects\hypha-energy\src\`

## New here? Say hi

Clone it, open it with your AI tool (Claude Code, Cursor, ...) and say "hi" or run `/henergy-start`.

## Folders

- **`solution/`**: what Hypha Energy is (`overview.md`), the repo map and architecture (`tech/`), the
  energy-community model (`product/`).
- **`progress/`**: per-member SDD workspaces (`members/<name>/`: activity log + ticket folders).
- **`planning/`**: current priorities and board conventions.
- **`patterns/`**: reusable lessons.
- **`workflow/`**: the SDD method and tool-agnostic procedures. Skills: `.agents/skills/` (canonical),
  `.claude/skills/` and `.cursor/skills/` (wrappers), all prefixed `henergy-`.

## For AI tools

Don't start from this README. Read **`AGENTS.md`**, then the `AGENTS.md` of the folder you work in.
Claude Code: `CLAUDE.md` points to `AGENTS.md`.
