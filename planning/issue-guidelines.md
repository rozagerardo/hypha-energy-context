# Issue and board conventions (Hypha-Energy org)

> Observed 2026-11-13, not formally agreed. Replace with the team's rules when Edgar/Zek/Marieke state them.
> The Hypha DAO conventions (`type(scope): description` titles, label taxonomy) are in `hypha-context`
> `planning/issue-guidelines.md` and do **not** automatically apply here.

## Observed

- **Board:** one org project, #13 "hypha.energy website development". Issue #35 asks to keep website issues
  out of the "Energy Anomaly Service" project (board hygiene), so more than one project exists.
- **Label:** `website-backlog` on website issues; they stay on GitHub until shipped, then move to `CHANGELOG.md`.
- **Titles:** plain, descriptive sentences ("Sitemap and llms.txt are hand-maintained and out of date"), no
  `type(scope)` prefix. Branch names seen: `fix/<slug>`, `chore/<slug>`.
- **PRs:** merged by Marieke (`Post4Marieke`) or Zek (`zekeriyaaydin`). Merging `main` of the website deploys it.
- **Changelog:** `CHANGELOG.md` is a short Keep-a-Changelog summary written for Zek, linking each issue.

## Rules for this repo's skills

- Reads are free; every write (issue, comment, label, PR, board move) states the exact change and asks first.
- Never push to an org repo or open a PR without Ger's explicit go-ahead.
- `--repo Hypha-Energy/<repo>`: always name the repo; there is no single default repo.
