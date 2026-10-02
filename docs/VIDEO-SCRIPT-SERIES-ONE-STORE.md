# One Store — Full Video Script Series (Reels / TikTok / Shorts)

**First-person, founder/builder tone. A–Z journey: story, stack, build, bugs, fixes, result.**

---

## 1. Short Summary of the Project Story

I built **One Store** — a full-stack e-commerce platform with a public storefront, a separate admin backoffice, and real production deployment. It’s not a tutorial CRUD app: it has **multilingual support** (English, French, Arabic with RTL), **product management** with variants and cost fields, **orders** with Stripe and PayPal, **image and video uploads** with Sharp processing, **per-product analytics and profitability** (exact vs estimated profit), and an **ads platform** with campaigns and targeting. I went through real bugs — uploads not showing, product images 404, Next.js static vs runtime serving, duplicate locale routing like `/ar/ar/admin`, dashboard KPI cards not opening the right views, missing product cost so profitability was only estimated, Prisma/schema sync, mobile admin menu behavior, and VPS deployment with PM2 and Caddy. I fixed them one by one and deployed to a VPS at **onestoreofficiel.com**. This series is the story of what I built, how I built it, what broke, and how I fixed it.

---

## 2. Full Episode List

| # | Episode | Focus | Duration |
|---|---------|--------|----------|
| 1 | What I Built (and Why) | Intro, problem, scope | 45–60s |
| 2 | The Tech Stack and Architecture | Frontend, backend, DB, auth, i18n, deploy | 60s |
| 3 | The Storefront | Customer-facing experience | 45–60s |
| 4 | The Admin Dashboard | Backoffice, KPIs, product analytics | 60s |
| 5 | The Hardest Bugs I Faced | Real bugs and why they hurt | 60–90s |
| 6 | Deployment and VPS (PM2, Caddy) | Production on a real server | 60s |
| 7 | Image Upload and Product Media | Upload flow, 404s, route handler | 60s |
| 8 | Multilingual + RTL + Routing Fixes | next-intl, /ar/ar bug, RTL | 60s |
| 9 | Analytics and Product Profitability | Cost fields, OrderItem.costPrice, exact vs estimated | 60s |
| 10 | Lessons Learned and Final Result | Takeaways, advice, result | 45–60s |
| *Bonus* | Optional: Ads Platform / RBAC | Deeper dives | 45–60s each |

---

## 3. Episode-by-Episode Scripts

---

### Episode 1 — What I Built (and Why)

**Episode title:** I Built a Full-Stack E-Commerce Platform — Here’s Why

**Hook:**  
“This isn’t a tutorial project. I built a real e-commerce platform from scratch and deployed it to production.”

**Full speaking script:**

I wanted something I could actually run as a business: a storefront where customers can browse and buy, and a full admin dashboard where I manage products, orders, and see real numbers. So I built **One Store**.

It’s a full-stack e-commerce app: public storefront, separate admin backoffice, multiple languages — English, French, and Arabic with proper RTL. I’ve got product management with variants — like TV sizes with their own SKU, price, and stock — orders with Stripe and PayPal, image and video uploads, and analytics that show revenue and profit per product. When I say profit, I mean real profit: I added cost fields and snapshot cost on each order line so the dashboard can show exact margin where we have cost, and “estimated” where we don’t.

I also have an ads platform inside the admin — campaigns, creatives, targeting — and everything is permission-based so different roles see different parts of the dashboard. So it’s not “just CRUD.” It’s a real system with real problems that I had to debug and fix before it could go live.

**Suggested visual ideas / B-roll:**  
- Quick cuts: storefront homepage → product page → cart → checkout.  
- Admin login → dashboard with KPI cards → products list → one product edit.  
- Language switcher (en / fr / ar) and one Arabic page with RTL.  
- Short screen recording of one order flow.

**Suggested on-screen text:**  
- “One Store — Full-stack e-commerce”  
- “Storefront + Admin + i18n + Analytics”  
- “Real profit, not just revenue”

