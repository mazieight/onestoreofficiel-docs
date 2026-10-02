# Phase 2 — Remaining Work (after Product Analytics v1)

Phase 1 is done: schema changes, `/admin/analytics/products`, KPI links, and exact vs estimated vs no-cost display.

---

## 1. Product detail analytics page

**Route:** `/[locale]/admin/analytics/products/[productId]` (or `.../products/[productId]/analytics`).

**Purpose:** When clicking a product row (or a “View detail” link), open a dedicated page for that product.

**Content to add:**
- **Sales over time** — units sold and revenue by day/week/month (from OrderItem for this product, PAID orders).
- **Profit over time** — when cost exists (exact or from current product/variant cost).
- **Stock** — current stock (product + variants) and optionally stock movement if you add history later.
- **Top variants** — for products with variants: table of variants by revenue/units/margin.
- **Orders involving this product** — list or count of orders that include this product (with link to order detail).
- **Refunds/cancellations** — for now order-level only (e.g. “X orders including this product were refunded/cancelled”); line-level when Phase 2 refunds are done.

**Files to add/change:**
- New page: `src/app/[locale]/admin/analytics/products/[productId]/page.tsx`.
- New (or extended) server actions: e.g. `getProductDetailAnalytics(productId, period)` in `admin-analytics-products.ts`.
- Optional: “View detail” link on each row in the product analytics table → product detail page.

---

## 2. Line-level refunds

**Current:** Refunds are order-level only (`Order.paymentStatus = REFUNDED`). You cannot say “product X had 3 units refunded” or “refunded value per product”.

**To support product-level refund impact:**
- **Option A — RefundItem (recommended):** New model, e.g. `RefundItem` with `orderId`, `orderItemId` (or productId + variantId), `quantity`, `amount`, `createdAt`. When processing a refund, create one row per refunded line (or one row per product/variant with total refunded qty and amount).
- **Option B — Fields on OrderItem:** Add optional `refundedQuantity` and `refundedAmount` on `OrderItem`, and update them when a refund is processed. Simpler schema but less history (e.g. partial refunds over time).

**Then:**
- In `getProductLevelAnalytics` (or a new function), subtract refunded quantity/amount per product so “revenue” and “profit” can be “net of refunds”.
- In product detail analytics, show “Refunded: X units, Y value” for that product.
- Optionally: “Refunded” KPI card could open a view of products by refunded value (once data exists).

---

## 3. Admin UI to edit product / variant cost

**Current:** `Product` has `cost` and `ProductVariant` has `cost` in the schema, but the admin product form likely does not expose them.

**To add:**
- **Product form (simple product):** Add an optional “Cost” (or “Purchase price” / “Supplier cost”) field; save to `Product.cost`. Only show when product has no variants, or use as fallback when variant has no cost.
- **Variant form (product with variants):** In the variant editor (modal or inline), add optional “Cost” per variant; save to `ProductVariant.cost`.

**Files to change:**
- Admin products page: `src/app/[locale]/admin/products/page.tsx` (or wherever the product/variant create/edit form lives).
- If validation lives in actions: `src/app/actions/admin-products.ts` — allow `cost` in the payload and pass it to `prisma.product.update` / variant update.
- Optionally: validation (e.g. cost ≥ 0, cost &lt; price) and labels/tooltips (“Used for profit calculation”).

**Result:** Admins can set cost so that:
- New orders get `OrderItem.costPrice` at checkout.
- Product analytics “Profit” tab shows **Exact** for those products/variants and real per-product profit.

---

## Summary

| Item | Priority | Effort |
|------|----------|--------|
| Product detail analytics page | High | Medium (new page + 1–2 actions) |
| Line-level refunds | Medium | High (schema + refund flow + analytics) |
| Admin UI: product/variant cost | High | Low (form fields + action) |

Recommended order: (1) Admin UI for cost, (2) Product detail analytics page, (3) Line-level refunds when you need refund reporting by product.
