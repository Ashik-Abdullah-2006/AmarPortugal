# AmarPortugal: guide for AI coding tools

Static EN/PT travel prototype. Vanilla HTML/CSS/JS, no packages, no build step, no backend.

## Layout
- `Web Prototype/index.html`: page shell; loads CSS and 14 plain scripts **in a fixed order** (they share one global scope, so keep the order).
- `Web Prototype/css/app.css`: all styles; brand colours are CSS variables in `:root`. Fixes added in v1.0.1 are at the end.
- `Web Prototype/assets/hero.jpg`: hero illustration.
- `Web Prototype/js/`: `data.js` (regions, services `S`, roles) · `planner.js` · `i18n.js` (**translations `T`**, global state) · `shell.js` (helpers, header/footer, home) · `community.js` · `search.js` · `map-engine.js` · `explore.js` · `passport.js` · `hotels.js` · `guides.js` · `food.js` · `router.js` (hash router `render()`, language switch) · `auth.js` (demo accounts, dashboard).
- `scripts/smoke-test.js`: repeatable checks (`npm test`).

## Translations
`T.en` / `T.pt` in `js/i18n.js`, read with `t("key")`. Inline strings use `L("English","Português")`. Keep both languages in sync.

## Demo state (all in the browser)
localStorage keys: `ap_trip` (planner), `ap_accounts_v1` and `ap_session_v1` (demo accounts, passwords hashed with a non-secure demo hash), `ap_community_v1`, `ap_pass`, `ap_photo`, `ap_up*` (photos), `ap_dl`, `ap_fd_demo` (food demo). Clearing site data resets everything. Demo logins are seeded in `js/auth.js` (`DEMO_ACCOUNTS`).

## Conventions
- Routes are hash routes (`#plan`, `#hotels`, ...) handled in `router.js` and `auth.js`.
- Do not add API keys or secrets; client code is public. Blank `*_API` / `*_ENDPOINT` variables are placeholders.
- Do not add fake businesses to hotels/guides/food; public listings are meant to come from reviewed providers.
- No claims of real bookings, payments or server-side security in UI text.
- Avoid duplicate global function names across files (they share one scope).

## Check manually
`cd "Web Prototype" && python3 -m http.server 8000`, then: switch EN/PT, open every nav item, add a planner day, upload a photo in Photo map and download the PNG, log in with a demo account. Then run `node scripts/smoke-test.js`.