**Suggested caption:**  
I built a full-stack e-commerce platform with storefront, admin, multilingual (EN/FR/AR), product variants, Stripe/PayPal, uploads, and per-product profitability. This is the story of how it was built and what broke (and how I fixed it). #buildinpublic #ecommerce #fullstack #nextjs

**Suggested CTA:**  
“Next episode: the exact tech stack I used and why.”

**Duration target:** 45–60 seconds  
**Best style:** Storytelling + educational

---

### Episode 2 — The Tech Stack and Architecture

**Episode title:** The Exact Tech Stack I Used to Build My E-Commerce Platform

**Hook:**  
“People keep asking what I used. Here’s the full stack — frontend, backend, database, and how it all fits together.”

**Full speaking script:**

On the frontend I use **Next.js 16** with **React 19**, **TypeScript**, and **Tailwind**. For UI I went with **Radix** and **Framer Motion** so the storefront and admin feel solid and responsive. For forms I use **React Hook Form** and **Zod** so validation lives in one place.

The backend is the same Next.js app: **server actions** for mutations and data loading, **API routes** for uploads and webhooks. Database is **MongoDB** with **Prisma** as the ORM — one schema for users, admins, products, variants, orders, order items, categories, brands, and the ads models. Auth is **NextAuth v5**: one strategy for customers (email/password), a separate one for admins (username/password and RBAC with permission keys like products.read, orders.write).

For internationalization I use **next-intl**: three locales — en, fr, ar — with proper RTL for Arabic. Uploads go to the filesystem — a dedicated folder — and I serve them with a **custom route handler** under `/uploads/[...path]` so Next.js doesn’t try to optimize them and break things. Images are processed with **Sharp** — resize, WebP, consistent size for product images.

For deployment I run **Node** on a **VPS**, **PM2** to keep the app running and restart on crash, and **Caddy** as reverse proxy for HTTPS. So: Next.js, React, TypeScript, Prisma, MongoDB, NextAuth, next-intl, Sharp, PM2, Caddy. That’s the stack.

**Suggested visual ideas / B-roll:**  
- Split screen or list: Next.js, React, Prisma, MongoDB, NextAuth, next-intl.  
- Quick diagram: Browser → Caddy → Node/PM2 → Next.js → MongoDB.  
- `package.json` or `prisma/schema.prisma` on screen briefly.

**Suggested on-screen text:**  
- “Next.js 16 + React 19 + TypeScript”  
- “Prisma + MongoDB”  
- “NextAuth v5 (customers + admin)”  
- “next-intl (en, fr, ar + RTL)”

**Suggested caption:**  
Full stack for One Store: Next.js 16, React 19, TypeScript, Tailwind, Radix, Prisma, MongoDB, NextAuth v5, next-intl, Sharp, PM2, Caddy. One codebase, storefront + admin. #techstack #nextjs #prisma #fullstack

**Suggested CTA:**  
“Next: how the storefront is built and what it does for customers.”

**Duration target:** 60 seconds  
**Best style:** Technical + educational

---

### Episode 3 — The Storefront

**Episode title:** How the Storefront Works — Products, Cart, Checkout

**Hook:**  
“The customer-facing side had to be fast, clear, and work in three languages. Here’s how I built it.”

**Full speaking script:**

The storefront is the same Next.js app under `[locale]` — so every page is localized. Customers see the homepage, categories, brands, and product pages. Products can be simple — one price, one stock — or they can have **variants**, like TVs with sizes: 43, 55, 65 inches, each with its own SKU, price, and stock.

I use **next-intl** for all copy: product titles, descriptions, and UI strings come from JSON per locale. Arabic gets `dir="rtl"` and the layout flips so it feels natural. The cart is in state — I use **Zustand** — and when they checkout we create a **pending order** in the database with the items, then send them to Stripe or PayPal. When payment succeeds, we update the order to PAID and send confirmation. So the flow is: browse → cart → checkout → payment → order confirmed.

On the product page we also store cost when we create the order: each order line gets the sale price and, when I have it, the cost at that moment. That’s what makes “exact profit” possible later in the admin. The storefront is responsive, and all the important actions — add to cart, change variant, go to checkout — work the same in every language.

