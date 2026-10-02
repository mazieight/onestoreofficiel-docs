<div align="center">

# **One Store**

### Full-stack e-commerce & backoffice platform

*Modern storefront · Admin dashboard · Product analytics · Ads platform · Multi-currency & i18n*

<br />

[![Next.js](https://img.shields.io/badge/Next.js-16-black?style=flat-square&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![Prisma](https://img.shields.io/badge/Prisma-5-2D3748?style=flat-square&logo=prisma)](https://www.prisma.io/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=flat-square&logo=mongodb)](https://www.mongodb.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3-38B2AC?style=flat-square&logo=tailwind-css)](https://tailwindcss.com/)
[![Stripe](https://img.shields.io/badge/Stripe-Payments-6772E5?style=flat-square&logo=stripe)](https://stripe.com/)
[![PayPal](https://img.shields.io/badge/PayPal-Checkout-00457C?style=flat-square&logo=paypal)](https://www.paypal.com/)
[![next-intl](https://img.shields.io/badge/next--intl-i18n_%7C_RTL-000?style=flat-square)](https://next-intl-docs.vercel.app/)

<br />

### Banner

![One Store — Storefront & Admin](docs/screenshots/banner.png)

<br />

---

## Quick Links

| | |
|:---|:---|
| **[Highlights](#-highlights)** | At-a-glance capabilities |
| **[Feature cards](#-feature-cards)** | Storefront, Admin, Payments, i18n, more |
| **[Screenshots](#-screenshots)** | Storefront, dashboard, analytics, mobile |
| **[Demo](#-demo)** | End-to-end walkthrough GIF |
| **[Tech stack](#-tech-stack)** | Frontend, backend, DB, auth, payments |
| **[Architecture](#-architecture)** | Routing, data layer, Prisma, uploads |
| **[Getting started](#-getting-started)** | Install, env, DB, run |
| **[Roadmap](#-roadmap)** | Planned improvements |

---

</div>

## Table of Contents

- [Highlights](#-highlights)
- [Feature Cards](#-feature-cards)
- [Overview](#-overview)
- [Screenshots](#-screenshots)
- [Demo](#-demo)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Core Workflows](#-core-workflows)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Scripts](#-scripts)
- [Deployment](#-deployment)
- [Internationalization](#-internationalization)
- [Analytics](#-analytics)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)
- [Author & Credits](#-author--credits)

---

## Highlights

| | |
|:---:|:---|
| **Storefront** | Locale-aware home, categories, brands, product detail (with variants), cart, checkout (Stripe & PayPal), favorites, account, order history. |
| **Admin** | Separate backoffice auth (username/password), RBAC with permission keys, dashboard KPIs, product-level analytics (revenue, cost, profit, margin, low stock), orders, customers, taxonomy (categories/brands), POS (barcode scan), ads management, settings, audit logs. |
| **Payments** | Stripe (Payment Intents + webhooks) and PayPal (order/capture + webhooks); optional COD. Order confirmation & transactional emails via Resend. |
| **i18n** | English, French, Arabic with **RTL** support (`dir="rtl"`, Tajawal font). next-intl with JSON message namespaces. |
| **Media** | Server-side upload API (Sharp: resize, WebP, EXIF); images & videos; `/uploads/[...path]` route for serving files. |
| **Analytics** | Dashboard period filters (day/week/month/year), product-level revenue/cost/profit from PAID orders, stock & low-stock views, export (CSV, Excel, PDF, JSON, PNG) from Overview. |
| **Ads** | Campaigns, ad creatives (image/video/cinematic/carousel), placements, targeting, frequency caps, impressions/clicks/revenue tracking, events, settings. |

---

## Feature Cards

<table>
<tr>
<td width="50%" valign="top">

**Storefront**

- Hero, category & brand carousels, product showcases (new, deals, promo)
- Product listing with filters, PDP with variants (e.g. TV sizes), gallery, add to cart, favorites
- Cart sheet, checkout (Stripe / PayPal), success page, order confirmation emails
- Account: profile, orders, favorites; contact, help, support, policies (shipping, returns, warranty, privacy, terms)

</td>
<td width="50%" valign="top">

**Admin dashboard**

- KPI grid: revenue, profit estimate, order counts (pending/paid/shipped/delivered/failed/cancelled/refunded), AOV, low stock
- Period selector (day/week/month/year), trend comparison, recent sales, charts
- Export: CSV, Excel, PDF, JSON, PNG from Overview
- Product analytics: revenue/profit/margin/stock/low-stock by product & variant; exact vs estimated profit

</td>
</tr>
<tr>
<td width="50%" valign="top">

**Products & inventory**

- Products: CRUD, SKU/barcode, slug, multi-locale title/description, price/compareAtPrice/cost, stock, status, badges, specs, category & brand
- Variants: per-variant price, stock, SKU, barcode, cost, low-stock threshold, variant media (image/video)
- Taxonomy: categories and brands with localized names

</td>
<td width="50%" valign="top">

**Orders & fulfillment**

- Order list with status/payment filters, detail view, status updates
- Payment provider (Stripe/PayPal/COD), shipping address, loyalty points on payment
- Order emails (Resend, language-aware); token-based confirmation for guests

</td>
</tr>
<tr>
<td width="50%" valign="top">

**Payments & integrations**

- Stripe Payment Intents + webhooks; PayPal order/capture + webhooks
- Resend for transactional email; optional Twilio SMS
- Dual auth: User (storefront) and Admin (backoffice) with RBAC and audit logs

</td>
<td width="50%" valign="top">

**Internationalization**

- Locales: **en**, **fr**, **ar** (default en)
- RTL for Arabic: `dir="rtl"`, Tajawal font; messages in `messages/{en,fr,ar}.json`
- Product/category/brand names in DB as JSON `{ en, fr, ar }`

</td>
</tr>
</table>

---

## Overview

**One Store** is a full-stack e-commerce and backoffice platform built for serious product operations. It combines a customer-facing storefront with a permission-driven admin dashboard, product-level profitability analytics, and an integrated ads system—all within a single Next.js application.

- **Who it's for:** Teams running a product catalog (e.g. tech/electronics), needing a unified storefront + admin, multi-language/RTL support, and clear visibility into revenue, cost, and profit per product/variant.
- **What it includes:** Locale-routed storefront (home, categories, products, cart, checkout, account), separate admin app (dashboard, orders, products, customers, categories, brands, ads, POS, settings), dual auth (customers vs admins), Prisma + MongoDB, Stripe & PayPal, Resend emails, optional Twilio SMS, and next-intl for en/fr/ar with RTL.
- **Why it matters:** Single codebase, server actions + API routes, RBAC, audit logging, and product analytics (exact profit where cost is set, estimated otherwise) make it suitable for scaling both sales and operations.

---

## Screenshots

*Add images at the paths below; they will render automatically. Recommended size: 800×500 px for consistency.*

### Screenshots grid

| Storefront | Admin |
|:----------:|:-----:|
| **[docs/screenshots/storefront-home.png](docs/screenshots/storefront-home.png)**<br/>*Home: hero, category carousel, product showcases* | **[docs/screenshots/admin-dashboard.png](docs/screenshots/admin-dashboard.png)**<br/>*Dashboard: KPI grid, period selector, charts, export* |
| ![Storefront Home](docs/screenshots/storefront-home.png) | ![Admin Dashboard](docs/screenshots/admin-dashboard.png) |
| **[docs/screenshots/product-detail.png](docs/screenshots/product-detail.png)**<br/>*PDP: variants, gallery, add to cart, favorites* | **[docs/screenshots/admin-analytics-products.png](docs/screenshots/admin-analytics-products.png)**<br/>*Product analytics: revenue, profit, stock, low stock* |
| ![Product Detail](docs/screenshots/product-detail.png) | ![Product Analytics](docs/screenshots/admin-analytics-products.png) |
| **[docs/screenshots/checkout.png](docs/screenshots/checkout.png)**<br/>*Checkout: order summary, Stripe/PayPal* | **[docs/screenshots/admin-products.png](docs/screenshots/admin-products.png)**<br/>*Product management: list, filters, create/edit* |
| ![Checkout](docs/screenshots/checkout.png) | ![Admin Products](docs/screenshots/admin-products.png) |

### Mobile

| **[docs/screenshots/admin-mobile.png](docs/screenshots/admin-mobile.png)** |
|:-----------------------------------------------------------------------:|
| *Admin on mobile: collapsible sidebar, responsive tables* |
| ![Admin Mobile](docs/screenshots/admin-mobile.png) |

**Exact file paths for screenshots:**

| Caption | Path |
|--------|------|
| Storefront — Home | `docs/screenshots/storefront-home.png` |
| Storefront — Product detail | `docs/screenshots/product-detail.png` |
| Storefront — Checkout | `docs/screenshots/checkout.png` |
| Admin — Dashboard | `docs/screenshots/admin-dashboard.png` |
| Admin — Product analytics | `docs/screenshots/admin-analytics-products.png` |
| Admin — Product management | `docs/screenshots/admin-products.png` |
| Admin — Mobile | `docs/screenshots/admin-mobile.png` |

---

## Demo

*Add a short screen recording to showcase the full flow: storefront → product → cart → checkout → admin order view.*

**File path:** `docs/screenshots/demo.gif`

![One Store — Demo](docs/screenshots/demo.gif)

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React 19, Next.js 16 (App Router), next-intl, Framer Motion, Zustand |
| **UI / Styling** | Tailwind CSS, tailwindcss-animate, Radix UI (primitives), class-variance-authority, clsx, tailwind-merge, Lucide icons, cmdk |
| **Backend** | Next.js Server Actions, Route Handlers (API routes) |
| **Database** | MongoDB |
| **ORM** | Prisma 5 |
| **Auth** | NextAuth v5 (Credentials); dual: User + Admin |
| **Payments** | Stripe (Payment Intents, webhooks), PayPal (orders, capture, webhooks) |
| **Email** | Resend |
| **SMS** | Twilio (optional; OTP/notifications) |
| **i18n** | next-intl (en, fr, ar), RTL support |
| **Charts / Export** | Recharts, xlsx, jspdf, html2canvas; jsbarcode for barcodes |
| **Media** | Sharp (resize, WebP), multipart upload API, `/uploads/[...path]` |
| **Tooling / Runtime** | TypeScript 5, Node, ESLint, PostCSS, Babel (React Compiler) |

---

## Architecture

- **Storefront vs Admin:** All routes live under `[locale]`. Storefront: `/`, `/products`, `/categories`, `/checkout`, `/account`, etc. Admin: `/[locale]/admin/*` (dashboard, orders, products, customers, categories, brands, ads, POS, settings). Admin uses a separate credential provider and `Admin` model; session carries `authType: "ADMIN"` and admin ID.
- **App routing:** Next.js App Router with `[locale]` at root; `next-intl` plugin and `routing` define locales and navigation (Link, redirect, usePathname, useRouter).
- **Server actions / data layer:** Server actions in `app/actions/` (e.g. `dashboard`, `admin-products`, `admin-analytics-products`, `products`, `catalog`, `settings`) and in `app/api/` for webhooks (Stripe, PayPal), upload, search, orders, favorites, ads (impression/click). Prisma used in actions and API routes; single `lib/prisma` singleton.
- **Prisma / DB:** MongoDB; models: User, Admin, AdminRole, AuditLog, SiteSettings, Category, Brand, Product, ProductVariant, VariantMedia, Order, OrderItem, OrderEmailLog, OrderConfirmationToken, CustomerNote, Favorite, Campaign, Ad, AdAsset, AdEvent, AdImpression, AdClick, AdUserCap, AdBehaviorProfile, AdSettings, Event.
- **Translations:** Messages in `messages/{en,fr,ar}.json`; `getTranslations` / `useTranslations` (next-intl); product/category/brand names in DB as JSON for each locale.
- **Analytics:** Dashboard stats and charts from Prisma aggregations over `Order` / `OrderItem` with date range and payment status; product-level analytics in `admin-analytics-products` (revenue, cost, profit, margin, stock, low stock) with period filter.
- **Uploads / media:** POST to `/api/upload` (auth + role check); Sharp for images; files on disk; GET `/uploads/[...path]` serves from same base path (path traversal guarded).

---

## Project Structure

```
├── prisma/
│   ├── schema.prisma          # MongoDB models: User, Admin, RBAC, Products, Orders, Ads, etc.
│   └── seed/                  # seed, seed-catalog, create-admin, seed-admin-roles, seed-ads
├── src/
│   ├── app/
│   │   ├── [locale]/          # Locale-prefixed routes
│   │   │   ├── page.tsx       # Home
│   │   │   ├── products/      # Listing, [slug] PDP
│   │   │   ├── categories/    # Category page
│   │   │   ├── checkout/      # Checkout, success
│   │   │   ├── account/       # Account, profile
│   │   │   ├── orders/        # Customer orders
│   │   │   ├── favorites/
│   │   │   ├── auth/          # Sign-in, sign-up
│   │   │   └── admin/         # Dashboard, orders, products, customers, categories, brands,
│   │   │                       # ads (list, create, edit, events, analytics, settings), POS, settings, profile
│   │   ├── actions/           # Server actions: dashboard, admin-products, admin-analytics-products,
│   │   │                       # products, catalog, settings, etc.
│   │   ├── api/               # upload, orders, favorites, search, webhooks (stripe, paypal), ads
│   │   └── uploads/[...path]/ # Serve uploaded files
│   ├── components/            # ui/, admin/, store/, home/, auth/, checkout/, ads/, etc.
│   ├── lib/                   # prisma, auth, stripe, paypal, email, sms, ads, rbac, permissions, money, etc.
│   ├── i18n/                  # routing, request (next-intl)
│   ├── messages/              # en.json, fr.json, ar.json
│   └── auth.ts, auth.config.ts
├── docs/                      # Implementation notes, screenshots, banner, demo
│   └── screenshots/           # storefront-home.png, product-detail.png, checkout.png,
│                              # admin-dashboard.png, admin-analytics-products.png, admin-products.png,
│                              # admin-mobile.png, banner.png, demo.gif
├── package.json
└── next.config.ts
```

---

## Core Workflows

1. **Browsing and purchasing:** Customer visits `/[locale]`, browses categories/products, adds to cart (Zustand), goes to checkout; pending order is created with items and cost snapshot; payment via Stripe or PayPal; webhook marks order PAID and optionally awards loyalty points; confirmation email sent (Resend).
2. **Admin product management:** Admin logs in at `/[locale]/admin/login`; creates/edits products (with variants, media, SKU/barcode); publishes; taxonomy (categories/brands) managed separately.
3. **Media upload flow:** Admin/Editor uses upload component → POST `/api/upload` with multipart files → Sharp processes images (resize, WebP) → files saved to upload directory → URLs like `/uploads/...` returned; product/variant forms reference these URLs; GET `/uploads/[...path]` serves files.
4. **Order lifecycle:** PENDING → payment (Stripe/PayPal/COD) → PAID (webhook or confirm); optional SHIPPED/DELIVERED; CANCELED/FAILED/REFUNDED; order emails and confirmation tokens for guest flows.
5. **Analytics / product profitability:** Dashboard Overview shows revenue, profit estimate, order counts; Product Analytics page queries `OrderItem` for PAID orders in period, joins Product/ProductVariant for cost; shows revenue, cost, profit, margin; exact profit when cost is set, else estimated or none; stock and low-stock views from Product/ProductVariant.
6. **Localization flow:** User switches locale (e.g. language toggle); `[locale]` changes; next-intl provides messages; product/category/brand names read from JSON fields by locale; RTL and Tajawal applied for Arabic.

---

## Getting Started

### Prerequisites

- **Node.js** 20+
- **MongoDB** (local or Atlas)
- **npm** / **yarn** / **pnpm**

### Installation

```bash
git clone <your-repo-url>
cd one-store
npm install
```

### Environment setup

Copy the example below into `.env` and fill in your values (see [Environment Variables](#-environment-variables)).

### Database setup

1. Set `DATABASE_URL` to your MongoDB connection string.
2. Generate Prisma client and push schema:

```bash
npm run db:push
```

3. (Optional) Seed catalog, admin user, roles, ads:

```bash
npm run db:seed-catalog
npm run db:admin
npm run db:seed-roles
npm run db:seed-ads
```

### Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). Use a locale prefix, e.g. [http://localhost:3000/en](http://localhost:3000/en) or `/fr`, `/ar`.

### Production build and start

```bash
npm run build
npm run start
```

---

## Environment Variables

Create a `.env` file in the project root. Example (only includes variables referenced in the codebase):

```env
# Database
DATABASE_URL="mongodb+srv://user:[EMAIL REDACTED]/onestore?retryWrites=true&w=majority"

# Auth (NextAuth)
AUTH_SECRET="your-generated-secret"
NEXTAUTH_URL="http://localhost:3000"

# Stripe
STRIPE_SECRET_KEY="REPLACE_WITH_PRIVATE_ENV_VALUE"
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_..."
STRIPE_WEBHOOK_SECRET="REPLACE_WITH_PRIVATE_ENV_VALUE"

# PayPal
PAYPAL_CLIENT_ID="..."
PAYPAL_CLIENT_SECRET="REPLACE_WITH_PRIVATE_ENV_VALUE"
PAYPAL_ENV="sandbox"
PAYPAL_WEBHOOK_ID="..."

# Email (Resend)
RESEND_API_KEY="REPLACE_WITH_PRIVATE_ENV_VALUE"
RESEND_FROM="[EMAIL REDACTED]"

# Optional: support / store
DEFAULT_SUPPORT_EMAIL="[EMAIL REDACTED]"
DEFAULT_SUPPORT_PHONE="[PHONE REDACTED]"
NEXT_PUBLIC_APP_URL="https://yourdomain.com"
NEXT_PUBLIC_STORE_NAME="One Store"

# Optional: SMS (Twilio)
TWILIO_ACCOUNT_SID="..."
TWILIO_AUTH_TOKEN="REPLACE_WITH_PRIVATE_ENV_VALUE"
TWILIO_FROM="[PHONE REDACTED]"

# Optional: ads debugging
ADS_DEBUG="0"
NEXT_PUBLIC_ADS_DEBUG="0"
```

Upload directory for `/api/upload` and `/uploads/[...path]` is currently configured in code (e.g. a path like `C:\onestore-data\uploads` or your server path). Change it in `src/app/api/upload/route.ts` and `src/app/uploads/[...path]/route.ts` to match your environment.

---

## Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start Next.js development server |
| `npm run build` | Production build |
| `npm run start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm run db:push` | Generate Prisma client and push schema to DB |
| `npm run db:seed` | Run main seed script |
| `npm run db:seed-catalog` | Seed catalog (categories, brands, products) |
| `npm run db:admin` | Create admin user |
| `npm run db:seed-roles` | Seed admin roles and permissions |
| `npm run db:seed-ads` | Seed ads-related data |
| `npm run i18n:audit` | Audit i18n keys (scripts/i18n-audit.mjs) |

---

## Deployment

- **Build:** `npm run build` (output in `.next`).
- **Runtime:** Node; run `npm run start` or use a process manager (e.g. PM2).
- **Database:** Use MongoDB Atlas or a self-hosted MongoDB; set `DATABASE_URL`.
- **Auth:** Set `AUTH_SECRET` (e.g. `openssl rand -base64 32`) and `NEXTAUTH_URL` to your production URL.
- **Payments:** Configure Stripe/PayPal webhooks to your production URLs; set webhook secrets in env.
- **Uploads:** Ensure the upload directory used in `/api/upload` and `/uploads/[...path]` exists and is writable; consider a dedicated volume or object storage for scale.
- **Vercel:** Can be deployed as a Node server; configure env vars and (if needed) external MongoDB and file storage.

---

## Internationalization

- **Supported locales:** `en` (default), `fr`, `ar`.
- **Configuration:** `src/i18n/routing.ts` defines locales and default; `next-intl` plugin in `next.config.ts`.
- **Messages:** JSON files in `src/messages/`; namespaced keys (e.g. `auth`, `admin`, `home`); used via `getTranslations` (server) and `useTranslations` (client).
- **RTL:** For `ar`, `LocaleHtmlAttributes` sets `dir="rtl"` and `lang="ar"` on `<html>`; Tajawal font is loaded for Arabic.
- **Content in DB:** Product title/description, category name, brand name stored as JSON `{ en, fr, ar }` and resolved by current locale in queries or components.

---

## Analytics

- **Dashboard Overview:** Revenue, profit estimate, order counts (pending, paid, shipped, delivered, failed, cancelled, refunded), AOV, total stock, low-stock count; period filter (day/week/month/year); trend vs previous period; recent sales list; charts; export to CSV, Excel, PDF, JSON, PNG.
- **Product analytics (dedicated page):** Tables for revenue by product/variant (units sold, revenue, order count); profit (cost, profit, margin %, exact vs estimated); stock by product/variant; low-stock list. Period filter applied to revenue/profit; stock and low-stock are current snapshot. Intended for understanding which products drive profit and which need restock.

---

## Roadmap

| Priority | Area | Description |
|:--------:|------|--------------|
| **High** | Refunds | Line-level refund quantity/amount for per-product refund reporting. |
| **High** | Product cost | Product-level cost for non-variant products in profit analytics. |
| **Medium** | Upload config | Make upload directory configurable via `UPLOAD_DIR` (or similar) env. |
| **Medium** | Object storage | Optional S3/compatible storage for uploads instead of local disk. |
| **Medium** | Tests | Unit and integration tests for server actions and API routes. |
| **Low** | Locales | Add more languages and message files. |
| **Low** | RBAC | Fine-grained nav access for product analytics vs dashboard (e.g. dedicated permission key). |

*Roadmap is indicative; priorities may shift with product needs.*

---

## Contributing

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit changes with clear messages.
4. Push and open a Pull Request.

Please keep the codebase consistent with existing patterns (server actions, Prisma, next-intl, RBAC) and update docs if you change setup or env.

---

## License

Proprietary / To be defined.

---

## Author & Credits

**One Store** — Full-stack e-commerce and backoffice platform.

Built with [Next.js](https://nextjs.org), [Prisma](https://www.prisma.io), [next-intl](https://next-intl-docs.vercel.app), [Stripe](https://stripe.com), [PayPal](https://developer.paypal.com), [Resend](https://resend.com), [Tailwind CSS](https://tailwindcss.com), and [Radix UI](https://www.radix-ui.com).
