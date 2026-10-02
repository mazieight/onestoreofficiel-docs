# OneStore Ads Platform

## 1) Root folder / files added

### Database & schema
- **prisma/schema.prisma** – Added models: `Ad`, `AdAsset`, `Event`, `AdEvent`, `AdImpression`, `AdClick`, `AdUserCap` and enums `AdStatus`, `AdType`.
- **prisma/seed/seed-ads.ts** – Seed script for 4 ads + 1 event.

### Lib
- **src/lib/ads-types.ts** – Shared types (Placement, PageType, AdRecord, TargetingRules, etc.).
- **src/lib/ads-engine.ts** – Selection engine: `getEligibleAds`, `selectBestAd`, `getAdForSlot`.
- **src/lib/ads-session.ts** – Client session id + `getPageTypeFromPath`.
- **src/lib/validators/ads.ts** – Zod schemas: `AdCreateSchema`, `AdUpdateSchema`, `EventCreateSchema`, `EventUpdateSchema`.

### API
- **src/app/api/ads/route.ts** – `GET /api/ads` (client: placement, pageType, route, etc.).
- **src/app/api/ads/impression/route.ts** – `POST /api/ads/impression` (single or batched).
- **src/app/api/ads/click/route.ts** – `POST /api/ads/click`.
- **src/app/api/admin/ads/route.ts** – `GET` (list), `POST` (create).
- **src/app/api/admin/ads/[id]/route.ts** – `GET`, `PUT`, `DELETE`.
- **src/app/api/admin/ads/[id]/pause|resume|stop|duplicate/route.ts** – Actions.
- **src/app/api/admin/ads/analytics/route.ts** – `GET` analytics.
- **src/app/api/admin/ads/events/route.ts** – `GET`, `POST`.
- **src/app/api/admin/ads/events/[id]/route.ts** – `PUT`, `DELETE`.

### Frontend (global + slots)
- **src/components/ads/ads-context.tsx** – `AdsProvider`, `useAds`, getAd, trackImpression, trackClick.
- **src/components/ads/ads-layout-client.tsx** – Wraps layout with AdsProvider + global slots.
- **src/components/ads/ad-slot.tsx** – `AdSlot`, `AdSlotTopBanner`, `AdSlotBottomSticky`, `AdSlotSidebar`, `AdSlotInFeed`, `AdSlotProduct`, `AdSlotCartCheckout`, `AdPopup`, `AdInterstitial`.
- **src/components/ads/ad-renderer.tsx** – `AdImage`, `AdVideo`, `AdCinematic`, `AdCarousel`, `AdRenderer`.

### Admin UI
- **src/components/admin/ads/ad-form.tsx** – Create/edit form (title, type, assets, CTA, placements, schedule, caps, safety, targeting JSON).
- **src/app/[locale]/admin/ads/page.tsx** – List with filters (status, placement, type) and actions.
- **src/app/[locale]/admin/ads/create/page.tsx** – Create ad page.
- **src/app/[locale]/admin/ads/[id]/edit/page.tsx** – Edit ad page.
- **src/app/[locale]/admin/ads/events/page.tsx** – Events CRUD.
- **src/app/[locale]/admin/ads/analytics/page.tsx** – Charts, by placement/page/device, top/underperformers, export CSV.

### Permissions & nav
- **src/lib/permissions.ts** – `ADS_READ`, `ADS_WRITE`; `NAV_PERMISSIONS` for `/admin/ads`, `/admin/ads/events`, `/admin/ads/analytics`.
- **src/lib/admin-rbac.ts** – EDITOR role includes `ADS_READ`, `ADS_WRITE`.
- **src/components/admin/layout/sidebar.tsx** – New “Ads” group: Ads, Events, Analytics; `ANY_NAV_PERMISSION` includes `ADS_READ`.
- **src/messages/en.json**, **fr.json**, **ar.json** – `nav.ads`, `nav.adsEvents`, `nav.adsAnalytics`, `nav.groups.ads`.