**Suggested visual ideas / B-roll:**  
- Scroll through homepage, then a category, then a product with size selector.  
- Add to cart, open cart drawer, go to checkout.  
- Switch language to Arabic, show RTL product page.

**Suggested on-screen text:**  
- “Locale in every URL: /en/, /fr/, /ar/”  
- “Variants = sizes, SKU, price, stock”  
- “Cart → Pending order → Stripe/PayPal → PAID”

**Suggested caption:**  
Storefront: localized routes, product variants (e.g. TV sizes), cart with Zustand, checkout with Stripe/PayPal, and order creation with cost snapshot for real profit later. #ecommerce #nextjs #storefront

**Suggested CTA:**  
“Next episode: the admin dashboard and how the KPIs and product analytics work.”

**Duration target:** 45–60 seconds  
**Best style:** Educational + storytelling

---

### Episode 4 — The Admin Dashboard

**Episode title:** The Admin Dashboard — KPIs, Orders, Products, and Product Analytics

**Hook:**  
“The admin side is where the real work happens. Here’s what’s in the dashboard and how it’s wired.”

**Full speaking script:**

The admin is a separate area — same app, but behind admin auth. You log in with username and password; NextAuth uses a different provider and session type so customers and admins never mix. The sidebar is permission-based: you only see Orders, Products, Customers, Categories, Brands, Ads, Settings if your role has the right permission keys.

The **dashboard** is a grid of KPI cards: revenue, profit estimate, sales count, total stock, then order-status cards — pending, paid, shipped, delivered, failed — and cancelled, refunded, average order value, low stock. The important fix was: each card had to **open the right place**. Revenue and profit should go to product analytics with the right view — revenue or profit. Orders cards should go to the orders list with the right filter — payment status or order status. Stock and low stock should go to the product analytics stock and low-stock views. So I made every card a **Link** with the correct path and query params, and I used the **next-intl Link** with paths *without* the locale prefix so the router prepends it once. That way we never got duplicate locale in the URL — no more `/ar/ar/admin`.

From the dashboard you drill into orders (with filters), products (create, edit, variants, cost), and a dedicated **product analytics** page: revenue by product, profit by product, stock, and low stock, with exact vs estimated profit where we have cost. So the dashboard is the hub; the cards are the entry points to the right details.

**Suggested visual ideas / B-roll:**  
- Admin login → dashboard with all KPI cards.  
- Click Revenue card → product analytics revenue tab.  
- Click Orders (e.g. Pending) → orders list filtered.  
- Product list → edit product → variant modal with cost field.

**Suggested on-screen text:**  
- “KPI cards = links to the right place”  
- “Revenue/Profit → /admin/analytics/products?view=…”  
- “Orders → /admin/orders?status=… or ?paymentStatus=…”

**Suggested caption:**  
Admin dashboard: KPI grid with correct links to product analytics and orders. Permission-based sidebar, product management with cost, and product-level analytics (revenue, profit, stock, low stock). #admin #dashboard #analytics

**Suggested CTA:**  
“Next: the hardest bugs I hit and how I fixed them.”

**Duration target:** 60 seconds  
**Best style:** Technical + educational

---

### Episode 5 — The Hardest Bugs I Faced

**Episode title:** The Bugs That Almost Broke My E-Commerce Platform (And How I Fixed Them)

**Hook:**  
“Not everything worked the first time. Here are the bugs that cost me the most time and what I did about them.”

**Full speaking script:**

First: **product images not showing** — 404. I was saving URLs like `/uploads/filename.webp` and the image component was sometimes getting paths with a double prefix or without a leading slash. So the browser asked for the wrong URL. I added a **normalizeImageUrl** helper: if it’s already absolute or external, leave it; if it’s relative or a bare filename, turn it into exactly one `/uploads/...` path; and if something was double-prefixed like `/uploads/uploads/...`, collapse it to one. Every place we display product images now uses that helper, and we’re consistent.

Second: **Next.js image optimization** was returning 400 for some uploads. So I decided uploads shouldn’t go through the optimizer. I serve them with a **route handler** at `src/app/uploads/[...path]/route.ts` that reads from the upload folder and returns the file with the right content-type. And in the image component, when the src is `/uploads/` or blob, we **skip optimization** so Next.js doesn’t touch those URLs.

