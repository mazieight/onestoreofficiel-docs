# Cost Fields — Admin Products & Variants

## Summary

Product and variant cost fields are wired end-to-end: form → DB → new orders (`OrderItem.costPrice`) → Product Analytics Profit tab (Exact when cost exists).

---

## 1. Exact files changed

| File | Change |
|------|--------|
| `src/app/actions/admin-products.ts` | `getProductById` returns `cost` on the product object. |
| `src/app/actions/products.ts` | `createProduct` (non-TV) persists `Product.cost`; `updateProduct` persists `Product.cost` and variant `compareAtPrice` / `cost` / `lowStockThreshold` on **create** branch of variant upsert. |
| `src/app/[locale]/admin/products/page.tsx` | Load edit: form reset uses `product.cost` and variant `compareAtPrice`, `cost`, `lowStockThreshold`; variant details modal: cost input has placeholder and `costHelp` text. |

**Not changed (already in place):**

- `prisma/schema.prisma` — `Product.cost`, `ProductVariant.cost`, `OrderItem.costPrice` already added earlier.
- `src/lib/orders/create-pending-order.ts` — already sets `costPrice` from product/variant cost when creating order items.
- Product form schema and UI — already had `cost` field and `form.fields.cost` / `form.fields.costHelp`.
- Variant modal — already had cost state and save; only loading and help text were completed.
- **Translations** — EN/FR/AR already have `admin.products.form.fields.cost` and `admin.products.form.fields.costHelp` (no file changes).

---

## 2. Exact patch (logical diff)

### `src/app/actions/admin-products.ts`

- In `getProductById` return object, add:
  - `cost: (p as any).cost ?? undefined`

### `src/app/actions/products.ts`

- **createProduct (non-TV):** In `prisma.product.create` `data`, add:
  - `cost: data.cost != null && !Number.isNaN(data.cost) ? data.cost : null`

- **updateProduct:** In `prisma.product.update` `data`, add:
  - `cost: (data as any).cost != null && !Number.isNaN((data as any).cost) ? (data as any).cost : null`

- **updateProduct (TV variant upsert):** In the `create` branch of `prisma.productVariant.upsert`, add:
  - `compareAtPrice: v.compareAtPrice ?? null`
  - `cost: v.cost ?? null`
  - `lowStockThreshold: v.lowStockThreshold != null ? Number(v.lowStockThreshold) : null`

### `src/app/[locale]/admin/products/page.tsx`

- In `form.reset` when loading product for edit:
  - Replace `cost: 0` with `cost: (product as any).cost ?? 0`.
  - In the variant type and map, add `compareAtPrice`, `cost`, `lowStockThreshold` to the variant type and to the mapped object: `compareAtPrice: v.compareAtPrice`, `cost: v.cost`, `lowStockThreshold: v.lowStockThreshold`.

- In the variant details modal (cost field):
  - Add `placeholder="0.00"` to the cost `Input`.
  - After the cost `Input`, add: `<p className="text-xs text-muted-foreground">{t("form.fields.costHelp")}</p>`.

---

## 3. Translations (already present)

No message file changes were required. Existing keys:

- **EN:** `admin.products.form.fields.cost` = "Cost", `admin.products.form.fields.costHelp` = "Internal cost used for margin calculation. Not shown to customers."
- **FR:** `cost` = "Coût", `costHelp` = "Coût interne utilisé pour le calcul de la marge. Non visible par les clients."
- **AR:** `cost` = "التكلفة", `costHelp` = "التكلفة الداخلية لحساب هامش الربح. لا تظهر للعملاء."

---

## 4. Validation steps

### 4.1 Product cost (non-TV product)

1. Go to **Admin → Products**, add or edit a **non-TV** product (e.g. category other than TVs).
2. In **Pricing & Inventory**, set **Cost** (e.g. 50), save.
3. In DB or via an API that returns the product, confirm `Product.cost` is 50 (or the value you set).
4. Create a new order containing that product (storefront checkout or POS).
5. In DB, confirm the corresponding `OrderItem` has `costPrice` = 50.
6. Open **Dashboard → Product analytics → Profit** tab; that product’s row should show **Exact** and profit = revenue − (50 × quantity).

### 4.2 Variant cost (TV product)

1. Go to **Admin → Products**, add or edit a **TV** product with at least one size.
2. For a size, click **Details**, set **Cost** (e.g. 200), save variant then save product.
3. In DB, confirm that `ProductVariant` for that size has `cost` = 200.
4. Create a new order with that TV variant.
5. In DB, confirm `OrderItem.costPrice` = 200.
6. In **Product analytics → Profit**, that variant’s row should show **Exact** and correct profit.

### 4.3 Edit load (cost round-trip)

1. Edit a product (non-TV) that has `cost` set in DB; open the product form.
2. Confirm the **Cost** field shows the saved value (not 0 or empty).
3. For a TV, open a variant that has `cost` set; open **Details**.
4. Confirm the **Cost** field shows the saved value.

### 4.4 Optional cost (null)

1. Save a product (or variant) with **Cost** empty or 0.
2. In DB, confirm `Product.cost` or `ProductVariant.cost` is `null` (or 0 if you explicitly want 0 stored).
3. New orders for that product/variant should have `OrderItem.costPrice` null.
4. In **Product analytics → Profit**, that row should show **Estimated** or **—** (no Exact badge).

### 4.5 Analytics Exact vs Estimated

1. With at least one product/variant with cost set and at least one without:
2. Create paid orders for both.
3. Open **Product analytics → Profit**.
4. Rows with cost set should show **Exact** and numeric cost/profit/margin.
5. Rows without cost should show **Estimated** or **—** and no incorrect “exact” profit.

---

## 5. Data flow

- **Product form (non-TV):** `cost` in form → `createProduct` / `updateProduct` → `Product.cost` (or null).
- **Variant form (TV):** `cost` in variant details modal → form `variants[i].cost` → `updateProduct` → `ProductVariant.cost` (or null).
- **New order:** `create-pending-order.ts` reads `Product.cost` or `ProductVariant.cost` and sets `OrderItem.costPrice`.
- **Analytics:** `getProductLevelAnalytics` uses `OrderItem.costPrice` or current product/variant cost; when present, profit is **Exact**.