### Integration in app
- **src/app/[locale]/layout.tsx** – Uses `AdsLayoutClient` (AdsProvider + SiteHeader, AdSlotTopBanner, main, Footer, AdSlotBottomSticky, AdPopup).
- **src/components/store/product-grid.tsx** – In-feed ad every 8 items via `AdSlotInFeed`.
- **src/app/[locale]/products/[slug]/product-detail-client.tsx** – `AdSlotProduct` after tabs.
- **src/app/[locale]/checkout/page.tsx** – `AdSlotCartCheckout` at bottom.

---

## 2) How to test locally

1. **Apply schema and seed ads**
   ```bash
   npm run db:push
   npm run db:seed-ads
   ```
   If `db:push` fails with a duplicate key on `Product.sku` (e.g. `sku: null`), fix existing data or indexes in MongoDB first; the ads collections are created when the push runs until that error.

2. **Run the app**
   ```bash
   npm run dev
   ```

3. **Admin**
   - Log in as admin (e.g. `/en/admin/login`).
   - Open **Ads** in the sidebar: list, create, edit, duplicate, pause/resume/stop, delete.
   - Open **Events**: create event, link ads in ad form via “Event IDs”.
   - Open **Analytics**: view by day, placement, page type; export CSV.

4. **Storefront**
   - Visit `/en`, `/en/products`, `/en/products/<slug>`, `/en/checkout` (with items).
   - You should see:
     - **Top banner** below the navbar (dismissible).
     - **Bottom sticky** (dismissible, remembered in localStorage).
     - **In-feed** ads in the product grid (every 8 products).
     - **Product page** ad below description.
     - **Checkout** minimal ad at bottom.
     - **Popup** at most once per session (if an ad is configured for `popup`).

5. **API**
   - `GET /api/ads?placement=topBanner&pageType=home&route=/en&sessionId=xxx&locale=en&device=desktop`
   - `POST /api/ads/impression` – body: `{ adId, placement, sessionId, ... }`
   - `POST /api/ads/click` – body: `{ adId, placement, sessionId, ... }`

---

## 3) Example seed data (4 ads + 1 event)

**Event**
- **Spring Sale 2025** – start yesterday, end 60 days from now, priority 10.

**Ads (all active, 1–60 days window)**
1. **Top Banner – Spring Sale** – Image, placement `topBanner`, CTA “Shop now”, linked to event.
2. **Bottom Sticky – Free Shipping** – Image, placement `bottomSticky`, CTA “Learn more”.
3. **In-Feed Promo** – Cinematic, placement `inFeed`, targeting category/home, linked to event.
4. **Popup – One per session** – Image, placement `popup`, targeting guest, 1 view/day.

Seed script: `npm run db:seed-ads` (see **prisma/seed/seed-ads.ts**).

---

## 4) Safety and defaults

- **Do not show on checkout** – Default ON in ad form; engine excludes checkout (and cart) when `safetyRules.doNotShowOnCheckout` is true.
- **Adult content** – Flag in schema; engine excludes ads with `adultContent: true`; form default OFF.
- **Popup** – Once per session (sessionStorage); frequency cap 1 view/day in seed.
- **Interstitial** – Slot exists; no ads in seed; use only for high-priority events with strict caps.

---

## 5) Smart selection (engine)

- **Eligibility**: status ACTIVE, within start/end and daily window, placement match, targeting (pages, route, category, product, user segment, device, language), frequency caps (views/clicks per day, cooldown), safety (no checkout, no adult).
- **Ranking**: `priority * 100` + event boost + relevance (category/product/page) + weighted random among top candidates.
- **Rotation**: Weight used for random choice among top N to avoid repetition; cooldown respected.

All logic is in **src/lib/ads-engine.ts**; client calls **GET /api/ads** with context and renders the returned ad in the right slot.
