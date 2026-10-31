# AmarPortugal setup (Windows)

This project preserves the bilingual website and adds backend flows for customer accounts/email verification, provider applications/private documents, admin review, configurable subscriptions, service listings, customer booking requests with contact information and a reference photo, and completion receipts. Food ordering/dispatch, hotel room inventory, live TVDE, service payments/payouts, and AT-certified fiscal invoices still need separate implementation/integrations. The legacy food demo remains browser-local; do not use it for real orders.

## Requirements

- Node.js 20.19+ and npm
- PostgreSQL 15+
- PowerShell or the VS Code terminal

## Start PostgreSQL

Create a PostgreSQL database and user using your chosen PostgreSQL installer/admin tool. This app expects a database named `amarportugal` by default. Keep its password private.

## Configure and start the API

From the project folder (the one containing `package.json`):

```powershell
Copy-Item .env.example .env
notepad .env
npm install
npx prisma generate
npx prisma migrate deploy
npm start
```

Edit `.env` and set `DATABASE_URL`, a random `JWT_SECRET` with at least 32 characters, `WEB_ORIGIN=http://localhost:3000`, and `COOKIE_SECURE=false` for local HTTP development. The production deployment must use HTTPS and `COOKIE_SECURE=true`. Never commit `.env` or expose its values in browser code.

The included migration is for a **new empty database**. If you already have database tables or records, baseline them with a reviewed migration; do not apply this initial migration over them.

Open `http://localhost:3000`. The health endpoint is `http://localhost:3000/api/health`.

## Create the first administrator

Set `ADMIN_EMAIL` and a unique `ADMIN_PASSWORD` (16+ characters) in `.env`, then run:

```powershell
npm run db:seed
```

The admin dashboard uses server-side role checks. Seeded super-admin credentials are not exposed by the app.

## API foundation

All endpoints use JSON. Authentication is a seven-day signed session cookie marked HttpOnly and SameSite=Lax. Browser requests must use `credentials: 'include'`.

- `POST /api/auth/register` — name, verified contact details, address, email, and password (10–128 chars); creates a CUSTOMER and queues an email verification link.
- `POST /api/auth/verify-email` and `/api/auth/verify-email/resend` — verify/resend the 24-hour single-use link.
- `POST /api/auth/login` — verifies password and starts a session.
- `POST /api/auth/logout` — clears the session.
- `GET /api/auth/me` — current account and roles.
- `PATCH /api/auth/me` — update name, phone, country, city, contact address, postal code, or locale.
- `GET /api/providers/requirements` — category-specific required document types.
- `POST /api/providers/applications` — save a category-specific provider application draft.
- `POST /api/providers/applications/:id/documents` and `/submit` — upload private documents and submit for review.
- `GET /api/providers/me` — applications, document review, and subscription state.
- `GET /api/admin/provider-applications`, `PATCH /api/admin/documents/:id`, and `PATCH /api/admin/provider-applications/:id` — admin review, decisions, and audit logs.
- `POST /api/admin/subscription-plans` — admin-configure category/vehicle-specific EUR prices. No price schedule was included in the supplied spec.
- `GET /api/providers/plans?applicationId=...` and `POST /api/providers/subscriptions/checkout` — approval-gated Stripe subscription checkout.
- `POST /api/webhooks/stripe` — signed subscription payment/renewal synchronization.
- `GET /api/marketplace/services` — only services whose provider documents are approved/current and subscription active.
- `POST /api/marketplace/provider/services` — provider service listing after approval and subscription.
- `POST /api/marketplace/services/:id/bookings` — verified customer name, email, phone, address, optional date/message/photo, server-side price snapshot.
- `GET /api/marketplace/bookings/mine`, `/api/marketplace/provider/bookings`, and `PATCH /api/marketplace/provider/bookings/:id/status` — owner-scoped requests and lifecycle.
- `GET /api/invoices/mine` and `/api/invoices/:id/receipt.pdf` — private receipt history/download.
- `GET /api/private-files/:id` — access-controlled private document/photo download.
- `GET /api/dashboard/customer` — own unified activities and notifications.
- `GET /api/dashboard/provider` — activities assigned to the current provider only.
- `GET /api/health` — server/database check.

Money is stored as integer euro cents and each booking preserves its listed-price snapshot. Stripe webhook confirmation is required to activate subscriptions; service booking payments and provider payouts are not implemented. SMTP emails retry through an outbox; without SMTP credentials they remain queued. Completion PDFs are platform receipts, **not AT-certified Portuguese tax invoices**. Configure an appropriate invoicing provider and obtain tax/legal review before relying on them for fiscal reporting.

## Keep using the static prototype

The original website can still run without installing packages:

```powershell
cd "Web Prototype"
py -m http.server 8000
```

Then open `http://127.0.0.1:8000`. Its existing account forms are still demo/localStorage behavior and are not connected to the API in this phase.

## Validation

```powershell
npm test
```

The automated tests cover static files and pure document/subscription/booking rules. End-to-end workflows still require PostgreSQL, `npm install`, Stripe test keys/webhook secret, SMTP credentials, and browser verification. They have not been run in this environment.

## Legal and launch review

Before operating, obtain professional review for GDPR notices, retention/deletion/export, consent, TVDE and other service-provider licensing, consumer cancellation/refund terms, tax, and payment-provider merchant eligibility. This code makes no legal-compliance or regulatory-authorization claim.
