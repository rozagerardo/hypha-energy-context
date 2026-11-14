# b1 — Read-only demo of the pilot communities (`/platform`)

> **Status:** **FROZEN 2026-11-14** (Ger signed off the spec: "we can freeze, and proceed with the implementation").
> Direction changes after this go to `decisions.md`, not here. This is Ger's sign-off on the **technical spec**;
> Edgar's sign-off on the Block 1 wording / the delivery (the agreement's Commitment Gate) is separate and still pending.
> **Phase B in progress (2026-11-14):** code written in `value-x.com` (branch `rozagerardo/feat/platform-pages`, uncommitted); server-side checks pass against the local hypha-web; browser/visual check pending. See `decisions.md` D9-D12.
> **SDD level:** 2 (standard). Phase B (code) happens in `value-x.com`, in a fresh session via the satellite.

## Goal

Partners, funders and reviewers can walk through a real Energy Community on the Hypha platform, read-only,
from the DENYSE and ValueX sites, each embedding the Hypha white-label iframe. Anonymous visitors; no write.

## Scope (agreement as posted in the Space 2026-11-14)

1. **ValueX site:** a "Platforms" navbar entry (dropdown) listing each community, a `/platform` index page, and
   one page per community embedding it. First community: `ponta-do-sol-energy-community` (public Space in prod,
   flagged `demo`; not Hypha Energy's own Space).
2. **DENYSE site** (`denyse.org/pontodelsol`, Edgar's wording): same snippet. **Blocked** on Edgar's admin contact.
3. Public sections browsable; account-bound actions do nothing in the embed; the page links to the full Hypha app
   (new tab). No synthetic Space needed.

Acceptance: pages load for an anonymous visitor, nothing can be written, a landing is in place, Edgar signs off.
Posted wording is looser ("potentially read-only", "write is not a strict requirement", "one snippet or DNS setup");
we keep read-only. Detail: `ger-hypha-energy/agreements/112026/posted-in-space-20261114.md`.

## Decisions (Ger, 2026-11-14)

- **D1 Structure = option C**, plus an index: navbar "Platforms" dropdown; `/platform` (index, kept so the agreement's
  literal path exists) and `/platform/<slug>` per community. Slug = the Hypha Space slug. Path differs from the
  agreement's example: Ger will mention it to Edgar at the demo.
- **D2 Hypha-side config:** new client `valuex` in `WHITE_LABEL_EMBED_CONFIG`; `denyse` untouched. Verified in
  `resolver.ts`: clients sharing a slug are allowed; origins are unioned.
- **D3 Origins:** both `https://value-x.com` and `https://www.value-x.com` (www is served directly, not redirected;
  checked 2026-11-14). Plus `http://localhost:5249` only in the local test config, not in prod.
- **D4 Hand-over:** PR to `Hypha-Energy/value-x.com`; Ger pushes, nothing before his go-ahead; Zek deploys by hand.
- **D5 Nav:** keep the current approach (each page has its own inline `<nav>`); **no shared component** (that
  belongs to the lower-priority ValueX refresh block, Block 3). Dropdown is added to each existing copy.
- **D6 Local testing:** local `hypha-web` (main), tmp and untracked, manual config by Ger (steps below). Fall back
  to a temporary prod entry only if the local DB lacks the Space.
- **D7 Community data:** logo, title and short description come from Hypha's public API
  (`GET https://app.hypha.earth/api/v1/spaces/<slug>`: `logoUrl`, `leadImage`, `title`, `description`), **referenced by URL,
  not copied** into the repo (Ger's choice). Held in one static list in value-x, not fetched at runtime
  (the site has no HTTP client or backend). Risk: if Hypha's image host changes, the image breaks; fallback is to copy the file into `wwwroot/images/`.
- **D8 Freeze rule:** option (a): freeze now, direction changes to `decisions.md`.

## Hypha DAO side (Ger's, outside the value-x PR)

Append to `WHITE_LABEL_EMBED_CONFIG` on the production Hypha app (Vercel; needs a redeploy). Client id pattern
`[a-z0-9][a-z0-9_-]{0,63}`. An invalid origin is dropped silently; if none survive, the Space entry is dropped and the frame is blank.

```json
{"id":"valuex","spaces":[{"slug":"ponta-do-sol-energy-community","framingOrigins":["https://value-x.com","https://www.value-x.com"]}]}
```

Facts: `hypha-integration-kit-context/tracks/white-label-embed-guide.md`, `hypha/white-label.md`.

## value-x change (Phase B, in `value-x.com`; follow its `AGENTS.md`)

1. **One community list** (slug, display name, logo URL, short blurb), used by the nav, the index and the page.
2. **Index** `@page "/platform"`: site nav and footer, a short intro, and a card per community (logo, name, blurb,
   link to `/platform/<slug>`). Aligned with the site's existing style (reuse `app.css` classes and the pattern of the
   Projects cards on Home; do not introduce a new look).
3. **Community page** `@page "/platform/{Slug}"`: site nav and footer, the landing note (below), the iframe, an
   "Open in Hypha" link to `https://app.hypha.earth/en/dho/<slug>/overview` (new tab). Unknown slug -> not found.
   Iframe: `src="{base}/embed/{slug}"`, `title="{name}"`, `width:100%`, tall (about 900px, adjust to viewport),
   `border:0`, **no `sandbox`**; test a narrow viewport.
4. **Embed base URL from configuration:** `Hypha:EmbedBaseUrl` in `appsettings.json` = `https://app.hypha.earth`.
   For local testing override with the environment variable `Hypha__EmbedBaseUrl=http://localhost:3000`; do not
   commit a localhost value.
5. **Navbar "Platforms" dropdown** on every existing page (Home, Denyse, SolarNow) and the new pages. The nav is
   custom CSS (`app.css`), no Bootstrap JS is loaded: use CSS (hover and `focus-within`) with a touch-friendly
   fallback; check mobile width. Home links `#section`, subpages `/#section`.
6. Smallest change in the existing pattern; do not refactor the nav; no commit or push without Ger's go-ahead.

### Landing note (copy)

A short block above the iframe (and the intro text on the index): what the visitor is looking at, and what to expect.
English, plain, no overpromising (brand terms: Energy Community, Orchestrator, Initiator, eParts, Energy Credits). Draft to start from; Ger edits in the PR:

> **Ponta do Sol Energy Community: live on the Hypha platform.** Explore this Energy Community as it runs: its
> agreements, members, treasury and energy data. This is a read-only view. To take part (sign in, vote, make a
> proposal), open it in the full Hypha app.

Blurb for the index card: take the first sentence of the Space's `description`.

## Verification

- Local: `dotnet watch --launch-profile http` with `Hypha__EmbedBaseUrl` set to the local hypha-web; `/platform` lists
  the community; its page renders the Space; no `frame-ancestors` console error; unknown slug is not found; the
  dropdown works on desktop and a narrow viewport.
- Framing from a non-approved origin is refused (expected).
- Prod (after the Hypha redeploy and Zek's deploy): anonymous load of `https://value-x.com/platform/ponta-do-sol-energy-community`
  and via `https://www.value-x.com/...`.
- Host facts: value-x answers from IIS (likely Azure App Service on Windows, **U**); `X-Frame-Options: SAMEORIGIN`
  on its own pages (irrelevant to framing others); no CSP header today.

## Local test of the Hypha side (Ger, manual, tmp, untracked)

See the session notes of 2026-11-14 / `decisions.md`; summary: in `hypha-web` (`main`) add to the git-ignored
`apps/web/.env`: `WHITE_LABEL_EMBED_CONFIG='[{"id":"valuex-local","spaces":[{"slug":"ponta-do-sol-energy-community","framingOrigins":["http://localhost:5249"]}]}]'`,
restart `pnpm dev:web`, check `http://localhost:3000/embed/ponta-do-sol-energy-community`.

## Open (does not block the freeze)

- [ ] Edgar's sign-off on the delivery and the denyse.org admin contact (then repeat the snippet there).
- [ ] Whether the local DB has the Space (decides D6 fallback).
- [ ] Final copy of the landing note (reviewed in the PR).

## Not in scope

Writes, login or signed-in embed (Hypha DAO #2635 / #2634); static hosting (`valuex-static-hosting`); a shared nav
component or any ValueX refresh (Block 3); DNS/subdomain delivery.
