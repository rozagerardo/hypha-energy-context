# AGENTS.md — `solution/`

> Read the root [`AGENTS.md`](../AGENTS.md) first for the global frame.

## What this directory is

The **persistent, evolving understanding of Hypha Energy**: what it is, how it works and the decisions
behind it. Knowledge, not a task log: updated as understanding deepens or the platform changes, not per
ticket. It describes what things *are*; status lives in `progress/`, planning in `planning/`.

**Start at [`overview.md`](overview.md)**, then the dimension that fits your task.

```
solution/
├── AGENTS.md       ← you are here
├── overview.md     ← what Hypha Energy is: read first
├── tech/           ← architecture, repo map, build/deploy facts
│   ├── overview.md     layers and data flow
│   └── codebases.md    each repo as an engineering surface (stack, run, deploy, gotchas)
└── product/        ← the energy-community model
    └── energy-community-model.md   settlement, pricing oracle, ownership, tokens
```

Add dimensions (`brand/`, `ecosystem/`, `workspace-stack/`) only when there is real worked content. Don't
scaffold empty ones.

## Conventions

- Keep it accurate: fix sections in place rather than appending corrections.
- Neutral, reference voice, for any contributor.
- Label evidence: **R** read in code/docs, **M** measured/run, **S** stated by a person, **U** unknown.
- The brand book (`Guidelines.md` in the `hypha-energy` repo) is canonical for voice and visuals. Point to it; don't copy it.
- Org repos are proprietary: summarise, don't paste source or data.

## Personal context

If `AGENTS.local.md` exists in this directory, read it (gitignored).