Third: **duplicate locale in the URL** — like `/ar/ar/admin/dashboard`. That happened when I used a normal Link with a path that already had the locale. The next-intl router prepends the locale, so I was doubling it. Fix: use the **Link from next-intl** and pass paths *without* the locale — e.g. `/admin/orders` — and let the framework add the locale once. I did that everywhere in the dashboard and sidebar.

Fourth: **dashboard KPI cards** — clicking Revenue or Profit didn’t open product analytics; it went to the wrong page or no page. I wired each card to a proper **href** — Revenue and Profit to `/admin/analytics/products` with `?view=revenue` or `?view=profit`, and the same for stock and low stock. So the cards became real entry points.

Fifth: **profit was always “estimated.”** The schema had cost on variants but not on the base product, and we weren’t storing cost on the order line. So historically we couldn’t show real profit. I added **Product.cost**, **OrderItem.costPrice**, and in **create-pending-order** I set costPrice from the product or variant when creating each line. In the admin I added cost fields to the product and variant forms and made the analytics page use OrderItem.costPrice when present — that’s “exact” — and current product/variant cost as fallback; otherwise we show “estimated” or “—”. So the hardest bugs were: wrong image URLs, Next.js serving uploads wrong, duplicate locale, wrong KPI links, and missing cost for real profitability. I fixed all of them.

**Suggested visual ideas / B-roll:**  
- Before: broken product image. After: same page with image loading.  
- Before: URL bar showing /ar/ar/admin. After: /ar/admin/dashboard.  
- Dashboard: click Revenue → product analytics.  
- Product analytics table: “Exact” vs “Estimated” badges.

**Suggested on-screen text:**  
- “Bug 1: Image 404 → normalizeImageUrl + route handler”  
- “Bug 2: Duplicate /ar/ar → Link without locale”  
- “Bug 3: KPI cards → correct href + query”  
- “Bug 4: No real profit → Product.cost + OrderItem.costPrice”

**Suggested caption:**  
Real bugs I fixed: product images 404 (URL normalizer + upload route handler), duplicate locale /ar/ar (next-intl Link without locale), KPI cards not opening the right page (correct hrefs), and no exact profit (cost on product/variant + costPrice on order line). #debugging #nextjs #ecommerce

**Suggested CTA:**  
“Next: how I deployed this on a VPS with PM2 and Caddy.”

**Duration target:** 60–90 seconds  
**Best style:** Technical + storytelling (problem → diagnosis → fix)

---

### Episode 6 — Deployment and VPS (PM2, Caddy)

**Episode title:** How I Deployed My E-Commerce App on a VPS (PM2 + Caddy)

**Hook:**  
“It’s one thing to run it locally. Deploying on a real server was a whole other level. Here’s how I did it.”

**Full speaking script:**

I deploy on a **VPS** — a single server that already runs other sites with **Caddy**. I didn’t want to break anything, so I didn’t replace Caddy; I added a new block only for my domain. The app runs as a **Node** process on an internal port — 3001 — and Caddy proxies 80 and 443 to that port. So the outside world only talks to Caddy; Caddy talks to the app.

I use **PM2** to run the Next.js app: `next start` in production. PM2 keeps it alive, restarts on crash, and I set up `pm2 startup` so it comes back after a server reboot. I have an **ecosystem config** so the app loads env from `.env.production` and listens on 3001. No secrets in the repo — I create `.env.production` on the server with DATABASE_URL, AUTH_SECRET, AUTH_URL, and any API keys. The database is **MongoDB** — I run it in Docker on the same host, and the app connects with DATABASE_URL. After I push code I build on the server — or I build locally and upload the built output — then `pm2 restart onestore`. Caddy gets a new block for my domain with `reverse_proxy 127.0.0.1:3001`, I validate the config and reload Caddy, and that’s it. So: VPS, Node, PM2, Caddy, MongoDB in Docker. No Kubernetes, no complex DevOps — just one app, one process, one reverse proxy. If something goes wrong I check PM2 logs and Caddy logs. It’s been stable.

