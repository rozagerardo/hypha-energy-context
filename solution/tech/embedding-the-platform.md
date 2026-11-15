# Embedding a Hypha Space on a partner site (read-only iframe)

> How Hypha Energy shows an Energy Community from the Hypha platform on a partner website, and the two cases where we did it.
> The embed itself is Hypha DAO's (hypha-web white-label delivery, live in production since 2026-11-05). Developer guide and
> facts live in the Hypha DAO integration-kit context (`hypha-integration-kit-context`, `tracks/white-label-embed-guide.md`,
> `hypha/white-label.md`); this page records how **we** use it. Verified against production 2026-11-15.

## What a visitor gets

A public Space shown inside the partner's own page, read-only: overview, agreements, members, treasury, banking, calendar,
coherence, ecosystem navigation, energy, memory, pipeline, rewards. No login, no writing. Actions that need an account do
nothing in the embed; links to things the embed does not serve (proposal detail, person pages, create flows) open the full app.

## The snippet

```html
<iframe
  src="https://app.hypha.earth/embed/<space-slug>"
  title="<Space name>"
  style="width:100%;height:900px;border:0">
</iframe>
```

Variants: a section `/embed/<slug>/<section>` (for example `/treasury`); a language `/embed/<slug>/<locale>[/<section>]`
(for example `/pt/overview`; the path is the only reliable way to pin a language inside a cross-site frame).
No `sandbox` attribute (if one is ever added it must include `allow-scripts allow-same-origin`, and `allow-popups
allow-popups-to-escape-sandbox`). The height is fixed by the page (no auto-resize yet); keep the width at 100%.

## What must be true for it to render

1. **The Space is public** and its slug is approved for a client.
2. **The embedding origin is on the Space's list**, exactly: scheme + host, no path, no wildcard; `https://x.org`, `https://www.x.org`
   and `http://localhost:<port>` are different origins. The visitor's browser sees the final origin after redirects, so a host
   that redirects to another needs only the target (check with `curl`; do not assume).
3. **Configured on the Hypha side** in the production env var `WHITE_LABEL_EMBED_CONFIG` (Vercel; a JSON array of clients; edit,
   then redeploy). Several clients may list the same slug; their origins are merged. Entry shape:
   `{"id":"<client>","spaces":[{"slug":"<space-slug>","framingOrigins":["https://<origin>"]}]}`. A malformed entry is dropped
   silently (a blank frame, no error), and a bad whole value switches the feature off.
4. The partner page is served over HTTPS and its own CSP (if any) allows `frame-src https://app.hypha.earth`.

Check from the outside (no browser needed):
`curl -sI https://app.hypha.earth/embed/<slug> | grep -i -o "frame-ancestors[^;]*"` lists the allowed origins; a plain 404 means
the slug is not approved, not public, or the config did not parse.

## Cases

| Partner site | Space | Origin(s) | Delivered as | State |
|---|---|---|---|---|
| **value-x.com** | `ponta-do-sol-energy-community` | `https://value-x.com`, `https://www.value-x.com` (www is served directly, no redirect) | Blazor page `/platform/<slug>` in the site: iframe only, full width, viewport height under the fixed nav (CSS class instead of the inline style); a "Platforms" section on Home links to it. Hardcoded base URL and a one-entry community list, no config. Client `valuex` | **Live 2026-11-15** (Zekeriya confirmed `https://www.value-x.com/platform/ponta-do-sol-energy-community`) |
| **denyse.org** | `ponta-do-sol-energy-community` | `https://denyse.org` (`www.` and `http` redirect with 301 to it) | The snippet above, pasted as-is by the site's admin into `denyse.org/pontodelsol` (Edgar emailed it to the admin on 2026-11-15). Client `denyse`, already enabled | Page not created yet (404); waiting on the admin |

Notes from doing it:
- Same Space on two partners = two clients sharing a slug; no cross-effect beyond the union of allowed origins.
- Testing before prod: a read-only sandbox clone of hypha-web with a local-only `WHITE_LABEL_EMBED_CONFIG` entry for
  `http://localhost:<port>` and any Space present in that clone's database; the production entry is a separate, owner-side change.
- Sending the snippet by email or chat: paste it as plain text or a code block; rich-text clients can turn the straight quotes into
  typographic ones, which breaks the HTML.
- Mobile: the iframe works at phone width; the partner's own nav is the partner's concern (value-x hides its menu below 768px).
