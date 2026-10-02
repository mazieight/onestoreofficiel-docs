# Per-Product Profitability & Analytics — Analysis & Implementation Plan

## 1. DIRECT ROOT CAUSE

**Why the current dashboard cannot yet show true per-product profit/details:**

1. **Aggregation level**: Dashboard metrics are computed at **order level only** (`Order.totalAmount`, `Order.paymentStatus`). There are no queries over `OrderItem`; revenue is `_sum(totalAmount)` for PAID orders, not sum of (quantity × price) per product.

2. **No cost on Product**: The `Product` model has `price` and `stock` but **no cost/purchase price field**. So for products without variants, **exact profit is impossible** with the current schema.

3. **Profit is a global estimate**: `profitEstimate = revenue * 0.25` (hardcoded 25% margin). No per-product or per-variant cost is used.

4. **Cost only on variants**: `ProductVariant` has an optional `cost` (Float?). So **exact profit is possible only for variant products when `cost` is set**; base products and variants without cost cannot show real profit.

5. **No cost snapshot on OrderItem**: When an order is created, only `price` (sale price) is stored per line. **Cost is not stored at order time.** So historical profit would have to use **current** Product/ProductVariant cost (which can change), unless we add a `costPrice` (or similar) field to `OrderItem` for historical correctness.

6. **Refunds are order-level only**: `Order.paymentStatus = REFUNDED` exists, but there is **no line-level refund quantity or refund amount**. We cannot compute “refunded quantity per product” or “refunded value per product” from current data.

7. **KPI cards link to orders/products lists, not to product-level analytics**: Clicking Revenue or Profit goes to dashboard with a metric dialog or to orders/products with filters—not to a **product-level breakdown** (revenue/profit by product/SKU).

8. **Stock and low stock**: Stock is available at both Product and ProductVariant level. Low stock is currently a **product-level** count (products where `stock <= 5`). Variant-level low stock (`ProductVariant.lowStockThreshold`) is not used in the dashboard.

---

## 2. DATA AVAILABILITY ANALYSIS

| Required metric | Current DB support | Notes |
|----------------|--------------------|--------|
| **Revenue per product** | ✅ Yes | `OrderItem`: productId, variantId, quantity, price. Sum(quantity × price) for orders with paymentStatus PAID. |
| **Revenue per variant** | ✅ Yes | Same, grouped by variantId. |
| **Cost per product** | ❌ No | `Product` has no cost field. |
| **Cost per variant** | ✅ Partial | `ProductVariant.cost` exists (optional). When set, real cost; when null, must use estimate or leave blank. |
| **Exact profit per product** | ❌ No (base product) / ✅ Partial (variant) | Base product: no cost → no real profit. Variant: real profit when variant.cost is set. |
| **Stock per product** | ✅ Yes | `Product.stock`. |
| **Stock per variant** | ✅ Yes | `ProductVariant.stock`. |
| **Refunds per product** | ❌ No | Only order-level REFUNDED; no item-level refund qty or amount. |
| **Cancellations per product** | ⚠️ Partial | Orders with status CANCELED/FAILED can be joined to OrderItem → “products in cancelled/failed orders”; not “cancelled quantity per product” (no line-level cancel/refund). |
| **Low stock (product)** | ✅ Yes | Count products where stock <= threshold (e.g. 5). |
| **Low stock (variant)** | ✅ Yes | `ProductVariant.lowStockThreshold` and `stock` exist; not yet used in dashboard. |

**Summary:**

- **Exact real profit**: Only where we have cost (today: variant with `cost` set). Base products and variants without cost → **estimated profit only** (e.g. margin on revenue) or show “—” for profit.
- **Refunded quantity/value per product**: Not possible without schema/data changes (e.g. RefundItem or line-level refund fields).

---

## 3. EXACT IMPLEMENTATION PLAN (per KPI card)

