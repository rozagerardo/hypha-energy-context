# Decisions — b1-readonly-demo

> The spec is frozen (2026-11-14). Changes to it are recorded here and override the spec where they differ.

## D9 — No landing note (2026-11-14, Ger)

Supersedes the spec's "Landing note (copy)" section, the "short intro" on the index, and the matching Open item.
- **Community page:** site nav and footer, the iframe, and the "Open in Hypha" link. No explanatory text block above the frame.
- **Index `/platform`:** heading plus one card per community (logo, name, blurb). No intro paragraph.
- Still "properly presented, aligned with the site's style" (Ger's earlier requirement). A page title/heading is not a landing note.
- **Why:** Ger does not want copy on the pages for now. **How to apply:** do not write or propose landing copy; the
  card blurb (first sentence of the Space description) stays, as it is data, not a note.

## D10 — Local test runs in the Hypha Energy sandbox clone, with `cordoba-ecovillage` (2026-11-14, Ger)

Supersedes the spec's D6 wording and its "Local test of the Hypha side" section.
- **Clone:** `C:\Users\rozag\projects\hypha\src\hypha-web--hypha-energy` (`main`, read-only: pull only, commits and
  pushes denied). Not the six DAO clones. Test config is local and uncommitted (`apps/web/.env`, git-ignored).
- **Space for the local test:** `cordoba-ecovillage`, not Ponta do Sol. It is **not in prod** (the public API returns
  `null`, and `/embed/cordoba-ecovillage` on prod is a 404), so it must exist in whichever database that clone's
  `.env` points to; verify in the browser before relying on it (and that it is public).
- **Local env (set by Ger, line 194 of `apps/web/.env`, checked 2026-11-14):**
  `WHITE_LABEL_EMBED_CONFIG='[{"id":"valuex-local","spaces":[{"slug":"cordoba-ecovillage","framingOrigins":["http://localhost:5249"]}]}]'`
  with `NEXT_PUBLIC_APP_URL=http://localhost:3000`.
- **value-x side of the local test:** `Hypha__EmbedBaseUrl=http://localhost:3000`, and a **temporary, uncommitted** extra
  entry for `cordoba-ecovillage` in the community list (slug, name, any logo), because an unknown slug is a 404. Remove it
  before any commit. The committed list contains only the real prod community (`ponta-do-sol-energy-community`).
- Setup before first run: `pnpm install` (running), `pnpm env:copy` was not needed if `.env` already exists.

## D11 — The prod `WHITE_LABEL_EMBED_CONFIG` change is a Hypha DAO-side change (2026-11-14, from the context repo)

The sandbox clone cannot commit or push, so the production entry (`valuex` client, origins `https://value-x.com` and
`https://www.value-x.com`) is raised by Ger through the normal Hypha DAO flow from a regular clone. The env var lives in
Vercel (not in git), so the real action is a Vercel env edit plus redeploy. This matches the spec ("Hypha DAO side,
outside the value-x PR"); no conflict. Prod and local configs differ by design: prod has no localhost and uses the
Ponta do Sol slug.

## D13 — Phase B implementation choices (2026-11-14, value-x, branch `rozagerardo/feat/platform-pages`, uncommitted)

Within the spec's "smallest change in the existing pattern"; nothing here changes direction.
- **List:** `Components/PlatformCommunities.cs` (static record list + `Find`). Real entry: `ponta-do-sol-energy-community`
  (title, `logoUrl` and first sentence of `description` taken from the raw API response). Used by nav, index and page.
- **Pages:** `Components/Pages/PlatformIndex.razor` (`/platform`), `CommunityPage.razor` (`/platform/{Slug}`). Named so they do not
  clash with the `PlatformCommunity` record. Unknown slug: "Community not found" + link, HTTP 404 (cascading `HttpContext`).
- **Iframe:** `{PlatformCommunities.EmbedBaseUrl}/embed/{slug}`, no `sandbox`, 100% wide, `min(900px, 85vh)` tall; "Open in Hypha" new tab.
  Per Ger's D12 (written after this entry) the base URL is a hardcoded constant next to the list; no `appsettings`, no env var. Superseded the config approach built first (removed).
- **Dropdown:** CSS only (hover + `focus-within`), added to the existing inline nav of Home, Denyse, SolarNow and both new pages. The
  trigger is a link to `/platform`, so touch devices without hover still reach the index.
- **Index card image:** uses `logoUrl` (contain, padded), not `leadImage`.
- **Data caveat:** a summarised web fetch returned a wrong `logoUrl`; the committed value was taken from the raw JSON (curl) and both
  image URLs answer 200.
- **TEMP (uncommitted, marked `TEMP-LOCAL-TEST`):** the `cordoba-ecovillage` list entry and `EmbedBaseUrl = http://localhost:3000`. Restore the prod URL and remove the entry before any commit.

## Open from D13 (Ger decides)
- **Mobile nav:** existing CSS hides `.nav-menu` entirely below 768px, so on phones no nav (and no Platforms dropdown) exists today.
  Options: (a) leave as is (smallest change; mobile visitors reach `/platform` only by direct link); (b) add a small
  mobile-only rule showing the menu or a Platforms link (touches shared CSS, goes beyond the spec's "no refactor" only mildly);
  (c) leave to Block 3 (ValueX refresh).

## D12 — Hardcode the embed base URL; no configuration, no env vars (2026-11-14, Ger)

Supersedes spec item 4 ("Embed base URL from configuration", `Hypha:EmbedBaseUrl`, the `Hypha__EmbedBaseUrl` override).
- The base URL `https://app.hypha.earth` is a **hardcoded constant** in value-x (one place, next to the community list).
  Nothing is added to `appsettings.json` and **no environment variable is needed in prod or anywhere else**, so the
  manual deploy by Zek needs no setup beyond shipping the build.
- **Why:** Ger has no access to the deploy setup, and the whole site gets cleaned up in a later block (Block 3).
- **Local test:** the only reason a different host was needed. For local testing only, **temporarily edit that constant to
  `http://localhost:3000` (and add the temporary `cordoba-ecovillage` list entry), uncommitted**, and restore both before any
  commit. Never commit the localhost value.

## D14 — Platforms as a Home section, single nav link, iframe-only page (2026-11-14, Ger, after reviewing the build)

Supersedes spec D1 (dropdown + `/platform` index) and spec item 5; also resolves the D13 mobile-nav question.
- **Home:** new `#platforms` section (same grid/cards as Projects, shorter logo area, heading only) listing the communities; each card links to `/platform/<slug>`.
- **Nav:** a single "Platforms" link to the section (`#platforms` on Home, `/#platforms` elsewhere), same as Projects. No dropdown, no sub-menus. The `/platform` index page and the dropdown CSS were removed (`/platform` is now a 404).
- **Community page:** site nav plus the iframe only (full width, viewport height under the fixed nav). No title, no "Open in Hypha" button, no footer.
- **Why:** keep the site's single-page pattern; mobile has no nav today (hidden below 768px) and Projects behaves the same, so Platforms is consistent with it.

## D15 — Hypha-side config applied; value-x PR merged; awaiting deploy (2026-11-14, Ger)

- **Hypha prod config done (Ger, Vercel, production app, redeployed).** `WHITE_LABEL_EMBED_CONFIG` now also has the client
  `{"id":"valuex","spaces":[{"slug":"ponta-do-sol-energy-community","framingOrigins":["https://value-x.com","https://www.value-x.com"]}]}`
  next to the existing clients (`denyse` unchanged; Ger's own `gerroza-test` client is also present, not part of Block 1). No localhost in prod.
- **Verified (Ger, 2026-11-14):** `curl -sI https://app.hypha.earth/embed/ponta-do-sol-energy-community` returns
  `frame-ancestors https://denyse.org https://value-x.com https://www.value-x.com`.
- **www is a separate origin:** `https://www.value-x.com/` answers 200 (not a redirect), so both origins are listed.
- **value-x PR #4 merged to `master`** (merge commit `ad338a7`, 2026-11-14 20:19 UTC) by Ger. Merging deploys nothing; **Zek was asked to deploy by hand**
  (`dotnet publish -c Release`, .NET 9 SDK). Not live yet.
- **Still pending:** the deploy; the anonymous prod check on both hosts; Edgar's sign-off (wording; path is `/platform/<slug>`, there is no `/platform` index, Ger will say so at the demo);
  denyse.org (admin contact from Edgar; the `denyse` client already exists); mobile nav (leave to Block 3, per D13 option c unless Ger says otherwise).
- **Pattern-card candidates (not drafted):** (1) verify "it redirects" style claims against the live system before configuring allowlists (the `www`
  origin was assumed redirected and was not); (2) a read-only sandbox clone of the platform repo for partner-integration tests, with test config kept
  local and the production change made through the owner's normal flow.
