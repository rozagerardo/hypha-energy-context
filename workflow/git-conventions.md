# Git conventions — branch naming

> Shared by every code repo that uses this context (`hypha-energy`, `Hypha.Energy`, `value-x.com`).
> Tool-agnostic: any AI tool or human creating a branch follows this. Personal tool-specific files
> (`CLAUDE.local.md`, `AGENTS.local.md`) should point here rather than restate it.

## Pattern

| Kind | Pattern | Example |
|------|---------|---------|
| **Team / PR branch** | `<type>/<description>` | `fix/dm-room-type` |
| **Personal working branch** | `<username>/<type>/GH-<issue#>-<description>` | `rozagerardo/feat/GH-2256-banking-kyb-onboarding` |
| **Personal, no GH issue** (tooling, local setup) | `<username>/<type>/<description>` | `rozagerardo/chore/dev-tooling-setup` |

- **`<username>`** — the contributor's GitHub handle. Personal prefix = "mine, in progress".
- **`GH-<issue#>`** — the GitHub issue the work is tied to. Omit only when there is no issue.
- **`<description>`** — kebab-case, shortened from the issue title (e.g. #2268 → `unified-deposits-ui-transfers`).
- **Handing off / opening a PR for the team:** the branch may keep its personal name, or be renamed
  to drop the username prefix (`<type>/<description>`). Either way, don't rename a branch with an
  open PR without re-pointing the PR.

## `<type>`

Conventional-Commits-style, matching what the repo's remote branches already use:

| Type | Use for |
|------|---------|
| `feat` | New functionality |
| `fix` | Bug fixes |
| `docs` | Documentation only |
| `chore` | Tooling, config, deps, housekeeping |
| `spec` | Spec / planning-only branches |

Use `feat`, not `feature` (a few legacy `feature/*` branches exist; don't add more).

## Not ours (don't imitate)

Tool-generated prefixes exist on the remote and aren't a naming model: `cursor/*` (Cursor),
`dependabot/*`, `gh-readonly-queue/*` (GitHub merge queue).

## Provenance

Written down 2026-10-30. Before this, the convention was unwritten: observed from remote branch
prefixes (`feat` ≈ 167, `fix` ≈ 69, `docs` ≈ 15, `chore` ≈ 12) plus the personal pattern first set
in the Hypha DAO context repo (`hypha-context`, activity log, 2026-09). Adjust here if the team decides otherwise.
