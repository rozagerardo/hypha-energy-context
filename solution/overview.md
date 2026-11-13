# Hypha Energy: overview

> Seeded 2026-11-13 from the Hypha-Energy org repos (READMEs, `v2` docs, website), Ger's agreement notes
> and the Hypha DAO white-label work. Labels: **R** read in a repo or doc, **S** stated by a founder
> (unverified in code), **U** unknown. Fix in place as understanding deepens.

## What it is

A community energy platform. Members of an **Energy Community** produce, store and consume energy
together; meters report every 15 minutes; an automated EMS dispatches batteries/EV/solar; a settlement
layer splits value fairly; governance and ownership run on the Hypha platform (tokens, Spaces,
proposals). **R** (`Hypha.Energy` README, `v2` docs).

## Business shape (S)

From Edgar's original website brief: target is existing communities with an installation (short time to
market); local **Orchestrators** activate "minimum viable ecosystems" (Iberia, Nordics, UK, Poland in
negotiation); Hypha Energy takes 30% non-dilutive equity; five revenue streams. Treat as unverified until
confirmed; the website brand book requires claims to match what the platform does today.

## Two layers (do not mix)

| Layer | What | Where |
|---|---|---|
| Energy product | Metering, EMS, price oracle, forecast, billing, EMOS web app, REST API | `Hypha-Energy/*` repos (C#/.NET, Python) |
| Hypha platform | Spaces, governance, treasury, tokens (EPARTS, EVOICE, ECREDITS, EURCREDITS), white-label embed | `hypha-dao/hypha-web` (Hypha DAO) |

Hypha Energy has its own Space on the Hypha platform. As of 2026-11-09: 1,128,072 EPARTS of 8,779,500
issued, about 206 EURC, almost no cash, so contributions are effectively paid in EPARTS (1 EPART = 1.00 EUR
in the Space). **R** (treasury view).

## Surfaces

- **hypha.energy**: the public website (Block 2 of Ger's agreement). Marieke leads.
- **ValueX** (value-x.com) and **DENYSE** (denyse.org): partner/grant-facing sites; the demo `/platform` goes here.
- **EMOS** (Blazor Server) and **API**: the product's operational UI and single source of truth.
- Pilot communities include Ponta do Sol (Portugal; first white-label client). Grant driver: a Portuguese
  grant for Energy Communities.

## People (as known)

Edgar Kampers (founder; decides scope/tiers), Marieke Post (go-to-market, website lead), Zekeriya Aydin
(technical lead: .NET platform, data pipeline, sites, deploys), Alexandre Prate (founder-side; hosting and
platform access; also active in Hypha DAO), Vlad (value engine in the v2 plan), Alaeddine (MSc thesis on
the planning controller). Others in the org: roles unknown.

## Where to go next

- Architecture and repo map: [`tech/`](tech/AGENTS.md), especially [`tech/codebases.md`](tech/codebases.md).
- The energy-community model (settlement, pricing, tokens): [`product/`](product/AGENTS.md).
- Work in flight: [`../progress/`](../progress/AGENTS.md), [`../planning/current.md`](../planning/current.md).
