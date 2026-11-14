# Analysis — static hosting for value-x.com (provisional, living)

> Confidence tags: **V** verified in the repo, **K** general knowledge to re-check against current docs,
> **U** unknown. Nothing here is a recommendation.

## What the site needs from a server (V, checked 2026-11-14)

- `Program.cs`: Razor components, static assets, antiforgery, HTTPS redirect. No API, DB, auth, services.
- Pages are static SSR: three routes (`/`, `/projects/denyse`, `/projects/solarnow`) plus `/Error`. After
  Block 1: `/platform`.
- Contact form posts directly to `formsubmit.co`; no server handler.
- `App.razor` still loads `_framework/blazor.web.js` (enhanced navigation). A static export would either
  need that file captured or the script dropped; with no interactivity nothing relies on it.
- Nothing is rendered per user or per request except the antiforgery/`Error` plumbing. So the output is
  deterministic HTML plus assets (13 MB, 65 files).

## Ways to get static output (K, to verify)

| Option | What it is | Notes |
|---|---|---|
| A. Keep the .NET host | Status quo | No work; cost and manual deploy stay. |
| B. Crawl-and-publish | Run the app in CI, crawl the 4 routes (wget/httrack or a small script), publish the folder | Smallest change, code stays Blazor. Needs the route list kept in sync; `_framework` and fingerprinted assets (`@Assets[...]`) must be captured. |
| C. A .NET static-site generator for Blazor/Razor | Third-party tools exist (names to check); .NET 9 itself has no built-in static export | Adds a dependency; maturity unknown **U**. |
| D. Standalone Blazor WebAssembly | Static files by design | Changes the runtime model and download size; poor fit for a static marketing page. |
| E. Rebuild as plain HTML or another static generator | Replace the Blazor site | Overlaps Block 3 (refresh, Lovable question). Biggest change, possibly the same effort as the refresh. |

## Where static files can be hosted (K, to verify)

- **Azure Static Web Apps:** has a Free tier and a paid Standard tier; custom domains and managed TLS
  included; Terraform resource `azurerm_static_web_app`. Deploys from GitHub Actions.
- **Azure Storage static website** (+ optional CDN / Front Door for custom domain TLS): Terraform supported.
- **Non-Azure:** Cloudflare Pages, GitHub Pages, Netlify, Vercel: also static; depends on whether the team
  wants to stay on Azure.
- Terraform support: all of the above are manageable in Terraform; not a differentiator. **U** which cloud
  Zek's Terraform plan targets.

## Cost (U)

Static hosting is typically free to a few euros per month; a dedicated .NET app host is typically paid
per instance per month. **The saving cannot be stated until we know what value-x.com runs on today and
what it costs.** Do not quote a number.

## Side benefits and risks to weigh

- (+) Deploy becomes "push to branch, pipeline publishes", removing the dependency on Zek doing it by hand.
- (+) Smaller attack surface; no runtime to patch.
- (-) Any future need for server behaviour (own form handler, auth, API) would need an add-on (functions) or a move back.
- (-) Build step must capture all routes and assets; easy to miss a page.
- (-) Zek's Terraform plans may already settle the hosting question.

## Relevant to Block 1

None needed: `/platform` is plain HTML (an iframe) and ships with whatever deploy exists. Note: a static
move would not change the iframe or the CSP/framing rules (those are the Hypha side and the host headers;
static hosts can set headers via config, e.g. a CSP `frame-src`, if one is ever added).

## Next steps (discovery)

1. Ask Zek: current host, cost, deploy method, Terraform scope.
2. Ask Edgar/Ger: is Block 3 replacing the site, and when.
3. If still relevant: try Option B locally on a branch (read-only experiment, no push) to see how clean the output is.
4. Write `brief.md`; raise with the team; then decide whether it becomes an issue.