| KPI card | What should happen when clicked |
|----------|---------------------------------|
| **Revenue** | Open **Product analytics** (or dedicated “Revenue by product” view): table with Product, SKU/Variant, Units sold, Revenue, optional Order count. Filter: paid orders in selected period. Link row → product detail analytics page. |
| **Profit** | Same product-level table: Product, SKU/Variant, Units sold, Revenue, Cost, Profit, Margin %. Cost/profit: **exact** where variant/product cost exists; **estimated** (or “—”) otherwise. Label clearly “Exact” vs “Estimated”. |
| **Orders (paid)** | Keep existing: orders list filtered by paymentStatus=PAID. Add optional “By product” tab or link to product analytics filtered by “products in paid orders”. |
| **Total stock** | Open **Stock by product** view: Product, Variant/SKU, Stock, Low stock status. Optionally link to products list with tab=all. |
| **Low stock** | Open **Low stock** view: products/variants where stock ≤ threshold (product-level and/or variant-level). Table: Product, Variant, Stock, Threshold, Status. Link to product edit or product detail. |
| **Failed** | Orders list with kpi=failed (existing). Optional: “Products in failed orders” breakdown (from OrderItems of failed orders). |
| **Cancelled** | Orders list status=CANCELED (existing). Optional: “Products in cancelled orders” breakdown. |
| **Refunded** | Orders list paymentStatus=REFUNDED (existing). Product-level “refunded value” only if we add refund line items later; for now order-level only. |
| **Avg order value** | Keep dashboard dialog or open analytics with AOV and optionally “AOV by product” (revenue/order count per product) if useful. |

**Architecture:**

- **A. KPI summary cards** — Unchanged; keep global totals.
- **B. Product-level drilldown** — New route: e.g. `/[locale]/admin/analytics/products` (or `.../dashboard/analytics/products`) with:
  - Period filter (day/week/month/year).
  - View/tabs: Revenue by product | Profit by product | Stock | Low stock.
  - Table: product name, SKU/variant, units sold, revenue, cost, profit, margin %, stock, low stock status, order count.
  - Sort/filter by product, SKU, revenue, profit, margin.
- **C. Product analytics detail page** — New route: `/[locale]/admin/analytics/products/[productId]` (or `.../products/[productId]/analytics`):
  - Sales over time (units, revenue).
  - Profit over time (when cost available).
  - Stock movement (if we add stock history later; else current stock + variant breakdown).
  - Top variants (by revenue/units).
  - Orders/refunds/cancellations involving this product (order-level refund/cancel only until we have line-level data).

---

## 4. SCHEMA CHANGES (recommended)

To support **exact** per-product profit and clear “exact vs estimated”:

1. **Add `cost` to Product (optional Float)**  
   - So simple products (no variants) can have a cost and thus real profit.

2. **Keep `ProductVariant.cost`**  
   - Already exists; use it for variant-level profit.

3. **Optional but best practice: add `costPrice` to OrderItem (optional Float)**  
   - Store cost at order creation time (from Product.cost or ProductVariant.cost).  
   - Enables **historical** profit that does not change when you later edit product/variant cost.  
   - If not added, historical profit must use **current** cost (or show as estimated when cost changes).

4. **Refunds (future)**  
   - For “refunded quantity per product” and “refunded value per product”: either:
     - Add a `Refund` / `RefundItem` model (orderId, productId?, variantId?, quantity, amount?), or
     - Add optional `refundedQuantity` / `refundedAmount` on OrderItem and update when processing refunds.  
   - Not in scope for the first implementation; keep refund at order level.

**Recommended Prisma changes (minimal for real profit):**

```prisma
model Product {
  // ... existing fields ...
  cost    Float?   // optional; when set, used for profit for products without variants
}

model OrderItem {
  // ... existing fields ...
  costPrice Float?  // optional; snapshot at order creation for historical profit
}
```

Then in `create-pending-order.ts` (and any other order-creation path), when creating each OrderItem, set `costPrice` from:
- variant: `variant.cost ?? undefined`
- product only: `product.cost ?? undefined`

So:
- **Exact real profit**: when `OrderItem.costPrice` is set (or when we use current Product/ProductVariant cost and they are set).
- **Estimated profit**: when cost is missing, use a label “Estimated (margin %)” and optionally a global margin.

---

## 5. FILES TO CHANGE

| Area | Files |
|------|--------|
| **Schema** | `prisma/schema.prisma` (Product.cost, OrderItem.costPrice). |
| **Order creation** | `src/lib/orders/create-pending-order.ts` (set OrderItem.costPrice from product/variant cost). |
| **Dashboard actions** | `src/app/actions/dashboard.ts` (keep as is or add helpers for product-level if needed). |
| **New server actions** | `src/app/actions/admin-analytics-products.ts` (per-product revenue, cost, profit, stock, low stock). |
| **Analytics pages** | New: `src/app/[locale]/admin/analytics/products/page.tsx` (product-level table). New: `src/app/[locale]/admin/analytics/products/[productId]/page.tsx` (product detail analytics). |
| **Dashboard KPI links** | `src/components/admin/dashboard/kpi-grid.tsx` (point Revenue, Profit, Stock, Low stock to new analytics routes or same page with query params). |
| **Sidebar / nav** | `src/components/admin/layout/sidebar.tsx` (optional: add “Product analytics” under Dashboard or Analytics). |
| **Translations** | `src/messages/en.json`, `src/messages/fr.json`, `src/messages/ar.json` (new keys under `admin.analytics.products` and related). |

