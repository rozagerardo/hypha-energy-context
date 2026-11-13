# Codebases

> Read 2026-11-13 (READMEs, trees, recent commits; three repos cloned). Org: https://github.com/Hypha-Energy.
> 8 of 11 repos are private; all proprietary / internal. Local clones: `C:\Users\rozag\projects\hypha-energy\src\`
> (marked **cloned**). Branch names differ: some repos use `master`.

## Cloned

### `hypha-energy` : the hypha.energy website (cloned, `main`)
- **Stack:** Vite + React + TypeScript + shadcn/Tailwind, Node `server.js`, i18n (`src/i18n`, `docs/i18n-translation-todo.md`),
  prerender for crawlers (`scripts/prerender*.mjs`), sitemap/`llms.txt` generated at build from `scripts/site-routes.mjs`.
- **Run:** `npm i`, `npm run dev`; tests `npm test` (vitest); lint `npm run lint`.
- **Deploy:** push to `main` triggers `.github/workflows/azure-deploy.yml` (Node 24, `npm run build:prerender`,
  zips `dist` + `server.js` + `private/` to Azure App Service). **Merging to main deploys.**
- **Rules:** `Guidelines.md` (brand book v3.1, canonical for visuals, voice, messaging; closed value sets;
  updated in `Brandbook`, copied out at release), `CHANGELOG.md` (written for Zek; entries move here when merged),
  `docs/brandbook-private.md` (gated `/brandbook` served by `server.js`, `BRANDBOOK_USERS`).
- **Process:** issues labelled `website-backlog`, org project #13. PRs are typically Claude-authored branches
  merged by Marieke (`Post4Marieke`) or Zek. Open issues (2026-11-13): #25 to #43 (shared layer vs Guidelines v3.1, unprovable
  claims in "Our ambition", SEO tags, review address #32, unused components #34).
- **Pages:** Home, Team, Roadmap, CaseStudy, ForEsoPartners, Regulations, Presentation(+PT), OnePager (investor only),
  Adelaide, Nev, workshops (Sunshine Coast), MediaKit, GenderEqualityPlan.
- **Gotcha:** README top is a stale pitch-deck prompt, not setup docs.

### `Hypha.Energy` : the platform monorepo (cloned, `master`)
- **Stack:** .NET 9/10, Blazor Server, ASP.NET Core API, PostgreSQL/TimescaleDB, MQTT (EMQX), Azure. Proprietary.
- **Projects:** `Hypha.EMOS` (web app: communities, users, device pricing, market dashboard, EMS panel),
  `Hypha.API` (REST, single source of truth, sole writer of EMS control and community/user data), `Hypha.EMS`
  (periodic rule-based dispatch worker), `Hypha.IoT.Generator` (meter simulator to MQTT / IoT Hub),
  `Hypha.PriceEngine` (separate repo in README, folder present), `Hypha.Invoicing`, `Hypha.Web`, `Hypha.Shared`.
- **Docs:** `docs/api`, `docs/design/price-and-settlement.md`, `.github/copilot-instructions.md`.
- **Process:** Zek commits directly (recent: smart meters tab, device discovery, member billing, metrics performance).
  Issues #2/#4 (DSO data from Nuno), #3 (configuration proposal). 54 commits.
- Outside the four agreement blocks; useful for domain context.

### `value-x.com` : ValueX site (cloned, `master`)
- **Stack:** .NET 9 Blazor Web App, static server-side rendering only (no interactivity), no tests, no DB. Has its own `CLAUDE.md`.
- **Run:** `dotnet run --launch-profile http` (http://localhost:5249).
- **Pages:** `Home` (`/`, sections `#home #offer #who-we-help #projects #contact`), `Denyse` (`/projects/denyse`), `SolarNow`.
- **Gotchas:** `Routes.razor` has no `DefaultLayout`, so `MainLayout`/`NavMenu` are unused leftovers and each page renders its own
  inline `<nav>` (duplicated; nav edits touch every page). Many inline styles; stale `Content Update` image paths in the csproj.
- **Deploy:** manual, by Zek (2026-11-13). 3 commits only (Feb and May 2026). Block 1 adds `/platform` here.
- **Open question:** Block 3 mentions a Lovable project for ValueX; this repo is Blazor. Which is live? **U**

## Not cloned (read via `gh api` on 2026-11-13)

| Repo | Vis. | What |
|---|---|---|
| `v2` | public | Docs: `Dictionary.md` (ubiquitous language), `PriceOracle.md`, infrastructure responsibility matrix (Zek: edge/MQTT/DB/oracle worker; Vlad: fair-split, EMS, settlement), Elysium-to-Hypha mapping, user metrics API |
| `Price_Engine` | private | C# + TS oracle price engine: day-ahead price service, calculator, retailer markups, loss profiles, SQL |
| `Hypha_forecast` | private | Python ARIMA forecast demo (train every 3 days, forecast every 15 min, Azure Container Apps) |
| `Hypha_EMS` | private | Python (linopy) community EMS optimiser prototype |
| `Solar_Energy_Anomaly_Detection` | private | Python XGBoost PV fault detection; FastAPI + Streamlit + Bicep |
| `alaeddine_thesis_project_planning_controller-` | private | MSc thesis: 24 h purpose-driven planning controller |
| `AI-pitch` | public | One-page pitch deck for Unicorn Events Lisbon 2026 (branch `Pitches`) |
| `demo-repository` | private | GitHub template demo; ignore |

## Cross-repo facts

- Zekeriya Aydin is the technical contact for all four agreement blocks and deploys value-x by hand; he also commits directly to `Hypha.Energy`.
- The DENYSE site is not in the org.
- The Hypha DAO side (`hypha-web`) provides the Energy tab and the white-label embed; see `overview.md` here.
