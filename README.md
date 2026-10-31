# AmarPortugal

AmarPortugal is a bilingual (English / Portuguese) travel and local-services project. This delivery preserves the browser prototype and adds an Express/PostgreSQL/Prisma marketplace foundation for shared accounts, provider verification, subscriptions, customer service requests, and unified history.

> **Current scope:** customer accounts, provider applications/documents, admin document review, approved-plan checkout, provider listings, service booking requests, private booking photos, and completion receipts have backend and dashboard flows. Food ordering, hotel room inventory, TVDE dispatch/location, service payments, provider payouts, and statutory Portuguese fiscal invoicing still need separate implementation/integration. Do not use the legacy food demo for real orders.

**Live demo:** `https://YOUR-DEMO-URL-HERE` (replace after deploying, see [HowToSetup.md](HowToSetup.md))

## Who it is for

- **Visitors** planning a trip: explore regions, build a day-by-day plan, make a photo map and a travel passport.
- **Service providers** (guides, photographers, hotels, restaurants, couriers, TVDE drivers) exploring a future marketplace: demo sign-up, profile and provider dashboard.

## Existing browser features

Each feature has real JavaScript behind it (see the file named in brackets).

1. **EN / PT language toggle** — switches every interface string instantly (`js/i18n.js`, `js/router.js`).
2. **Portugal region explorer** — 18 districts + 2 autonomous regions with sights, food and articles (`js/explore.js`, `js/data.js`).
3. **Day-by-day trip planner** — add days, pick regions and places, or use the built-in local demo guide to suggest a route (`js/planner.js`).
4. **Custom photo map** — mark visited regions, add browser-local photos, pick a theme, download a PNG (`js/map-engine.js`, `js/passport.js`).
5. **Illustrated travel passport** — create a personal passport design and download it (`js/passport.js`).

Hotels, guides, food and TVDE pages are search/booking **UI shells**. They list no businesses on purpose: public listings should only come from reviewed providers, so no fake businesses are shown.

## Backend foundation

- Express REST API serves the static site and `/api/*` endpoints.
- Prisma/PostgreSQL schema defines users, multiple roles, provider verification status, cross-service activity history, notifications, and audit logs.
- Passwords are hashed with bcrypt; sessions use signed HttpOnly cookies.
- Customer activity reads are scoped to the authenticated customer; provider activity reads are scoped to the authenticated provider.
- Provider signup is only an application. It does not represent an approval or legal authorization.
- Provider documents and customer reference photos are stored privately, outside the public website directory.
- Service publication requires approved, unexpired required documents and an active subscription. Prices are configured by admins; no unprovided prices are invented.
- Customer booking requests capture name, verified email, phone, contact address, optional date/note/photo, and a server-side price snapshot.
- Completion queues a bilingual PDF platform receipt email. The document says it is not an AT-certified Portuguese fiscal invoice.

See [HowToSetup.md](HowToSetup.md) for Windows setup and [docs/IMPLEMENTATION-PLAN.md](docs/IMPLEMENTATION-PLAN.md) for the inspected architecture, delivery sequence, and remaining work.

## Screenshots

| English home | Portuguese home |
|---|---|
| ![English home](docs/screenshots/01-home-en.png) | ![Portuguese home](docs/screenshots/02-home-pt.png) |

| Explore (PT) | Trip planner (EN) | Photo map (EN) |
|---|---|---|
| ![Explore in Portuguese](docs/screenshots/03-explore-pt.png) | ![Trip planner](docs/screenshots/04-trip-planner-en.png) | ![Photo map](docs/screenshots/05-photo-map-en.png) |

## Legacy demo logins (fake, client-side only)

| Email | Password | Role |
|---|---|---|
| `demo.traveller@amarportugal.test` | `Demo-1234` | Traveller |
| `demo.guide@amarportugal.test` | `Demo-1234` | Local guide |

These accounts are created inside your own browser storage. They are not real accounts and do not authenticate against the new API. Do not enter real passwords, identity details, or bank data in the prototype screens.

## Dependencies

The static site uses plain HTML5, CSS3 and vanilla JavaScript. The API uses Node.js, Express, PostgreSQL, Prisma, and supporting security/validation packages listed in `package.json`.

## Run and check it

See [HowToSetup.md](HowToSetup.md). Quick start:

```bash
cd "Web Prototype" && python3 -m http.server 8000   # then open http://127.0.0.1:8000/
node scripts/smoke-test.js                           # repeatable checks, no install needed
```

## Data and privacy scope

- State lives in the visitor's browser only. Clearing site data resets the demo.
- The Explore page may request destination photos from Wikimedia Commons at runtime, and some links open public Portuguese services or Google Maps. Those third parties see the request; nothing else leaves the browser.
- Photos added to the photo map stay on the device.

## Licence note

See [LICENSE-NOTE.md](LICENSE-NOTE.md). Confirm your rights to the source code and to the hero illustration before any commercial sale.