---

## 6. COPY-PASTE PATCH

See implementation in the codebase:

- `prisma/schema.prisma` — add `Product.cost`, `OrderItem.costPrice`.
- `src/app/actions/admin-analytics-products.ts` — new file (per-product queries).
- `src/app/[locale]/admin/analytics/products/page.tsx` — new product-level analytics page.
- `src/components/admin/dashboard/kpi-grid.tsx` — update hrefs for Revenue, Profit, Stock, Low stock to `/admin/analytics/products?view=...`.
- `src/messages/en.json` (and fr/ar) — new keys for product analytics.

---

## 7. TRANSLATIONS (EN / FR / AR)

Keys and values for the new product analytics / product profit UI (add under `admin.analytics.products` or `admin.dashboard.analytics.products`):

**EN (en.json):**

```json
"admin": {
  "analytics": {
    "products": {
      "title": "Product analytics",
      "subtitle": "Revenue, cost, and profit by product and variant",
      "period": "Period",
      "view": "View",
      "viewRevenue": "Revenue by product",
      "viewProfit": "Profit by product",
      "viewStock": "Stock by product",
      "viewLowStock": "Low stock",
      "product": "Product",
      "sku": "SKU",
      "variant": "Variant",
      "unitsSold": "Units sold",
      "revenue": "Revenue",
      "cost": "Cost",
      "profit": "Profit",
      "margin": "Margin",
      "stock": "Stock",
      "lowStockStatus": "Low stock",
      "orderCount": "Orders",
      "exact": "Exact",
      "estimated": "Estimated",
      "noCost": "—",
      "noData": "No data for the selected period",
      "backToDashboard": "Back to dashboard",
      "productDetail": "Product detail",
      "salesOverTime": "Sales over time",
      "revenueOverTime": "Revenue over time",
      "profitOverTime": "Profit over time",
      "topVariants": "Top variants"
    }
  }
}
```

**FR (fr.json):** same structure, French values (e.g. "Analytiques produit", "Revenu par produit", "Bénéfice par produit", "Stock par produit", "Stock faible", "Exact", "Estimé", "Retour au tableau de bord").

**AR (ar.json):** same structure, Arabic values with RTL (e.g. "تحليلات المنتجات", "الإيرادات حسب المنتج", "الربح حسب المنتج", "المخزون حسب المنتج", "مخزون منخفض", "دقيق", "تقديري", "العودة إلى لوحة التحكم").

---

## 8. VALIDATION

**How to verify:**

1. **Per-product numbers are automatic**  
   - Run reports for a given period; change period and confirm numbers change from real order/item data (no hardcoded values).

2. **Revenue**  
   - For a product, sum (OrderItem.quantity × OrderItem.price) for orders with paymentStatus PAID in period. Compare dashboard product table total to `Order.totalAmount` sum for same orders (should match).

3. **Cost and profit**  
   - Where Product.cost or ProductVariant.cost is set: profit = revenue − (cost × quantity). Margin = profit / revenue.  
   - Where cost is null: profit shows “Estimated” or “—” and no wrong “exact” number.

4. **Stock**  
   - Product table stock matches Product.stock and ProductVariant.stock. Low stock count matches products/variants where stock ≤ threshold.

5. **KPI cards → product-level detail**  
   - Click Revenue → product table with revenue column.  
   - Click Profit → same table with cost, profit, margin.  
   - Click Total stock → stock-by-product view.  
   - Click Low stock → only low-stock products/variants.

6. **Product detail page**  
   - Click a product row → detail page shows that product’s sales/revenue (and profit if cost exists) over time and by variant.

7. **Exact vs estimated**  
   - UI labels “Exact” when cost comes from DB (Product.cost / Variant.cost or OrderItem.costPrice). “Estimated” when using a margin or “—” when no cost.

---

*End of analysis and implementation plan.*