**Suggested visual ideas / B-roll:**  
- Terminal: `pm2 list`, `pm2 logs`.  
- Caddyfile snippet with reverse_proxy.  
- Browser: https://onestoreofficiel.com loading.

**Suggested on-screen text:**  
- “VPS → Node (port 3001) → PM2”  
- “Caddy: 80/443 → 3001”  
- “.env.production only on server”

**Suggested caption:**  
Deployment: VPS, Node.js, PM2 for process management, Caddy as reverse proxy for HTTPS. App on 3001, MongoDB in Docker. No overkill — just stable and simple. #deployment #vps #pm2 #caddy

**Suggested CTA:**  
“Next: how image uploads work and why they were returning 404.”

**Duration target:** 60 seconds  
**Best style:** Technical + educational

---

### Episode 7 — Image Upload and Product Media

**Episode title:** How I Fixed Image Uploads and Product Images (404 → Working)

**Hook:**  
“Product images were 404ing. Here’s what was wrong and exactly how I fixed it.”

**Full speaking script:**

Uploads are a classic pain point. In my case we had two separate issues: **where files live** and **how we build the URL**.

Files go to a fixed folder on disk — on the server it’s something like `C:\onestore-data\uploads` or the Linux equivalent. The upload API receives the file, checks size and type, and for images it uses **Sharp** to resize and convert to WebP so we have a consistent size and format. Then it writes the file to that folder and returns a URL like `/uploads/product-123456.webp`. The frontend stores that URL on the product. So far so good.

The first problem was **serving**. Next.js by default doesn’t serve arbitrary files from a custom folder. And when we used the Next.js Image component with `/uploads/...` src, the image optimizer sometimes failed and returned 400. So I added a **route handler**: `src/app/uploads/[...path]/route.ts`. That’s a dynamic route that takes the path after `/uploads/`, resolves it inside the uploads directory with a security check so we can’t escape outside that folder, and returns the file with the correct content-type and cache headers. So `/uploads/anything.webp` is served by our code, not by the static pipeline. And in the image component we **skip optimization** for any src that starts with `/uploads/` or is a blob — so Next.js doesn’t try to optimize those and break them.

The second problem was **inconsistent URLs** in the database and in the UI: sometimes we had a leading slash, sometimes not, sometimes we accidentally had `/uploads/uploads/`. So I added a small **normalizeImageUrl** helper: it normalizes everything to a single canonical form — one `/uploads/` prefix, no double prefix — and every place we display a product image uses it. After that, no more 404s for product images. So: upload to disk with Sharp, serve via custom route, and normalize every URL before display.

**Suggested visual ideas / B-roll:**  
- Admin: upload image on product form → see preview.  
- Save product → storefront product page with image loading.  
- Quick code peek: upload route handler and normalizeImageUrl (optional).

**Suggested on-screen text:**  
- “Upload → Sharp (WebP) → disk”  
- “Serve: /uploads/[...path] route handler”  
- “Display: normalizeImageUrl → one /uploads/ path”

**Suggested caption:**  
How I fixed product image 404s: custom route handler for /uploads/, skip Next.js image optimization for uploads, and a normalizeImageUrl helper so we never double-prefix or miss a slash. #nextjs #uploads #sharp

**Suggested CTA:**  
“Next: multilingual and the duplicate locale bug.”

**Duration target:** 60 seconds  
**Best style:** Technical + educational

---

### Episode 8 — Multilingual + RTL + Routing Fixes

**Episode title:** Multilingual (EN, FR, AR) and the Duplicate Locale Bug I Fixed

**Hook:**  
“Three languages, RTL for Arabic, and one routing bug that broke every admin link. Here’s the fix.”

**Full speaking script:**

I use **next-intl** for i18n. Three locales: **en**, **fr**, **ar**. Every page lives under `[locale]`, so the URL is always like `/en/products` or `/ar/admin/dashboard`. Translations live in JSON files per locale — en.json, fr.json, ar.json — and we use `useTranslations` for every string so the UI is fully translated. For Arabic we set `dir="rtl"` on the container so the whole layout flips. The admin sidebar on mobile uses a sheet that opens from the **right** in RTL and from the **left** in LTR, so it feels natural in both directions.

