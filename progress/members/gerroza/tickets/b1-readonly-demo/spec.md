# b1 — Read-only demo of the pilot communities (`/platform`)

> **Status:** Phase A (spec) draft; Phase B partly started (value-x page). Not frozen: the Commitment Gate
> is Edgar's sign-off on Block 1 wording plus the agreement itself (see `ger-hypha-energy`).
> **Last updated:** 2026-11-13. **SDD level:** 2 (standard).

## Goal

Partners, funders and reviewers can walk through the Hypha Energy platform with realistic data without
being able to change anything, from a sub path (`/platform`) of the DENYSE and ValueX sites, each
embedding the Hypha platform through one iframe snippet.

## Scope (from the agreement draft)

1. Test, pilot and synthetic Energy Communities as a read-only view, on the live white-label embed.
2. `/platform` on both sites, one snippet each.
3. Public sections browsable; account-bound actions open the full Hypha app in a new tab. Nothing is written.
4. A Space already public in production; a made-up Space only if synthetic data is needed.

Acceptance: both sub paths load for an anonymous visitor, no write possible, Edgar signs off. Deadline: this week.

## Known facts

- Embed delivery is Hypha DAO's (hypha-web #2616 analysis, #2625 / PR #2632 live 2026-11-05, #2650 allowlist).
  Facts and partner guide: `C:\Users\rozag\projects\hypha\src\hypha-integration-kit-context\tracks\white-label.md`
  and `white-label-embed-guide.md`. The embed serves 12 public sections; proposal detail, voters, person
  pages and create flows are not served.
- DENYSE origin is already allowed in the embed config. **The ValueX origin must be added by hand** on the
  Hypha DAO side (env config; Ger's DAO work).
- ValueX site = `src/value-x.com` (.NET 9 Blazor, static SSR, no tests). Pages live in `Components/Pages/`;
  `Routes.razor` has no default layout, so each page renders its own `<nav>` (copy it from `Denyse.razor`).
  No interactive render mode: the iframe snippet is plain HTML, which is fine.
- Deploy of value-x is **manual by Zek**: prod expected weekend or Monday after the page is merged/handed over.
- denyse.org is not managed by Zek; Edgar to provide the admin contact (name uncertain). Not in the org.

## Open

- [ ] Which Space(s) to show, and their public URLs / embed parameters.
- [ ] Hand-over path for the value-x change (PR to `Hypha-Energy/value-x.com` vs. patch to Zek). Ger's call; no push without go-ahead.
- [ ] ValueX origin added to the embed config (DAO side).
- [ ] denyse.org admin contact (Edgar).
- [ ] Landing note text on each sub path.

## Plan (Phase B)

1. Read the embed guide; take the exact snippet and allowed-origin rules.
2. Add `Components/Pages/Platform.razor` (`@page "/platform"`) to `value-x.com` with the site nav, a short
   landing note and the iframe; run locally (`dotnet run --launch-profile http`).
3. Verify: anonymous load, no write possible, frames from the allowed origin only.
4. Hand over to Zek for manual deploy; repeat the snippet on denyse.org once the admin is known.
