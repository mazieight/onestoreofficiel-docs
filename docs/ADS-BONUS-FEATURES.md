# Ads Platform – Bonus Pro Max Features

## 1) AI Smart Optimization Engine

- **Location:** `src/lib/ads-optimization.ts`, Admin → Ads → **Settings**
- **Behavior:**
  - Tracks CTR per ad.
  - If **Auto Optimization** is enabled (toggle in Settings) and ad has **Auto Optimize** on:
    - CTR &lt; threshold after min impressions → weight reduced.
    - CTR high → weight increased.
    - Very low CTR (e.g. &lt; 0.1%) after min impressions → ad auto-paused.
  - **Suggestions:** “This ad underperforms, consider replacing creative” for low CTR ads.
- **API:** `GET/PUT /api/admin/ads/settings`, `POST /api/admin/ads/run-optimization`
- **DB:** `AdSettings` (singleton id = "default"): `autoOptimizeEnabled`, `ctrThreshold`, `minImpressionsOptim`, `fatigueIgnoreThreshold`

---

## 2) A/B Testing (Campaigns & Variants)

- **DB:** `Campaign` (name, status, testPeriodEnd, winnerAdId), `Ad.campaignId`, `Ad.variantLabel`, `Ad.trafficWeight`
- **Flow:** Create a Campaign, then create multiple Ads with the same `campaignId`, different `variantLabel` (A, B, C) and optional `trafficWeight`. Engine picks one variant per campaign by weight for each request.
- **API:** `GET/POST /api/admin/ads/campaigns`, `GET /api/admin/ads/campaigns/:id/recommendation`, `POST /api/admin/ads/campaigns/:id/winner` (body: `{ winnerAdId }`)
- **Recommendation:** After test period, call recommendation API to get “Variant B performs X% better” and **Set Winner** to set `Campaign.winnerAdId` and mark campaign completed.

---

## 3) Personalized Ads (Behavioral Targeting)

- **DB:** `AdBehaviorProfile` (userKey, viewedCategoryIds, viewedProductIds, abandonedProductIds, pageVisitCount, firstVisitTodayAt)
- **Targeting:**
  - **Boost by category:** User’s most viewed categories → boost ads whose targeting includes those category IDs.
  - **Retarget abandoned cart:** In ad targeting set `retargetAbandonedCart: true`; only users with `abandonedProductIds` non-empty see the ad; scoring boost applied.
- **Tracking:** `POST /api/ads/behavior` with `action`: `categoryView` | `productView` | `abandonedCart` | `pageVisit`. Client calls `trackBehavior('productView', { productId })` e.g. on product page; optional `recordAbandonedCart(userKey, productIds)` when leaving cart with items.
- **Context:** `GET /api/ads/context?userKey=...` returns `boostedCategoryIds`, `abandonedProductIds`, `pageVisitCount`, `firstVisitToday`. Used by `GET /api/ads` for scoring.

---

## 4) Dynamic Event Triggers

- **Ad fields:** `triggerTypes` (JSON array): `after30s` | `scroll60` | `exitIntent` | `firstVisitDay` | `3rdPageVisit`
- **Client:** `useAdTrigger(triggerTypes, pageVisitCount, firstVisitToday)` in `AdSlot`: ad is not rendered until the selected trigger fires (e.g. 30s on page, 60% scroll, mouse leave at top, first visit today, 3rd page).
- **Location:** `src/components/ads/use-ad-trigger.ts`, used in `ad-slot.tsx`

---

## 5) Frequency & Fatigue Control

- **Caps:** Existing `frequencyCaps`: `viewsPerDay`, `clicksPerDay`, `cooldownMinutes`.
- **New:** `fatigueIgnoreThreshold` (e.g. 5): after (views − clicks) ≥ this for the user/day, ad’s effective weight is multiplied by `fatigueCooldownMultiplier` (default 0.5) so it is shown less often.
- **DB:** `AdUserCap` already has views/clicks per ad/user/date; fatigue uses (viewsCount − clicksCount) and optional `ignoreCount` for future “dismiss” events.

---

## 6) Cinematic Creative Studio

- **Ad field:** `creativeOptions` (JSON): `overlayText`, `font`, `gradientCss`, `ctaStyle` (primary | outline | ghost), `entranceAnimation` (fade | slideUp | slideLeft | zoom).
- **Rendering:** `AdCinematic` in `ad-renderer.tsx` uses these for overlay text, gradient, CTA button style, and entrance animation. Admin can set these in the ad form (e.g. JSON or dedicated fields) for a live-preview-style creative studio.

---

## 7) Revenue Mode (CPM / CPC)

- **Ad fields:** `costModel` (CPM | CPC), `costValue` (e.g. 2.50).
- **Recording:** On impression, if `costModel === "CPM"` create `AdRevenue` with `amount = costValue / 1000`. On click, if `costModel === "CPC"` create `AdRevenue` with `amount = costValue`.
- **DB:** `AdRevenue` (adId, type, amount, referenceType, referenceId). Ready for billing and reporting.

---

## 8) Safety & Performance

- **Rendering:** Ads do not block initial render; slots render after data fetch. `AdRenderer` uses `IntersectionObserver` for impression and lazy display.
- **Media:** `loading="lazy"`, `decoding="async"` on images; video uses `loading="lazy"` where supported.
- **A11y:** `role="article"`, `aria-label` on ad container and CTA link.
- **Upload:** Image/video upload remains in `/api/upload`; optional server-side compression can be added there (e.g. sharp for images).

---

## 9) Smart Dashboard Insights

- **Analytics API** (`GET /api/admin/ads/analytics`) now also returns:
  - **bestPlacement** – placement with best CTR
  - **bestCreativeType** – ad type with best CTR
  - **peakEngagementHours** – top 5 hours by impression count
  - **suggestedImprovements** – e.g. underperformers, “allocate more to best placement”, “test similar creative type”
- **Admin:** Analytics page shows an **Insights** card with these and suggested improvements.

---

## Admin UI Additions

- **Ads → Settings:** Toggle “Enable Auto Optimization”, CTR threshold, min impressions, fatigue threshold; **Run optimization** button.
- **Ads → Analytics:** Insights card (best placement, best creative type, peak hours, suggested improvements).
- **Campaigns:** Use `GET/POST /api/admin/ads/campaigns` and recommendation/winner endpoints to build a Campaigns list, create campaign, add variants (ads with same campaignId), view recommendation, set winner.

---

## Schema Additions (Prisma)

- `Campaign`, `Ad.campaignId`, `Ad.variantLabel`, `Ad.trafficWeight`
- `Ad.autoOptimize`, `Ad.triggerTypes`, `Ad.creativeOptions`, `Ad.costModel`, `Ad.costValue`
- `AdRevenue`
- `AdUserCap.ignoreCount`
- `AdBehaviorProfile`
- `AdSettings` (singleton)
- `Ad.targetingRules.retargetAbandonedCart`, `Ad.frequencyCaps.fatigueIgnoreThreshold`, `fatigueCooldownMultiplier`

Run `npm run db:push` and `npm run db:seed-ads` to apply and seed (including default AdSettings).
