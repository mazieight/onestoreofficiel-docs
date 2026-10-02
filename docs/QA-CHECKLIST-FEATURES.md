# OneStore — QA Checklist (Admin, Storefront, Settings)

Use this checklist to verify the implemented features across locales and roles.

---

## 1) Admin Product Form — Categories & Brands

- [ ] **EN** Open `/en/admin/products` → Click "Add Product". Category dropdown shows all 11 categories (Laptops, Smartphones, Tablets, Smartwatches, Headphones, Cameras, Monitors, TVs, Gaming, Kitchen Appliances, Accessories) with translated labels.
- [ ] **FR** Same flow at `/fr/admin/products` — category labels in French.
- [ ] **AR** Same at `/ar/admin/products` — category labels in Arabic.
- [ ] Brand dropdown shows all brands (e.g. Apple, Samsung, Sony, HP, Dell, Lenovo, etc.) with **human-readable names**, not raw IDs.
- [ ] Saving a product with a selected category/brand persists correctly; editing loads the same category/brand.

---

## 2) Storefront Product Details Page

- [ ] From `/en/products` (or `/fr` / `/ar`), click a product card → navigates to `/[locale]/products/[slug]`.
- [ ] Product page shows: gallery (images + video if present), thumbnails row, title, badges (New/Promo/Sale/Sold Out), price, compare-at price (strikethrough) and discount % when applicable.
- [ ] Stock status: if stock = 0, "Sold Out" is shown and Add to Cart is disabled.
- [ ] Tabs: Description, Specs, Shipping & Returns work.
- [ ] "Related Products" section appears when there are other products in same category/brand.
- [ ] Prices use MAD (DH) and `formatPrice`; no USD.
- [ ] After publishing a product from admin, storefront product list/detail updates (revalidate).

---

## 3) Product Card — Hover Flip & Orange Shadow

- [ ] **Desktop:** Hover a product card on products grid → card flips to back with short description, brand, stock status, badges, "View details" CTA.
- [ ] Orange shadow/glow appears on hover (brand color).
- [ ] **Reduced motion:** If user has "Reduce motion", overlay or non-flip behavior is used instead of 3D flip.
- [ ] **Mobile:** No hover; tap opens product details; card shows normal front.

---

## 4) Sale vs Sold Out Logic

- [ ] **Sale** badge: shown when product has `badge === "sale"` OR compareAtPrice > price (discount). Not mixed with Sold Out.
- [ ] **Sold Out** badge: shown only when stockQuantity <= 0. Takes priority (e.g. Sold Out overlay; Add to Cart disabled).
- [ ] Admin product form: Badge select includes **New / Promo / Sale / Normal**.
- [ ] Storefront: Product with stock = 0 and on sale shows Sold Out prominently; sale info still visible in details.

---

## 5) Customers Page — VIP & Metrics

- [ ] **KPI cards** show real data: Total Customers, New This Month, Active Customers, **Total Revenue (MAD)**, **VIP Customers (count)**.
- [ ] **Add Customer** opens sheet/modal in all locales (`/en/admin/customers`, `/fr/...`, `/ar/...`).
- [ ] Add Customer form includes **Segment** (or "Customer Badge") select: New, VIP, Returning, Regular.
- [ ] Selecting **VIP** and saving creates customer with segment VIP (manual override).
- [ ] Filters: Segment (e.g. VIP), Status, Type work and update the table.

---

## 6) Settings Page — Social Links & RBAC

- [ ] **Settings** at `/en/admin/settings` (and `/fr`, `/ar`) opens without 404.
- [ ] **Store Profile** → Social Networks: fields are **Facebook, Instagram, YouTube, WhatsApp** (no LinkedIn/Twitter).
- [ ] **Roles & Permissions:** Role editor shows permission matrix; saving updates non-Owner roles.
- [ ] **Admin Team:** Add admin user generates username + strong password; copy button works; password stored hashed.
- [ ] Sidebar: user with Support role does **not** see Settings/Users; server blocks unauthorized access to settings actions.

---

## 7) General Quality

- [ ] No **missing i18n key** warnings in console for en/fr/ar.
- [ ] All storefront and admin copy use translations (no hardcoded EN-only strings in UI).
- [ ] **"All" tab** on products page works (no regression).
- [ ] Lists (products, customers, categories) do not fail entirely if one row is corrupt; defensive mapping/error handling in place.
- [ ] **MAD (DH)** and `formatPrice` used everywhere; no USD.
- [ ] First uploaded media is primary on product detail.

---

## Quick Smoke

1. Open `/en`, `/fr`, `/ar` — homepage loads.
2. Open `/en/admin/products` — add product, choose category + brand from full lists, set badge Sale, save.
3. Open `/en/products` → click product → detail page with gallery, price, related.
4. Open `/en/admin/customers` — add customer with segment VIP; check KPI cards.
5. Open `/en/admin/settings` — change social links (YouTube/WhatsApp), save; check Store Profile section.
