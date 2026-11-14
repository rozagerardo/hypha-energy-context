# valuex-static-hosting — Can value-x.com be served as a static site, and would it cut cost?

> **Status:** Discovery (not started beyond first read). **SDD level:** 2 (analysis ticket, team-facing output).
> **Issue:** none yet. Deliberately no GitHub issue until it has been raised with the team (Zek, Edgar).
> **Last updated:** 2026-11-14. **Platform area:** ValueX site / hosting (touches Block 3 "ValueX refresh").
> **Priority:** low; do not let it block Block 1 (`b1-readonly-demo`).

## Question

`value-x.com` is a .NET 9 Blazor Web App using static SSR only (no interactivity, no DB, no API), deployed
by hand by Zek to a .NET host (exact host unknown). Could it be built to static files and served from
static hosting (and managed in Terraform, which Zek has mentioned wanting), and would that reduce cost
and deploy friction?

## Goal (output)

A short `brief.md` for the team: feasibility, the options with trade-offs, rough cost comparison against the
*actual* current hosting bill, what changes for deploys (manual -> pipeline), and what it means for Block 3.
Not a migration. Any migration is a separate, agreed ticket.

## Out of scope

Doing the migration; changing hosting; touching infra or Terraform; Block 1 delivery.

## Open inputs needed from others

- [ ] Where value-x.com runs today, what it costs, and how Zek deploys (Zek).
- [ ] Terraform plans: what is in scope, which cloud/subscription (Zek).
- [ ] Whether Block 3 will replace the site anyway (Lovable or other), which would make this moot (Edgar/Ger).

## Acceptance

Brief delivered, options presented without a pre-chosen winner, open inputs answered or marked unknown,
raised with the team before any issue is opened.