The bug that got me was **duplicate locale in the URL**. I had links like `/${locale}/admin/orders` — so for Arabic that’s `/ar/admin/orders`. But the next-intl **Link** component also prepends the locale. So when you used that Link with href `/ar/admin/orders`, the router produced `/ar/ar/admin/orders`. Same for the dashboard: every KPI link was built with the locale already in it, so we got `/ar/ar/admin/analytics/products`. The fix was to **never put the locale in the path** when using the next-intl Link. I changed every admin link to use paths like `/admin/orders`, `/admin/analytics/products?view=profit`. The framework’s Link adds the current locale once, so we get `/ar/admin/orders` and `/ar/admin/analytics/products?view=profit`. One locale, no duplicate. I also made sure the sidebar and any other nav use the same pattern. So: next-intl for routes and translations, RTL for Arabic, and always pass path-without-locale to Link so we never double the locale again.

**Suggested visual ideas / B-roll:**  
- Language switcher: en → fr → ar.  
- Arabic product page with RTL layout.  
- Before: URL /ar/ar/admin. After: /ar/admin/dashboard.  
- Mobile admin: open sidebar from correct side in RTL.

**Suggested on-screen text:**  
- “next-intl: en, fr, ar”  
- “Bug: /ar/ar/admin → Fix: Link without locale”  
- “RTL: dir=rtl, sidebar side”

**Suggested caption:**  
Multilingual with next-intl (en, fr, ar) and RTL for Arabic. The duplicate locale bug (/ar/ar/admin) was fixed by using Link with paths without locale so the router prepends it once. #i18n #nextjs #rtl

**Suggested CTA:**  
“Next: how analytics and product profitability work with exact vs estimated.”

**Duration target:** 60 seconds  
**Best style:** Technical + educational

---

### Episode 9 — Analytics and Product Profitability

**Episode title:** How I Added Real Product Profitability (Exact vs Estimated)

**Hook:**  
“Revenue is easy. Real profit per product was the hard part. Here’s how I did it.”

**Full speaking script:**

At first the dashboard had revenue and a **global** profit estimate — like 25% of revenue — but no per-product profit. To get real profit per product I needed **cost** at the product or variant level, and I needed to **snapshot it on the order** so historical reports don’t change when I update today’s cost.

So I added **Product.cost** for simple products and kept **ProductVariant.cost** for variants. In the admin product form I added a cost field — and in the variant details modal I added cost per size. When we create an order, in **create-pending-order** we set **OrderItem.costPrice** from the product or variant cost at that moment. So every order line has the sale price and, when we had it, the cost. That’s the snapshot.

In the **product analytics** page we have tabs: revenue by product, profit by product, stock, low stock. For profit we query order items from PAID orders in the selected period. For each row we have revenue — quantity times price — and cost. When **OrderItem.costPrice** is set we use it: that’s **exact** profit and we show an “Exact” badge. When it’s null we can use current product or variant cost if we have it — still exact for that row — or we show “Estimated” or “—” when there’s no cost at all. So the UI is honest: exact when we have real cost, estimated when we don’t. The KPI cards on the dashboard link to this page with the right view — revenue, profit, stock, low stock — so one click from the dashboard takes you to the right breakdown. That’s how product profitability works: cost on product and variant, costPrice on the order line, and analytics that show the difference.

**Suggested visual ideas / B-roll:**  
- Product form: cost field. Variant modal: cost field.  
- Product analytics page: Revenue tab then Profit tab.  
- Table row with “Exact” badge vs “Estimated”.  
- Dashboard: click Profit card → product analytics profit view.

**Suggested on-screen text:**  
- “Product.cost + Variant.cost”  
- “OrderItem.costPrice = snapshot at order time”  
- “Exact vs Estimated in UI”

**Suggested caption:**  
Real product profitability: cost on product/variant, costPrice snapshot on OrderItem, and analytics that show exact profit when we have cost and “estimated” when we don’t. #analytics #ecommerce #profitability

