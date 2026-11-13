# GitHub Sync — keeping the board & issues in step (via the `gh` CLI)

> The tool-agnostic procedure for reading and updating Hypha's GitHub **issues, PRs, and project
> board** on a developer's behalf. It is a **skill over the `gh` CLI**, deliberately *not* a GitHub
> MCP server (see the principle below). **Gated to technical developers** who opted in. The Claude
> Code `/henergy-board` skill and the Cursor command wrap it; it also runs as a **soft step**
> inside [`task-init.md`](task-init.md) and [`checkpoint.md`](checkpoint.md). Conventions live in
> [`../planning/issue-guidelines.md`](../planning/issue-guidelines.md) — don't duplicate them here.

---

## Principle — skill over MCP

For an external connection like GitHub, prefer a **thin skill that shells out to the `gh` CLI** over
a standing MCP server. The CLI is invoked **on demand** (no always-on server, no token overhead, no
extra surface), it's already installed and authenticated on a developer's machine, and it composes
with everything else the AI can run. The skill is just *knowing when and how* to call `gh` — the
intelligence, not the transport.

## When this applies (gating — check before doing anything)

Run GitHub sync **only** when the developer's config opts in:

- The general `AGENTS.local.md` has a `github.enabled: true` block (see schema below), **and**
- the developer is on a **technical** engagement profile (`disciplined_active` or
  `technical_low_discipline`).

If `github.enabled` is absent/false, or the profile is `conceptual_platform` / `strategic_consumer`,
**skip silently** — don't offer it. If a technical developer has no `github` block yet, you may offer
to set one up (that's part of [`personal-config.md`](personal-config.md) / `/henergy-dev-setup`).

Before the first `gh` call in a session, confirm the CLI is available (`gh --version`) and
authenticated (`gh auth status`). If `gh` is missing, **offer the install command for the developer
to run** — don't auto-install. Project-board commands need the `project` scope:
`gh auth refresh -s project` (offer it; don't run silently).

## The golden rule — reads are free, writes ask first

- **Reads** (`view`, `list`, `status`) — run freely to ground your understanding.
- **Writes** — creating/editing issues, commenting, labelling, opening PRs, moving board cards,
  setting fields — are **outward-facing actions**. **State exactly what you're about to write and
  get a yes first.** Honor the developer's `decision_posture`: surface the proposed change, don't
  silently push it.

## What it does

### Read (ground the work)
```bash
gh issue view <n> --repo Hypha-Energy/<repo>            # one issue
gh issue list  --repo Hypha-Energy/<repo> --assignee @me --state open
gh pr list     --repo Hypha-Energy/<repo> --assignee @me
gh project item-list 13 --owner Hypha-Energy               # the board (hypha.energy website development)
```

**Board reference:** the active work board is at
`https://github.com/orgs/Hypha-Energy/projects/13/views/1` — share this with the developer during
setup so they can confirm they're looking at the right board.

### Write (only after a yes) — follow [`issue-guidelines.md`](../planning/issue-guidelines.md)
```bash
gh issue create --repo Hypha-Energy/<repo> \
  --title "type(scope): description" --body-file <file> --label <labels>   # naming + label taxonomy
gh issue edit <n> --repo Hypha-Energy/<repo> --add-label <label>
gh issue comment <n> --repo Hypha-Energy/<repo> --body-file <file>
gh pr create   --repo Hypha-Energy/<repo> --fill                            # link the issue
gh project item-edit --id <item> --project-id <id> --field-id <id> --single-select-option-id <id>
```
Title convention, label taxonomy, board columns (Backlog → Next → In Progress → Blocked
→ Done), Priority/Target-date fields, and draft items are all defined in
[`issue-guidelines.md`](../planning/issue-guidelines.md). Apply them; don't reinvent.

## The soft-step hooks (where this fires automatically)

- **In [`task-init.md`](task-init.md) (starting work):** if gated-in, read the linked issue to ground
  the spec. If the work has **no issue** and belongs on the board, *offer* to create one (correct
  `type(scope):` title, labels, set Priority/Target date). **Whether the issue was just created or
  already existed, add it to project #13 if it isn't there yet** — this is the step most likely to
  be missed. Default status: Backlog. Proceed once confirmed.
- **In [`checkpoint.md`](checkpoint.md) (recording progress):** if gated-in, *offer* to reflect the
  session on GitHub — a progress comment, a board-column move (e.g. Backlog → Next → In Progress; an issue stays In Progress
  while its PR is open), label/field updates. Mirror the same status you wrote into the ticket's `spec.md` header.
- **Ad-hoc:** the developer says "update the board", "sync this to GitHub", "move #X to In Review",
  "open the PR" → run this procedure directly.

## Per-developer config (general `AGENTS.local.md`)

```yaml
github:
  enabled: true
  cli: gh
  project: Hypha-Energy/projects/13        # "hypha.energy website development" (org board)
  watch: issues and PRs assigned to me  # what this developer tracks; varies per person
```
The **project is a team default** (everyone shares board #13); the **`watch` filter is personal**
(one developer tracks only their assigned issues; another might track a whole label/area). Keep both
in the developer's config so the AI knows what to surface without asking each time.

## Relationship to the rest of the repo

- **Conventions:** [`../planning/issue-guidelines.md`](../planning/issue-guidelines.md) (titles,
  labels, board, fields) — the team-facing reference.
- **Source of truth:** the GitHub board is canonical for ticket status; `planning/current.md` and the
  per-ticket `spec.md` headers are synced narrative views, not replacements.
- **Boundary:** GitHub holds the *public* work record; heavy/private working material stays in the
  ticket's `.local/`. Don't paste private context into an issue/comment.
