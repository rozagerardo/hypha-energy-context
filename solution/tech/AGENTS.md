# AGENTS.md — `solution/tech/`

> Read the root [`AGENTS.md`](../../AGENTS.md) and [`solution/AGENTS.md`](../AGENTS.md) first.

## What this directory is

The **technical dimension**: how Hypha Energy is built. Read before assisting with any Hypha Energy
development task.

| File | Purpose |
|------|---------|
| `overview.md` | Layers and data flow: edge, ingestion, oracle, EMS, settlement, API/EMOS, the Hypha platform bridge. |
| `codebases.md` | The org repos as **engineering surfaces**: stack, how to run, how it deploys, who merges, gotchas. Which repos are cloned locally. |

## Conventions

- Knowledge, not a task log; fix sections in place when the code moves on.
- Source code and canonical setup live in the repos themselves (read-only clones in
  `C:\Users\rozag\projects\hypha-energy\src\`). Gathered local-dev gotchas and cross-repo "how do I run
  this" insight belong here (add `local-dev/` when there is content).
- Proprietary code: summarise and point; don't paste.

## Personal context

If `AGENTS.local.md` exists in this directory, read it (gitignored).