**Suggested CTA:**  
“Last episode: what I learned and the final result.”

**Duration target:** 60 seconds  
**Best style:** Technical + educational

---

### Episode 10 — Lessons Learned and Final Result

**Episode title:** What I Learned Building and Shipping This E-Commerce Platform

**Hook:**  
“After building, debugging, and deploying — here’s what I’d do again and what I’d do differently.”

**Full speaking script:**

The main lesson: **document the real problems.** When something breaks — images 404, wrong URLs, wrong links — write down what you did to fix it. That’s how we got the normalizeImageUrl helper, the uploads route handler, and the rule to never put locale in Link hrefs. Next time I’ll do that from day one.

Second: **schema and data flow matter.** Adding Product.cost and OrderItem.costPrice wasn’t just “one more field” — it changed how we create orders and how we report. So think about what you need to report before you lock the schema.

Third: **deployment is part of the product.** PM2 and Caddy and a single VPS were enough for me. I didn’t need Kubernetes or a big DevOps setup. Know what you need and don’t over-engineer.

The result: a **storefront** that works in three languages with RTL, a **admin dashboard** with KPIs that actually open the right pages, **product management** with variants and cost, **orders** with Stripe and PayPal, **uploads** that work and don’t 404, and **product analytics** with real exact vs estimated profit. It’s live, it’s stable, and it’s something I can improve over time. If you’re building something similar, start with a clear data model, fix the first bugs properly so they don’t come back, and ship. You’ve got this.

**Suggested visual ideas / B-roll:**  
- Short montage: storefront, admin dashboard, product analytics, one order, one upload.  
- You on camera for the “you’ve got this” line.

**Suggested on-screen text:**  
- “Document the fix”  
- “Schema + data flow first”  
- “Deploy simple: PM2 + Caddy”  
- “Result: live, stable, improvable”

**Suggested caption:**  
Lessons from building One Store: document fixes, design schema for reporting, keep deployment simple. Result: storefront + admin + analytics + real profit, live and stable. #buildinpublic #lessons #ecommerce

**Suggested CTA:**  
“Full series in my playlist — from stack to deployment to bugs. Link in bio.”

**Duration target:** 45–60 seconds  
**Best style:** Motivational + storytelling

---

## 4. Optional Bonus Episode Ideas

- **Ads platform:** Campaigns, creatives, targeting rules, placements, and how it’s wired in the admin (without inventing features — only what exists in the schema and UI).
- **RBAC and permissions:** How admin roles and permission keys (e.g. products.read, orders.write) control sidebar and actions, and how I seed roles.
- **Orders flow deep dive:** From cart to createPendingOrder, Stripe/PayPal session, webhook or callback, and updating order status and sending emails.
- **Prisma + MongoDB:** Why I chose MongoDB with Prisma, schema design (relations, indexes), and one or two schema sync / migration stories if you have them.

---

## 5. How to Record and Post the Series

**Order to record (recommended):**  
1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10.  
This order tells the full story: what it is, how it’s built, storefront, admin, bugs, deployment, uploads, i18n, profitability, lessons.

**Posting options:**  
- **Daily:** One episode per day so the series runs ~10 days.  
- **Twice per week:** Stretch to ~5 weeks; good if you want to add B-roll and edits.  
- **Playlist:** Publish all, then put them in a “One Store build series” playlist and pin the first or the “final result” episode.

**Recording tips:**  
- Use the **hook** as your first line to camera; then cut to B-roll or screen where it helps.  
- Keep **on-screen text** short and in sync with what you say.  
- For technical episodes (2, 5, 6, 7, 8, 9), short code or config peeks (without secrets) add credibility.  
- End each episode with the **CTA** so viewers know what’s next.

**Reuse:**  
- Same scripts can be adapted for YouTube Shorts, TikTok, and Reels by trimming to 30–45s if needed (hook + 2–3 main points + CTA).  
- You can also turn the series into one long “documentary” cut (e.g. 10–15 minutes) for YouTube with the same content in order.

---

*End of script series. All references (stack, schema, routes, bugs, fixes) are based on the actual One Store codebase and docs.*
