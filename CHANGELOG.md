# Changelog

## 1.1.0 - 2026-10-31 (full review and security hardening)
Backend
- Fixed: bookings always failed validation (blank or local `datetime-local` date was rejected). The server now ignores blank form fields and the browser sends a proper ISO date.
- Fixed: the auth rate limit (30 per 15 min) also counted `/auth/me`, so normal use could lock people out. Limits are now per route: login 10 failed attempts / 15 min, register 10 / hour, verify 20 / 15 min, plus a general 300 / min API ceiling, uploads and bookings.
- Fixed: the provider dashboard required a legacy profile and returned 403 for every approved provider.
- Fixed: Stripe period dates were read from the wrong field on newer Stripe API versions, which could leave paid subscriptions "not bookable". Trial status is now mapped too.
- Fixed: rejecting a document no longer re-opens a suspended or rejected application; suspended/rejected providers lose the provider role.
- Added: duplicate-subscription guard, document type must match the service category, rejected documents must be re-uploaded before resubmitting, atomic booking status changes (no double clicks), provider can hide a service, admin can set document expiry and deactivate a plan.
- Security: strict Content-Security-Policy, JWT algorithm pinned, login timing equalised, 72-byte password limit (bcrypt), placeholder `JWT_SECRET` and `COOKIE_SECURE=false` refused in production, verification token wiped from the mail queue after sending, safe download filenames, unknown `/api` routes and missing files return real 404, invalid JSON returns 400, `TRUST_PROXY` support.
- `prisma` CLI moved to dependencies so `npm ci --omit=dev` + `db:deploy` works on a server.
Frontend
- Fixed: admin approve/reject buttons crashed (wrong element lookup).
- Fixed: reloading on `#dash` bounced logged-in people to the login page; after login the header crashed.
- Fixed: friendly messages for server errors, rate limits and offline; expired sessions return to login.
- Added: email-verification result message, provider "Decline" button and hide/publish toggle.
- Security: rich-text notes are sanitised, `esc()` also escapes `'`, inline `onerror` removed, community text no longer claims posts are public (demo is device-only).
Tooling
- `npm run web:static` is now a Node script (works on Windows). Smoke test checks CSP compatibility.

## 1.0.1 - 2026-10-30
- Split the 489 KB `app.js` into 14 modules under `js/`; the embedded 245 KB base64 hero image is now `assets/hero.jpg`.
- Fixed the Photo map page showing the heading "Travel passport" instead of "Photo map".
- Fixed the header wrapping onto two rows on desktop, hero text that was unreadable over the illustration, and sideways scrolling on phones caused by the file input.
- Home search placeholder is now translated in Portuguese.
- Added fake client-side demo logins (shown on the Log in page) so the demo can be reviewed.
- Added `scripts/smoke-test.js`, `package.json` (no dependencies), real EN/PT screenshots, nginx example and a handoff checklist.
- Removed the temporary `SUBMISSION-STATUS.md` and normalised line endings.

## 1.0.0 - 2026-10-30
- Packaged the AmarPortugal static travel and local-services prototype.
- Separated the HTML entry point, CSS and JavaScript into the Vibe96 folder structure.
- Added setup, product scope, AI coding and licence notes.
- Documented five existing browser-side features and prototype limitations.
