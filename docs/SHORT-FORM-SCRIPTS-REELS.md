# One Store — Short-Form Scripts (Reels / TikTok / Shorts)

**Camera-ready. First person. Hook → Body → Closing → CTA. Same story, same technical truth.**

---

## How to Use This Doc

- **A = 30s** | **B = 45s** | **C = 60s** per episode.
- **EN** = polished English. **Hybrid** = creator-style (more casual, "real talk").
- **FR / Darija** = French + Moroccan Darija spoken versions at the end.
- Speak in short sentences. Pause on the hook. One idea per line.

---

# EPISODE 1 — What I Built (and Why)

---

## English — Version A (30s)

**Hook:**  
This isn’t a tutorial. I built a real e-commerce platform and put it in production.

**Body:**  
Storefront. Admin dashboard. Three languages — English, French, Arabic with RTL. Products with variants. Stripe and PayPal. Real profit per product, not just revenue. I added cost and snapshot it on every order.

**Closing:**  
It’s not “just CRUD.” I debugged real bugs to get here.

**CTA:**  
Next: the exact stack I used.

---

## English — Version B (45s)

**Hook:**  
This isn’t a tutorial project. I built a full e-commerce platform and deployed it for real.

**Body:**  
I wanted something I could run as a business. Storefront where people buy. Admin where I manage products, orders, and numbers. Three languages — EN, FR, AR with RTL. Product variants — like TV sizes, each with its own SKU and price. Stripe and PayPal. Image uploads. And real profit: I store cost on products and on each order line so the dashboard shows exact margin when we have it, and “estimated” when we don’t.

**Closing:**  
So it’s a real system. With real bugs I had to fix.

**CTA:**  
Next episode: the full tech stack.

---

## English — Version C (60s)

**Hook:**  
This isn’t a tutorial. I built a real e-commerce platform from scratch and it’s live.

**Body:**  
I wanted something I could actually run. Storefront for customers. Full admin for me — products, orders, analytics. Three languages: English, French, Arabic with proper RTL. Products can have variants — like TV sizes with their own SKU, price, stock. Orders with Stripe and PayPal. Uploads for images and video. And real profitability: I added cost on products and variants, and I snapshot cost on every order line. So the dashboard shows exact profit when we have cost, “estimated” when we don’t. There’s an ads platform in the admin too. Permission-based. Different roles see different things.

**Closing:**  
So it’s not “just CRUD.” I hit real bugs and fixed them before going live.

**CTA:**  
Next: the exact stack — frontend, backend, database, deploy.

---

## Hybrid Creator-Style (45s)

**Hook:**  
Real talk — this isn’t a course project. I built an e-commerce app and shipped it.

**Body:**  
Storefront, admin, three languages. Variants, Stripe, PayPal, uploads. And real profit — I store cost and snapshot it on orders so we see exact margin. Had to fix a bunch of stuff: images 404, wrong URLs, duplicate locale. But now it’s live.

**Closing:**  
So yeah. Real product, real bugs, real fixes.

**CTA:**  
Next vid: the stack. Link below.

---

# EPISODE 2 — The Tech Stack

---

## English — Version A (30s)

**Hook:**  
Everyone asks what I used. Here’s the stack.

**Body:**  
Next.js 16, React 19, TypeScript, Tailwind. Radix and Framer Motion for UI. Backend: same app — server actions, API routes. Database: MongoDB with Prisma. Auth: NextAuth v5 — customers and admins separate. next-intl for en, fr, ar. Sharp for images. Uploads: custom route so Next.js doesn’t break them. Deploy: VPS, PM2, Caddy.

**Closing:**  
One codebase. Storefront and admin.

**CTA:**  
Next: how the storefront works.

---

## English — Version B (45s)

**Hook:**  
People keep asking what I used. Full stack — frontend, backend, database, deploy.

**Body:**  
Frontend: Next.js 16, React 19, TypeScript, Tailwind. Radix and Framer Motion. React Hook Form and Zod for forms. Backend is the same Next.js app — server actions for data, API routes for uploads and webhooks. Database: MongoDB. ORM: Prisma. One schema for users, products, orders, everything. Auth: NextAuth v5. Customers get email/password. Admins get username/password and permissions. i18n: next-intl — en, fr, ar, RTL for Arabic. Images: Sharp, WebP. I serve uploads with a custom route so the optimizer doesn’t break them. Deploy: VPS, Node, PM2, Caddy for HTTPS.

**Closing:**  
That’s the stack. One app, storefront and admin.

**CTA:**  
Next: storefront — cart, checkout, variants.

---

## English — Version C (60s)

**Hook:**  
“What did you use?” Here’s the full stack and how it fits together.

**Body:**  
Frontend: Next.js 16, React 19, TypeScript, Tailwind. Radix and Framer Motion so the UI feels solid. Forms: React Hook Form and Zod. Backend is the same app — server actions for mutations and data, API routes for uploads and webhooks. Database: MongoDB. Prisma as ORM — one schema for users, admins, products, variants, orders, categories, brands, ads. Auth: NextAuth v5. One strategy for customers, one for admins with permission keys. i18n: next-intl. Three locales — en, fr, ar — and RTL for Arabic. Uploads go to disk; I serve them with a custom route handler under slash uploads so Next.js doesn’t optimize them and break. Images processed with Sharp — resize, WebP. Deploy: VPS, Node on port 3001, PM2 to keep it running, Caddy as reverse proxy for HTTPS.

**Closing:**  
So: Next.js, Prisma, MongoDB, NextAuth, next-intl, Sharp, PM2, Caddy. One codebase.

**CTA:**  
Next episode: the storefront — products, cart, checkout.

---

## Hybrid Creator-Style (45s)

**Hook:**  
Stack check. What I actually used.

**Body:**  
Next.js 16, React 19, TypeScript, Tailwind. Prisma plus MongoDB. NextAuth for customers and admins — separate. next-intl for three languages and RTL. Sharp for images; custom route for uploads so they don’t 404. Deploy: one VPS, PM2, Caddy. No Kubernetes. Just one process.

**Closing:**  
That’s it. Simple and it works.

**CTA:**  
Next: storefront flow.

---

# EPISODE 3 — The Storefront

---

## English — Version A (30s)

**Hook:**  
The storefront had to work in three languages. Here’s how I built it.

**Body:**  
Every page is under locale — slash en, fr, ar. Products can be simple or have variants — like TV sizes, each with SKU and price. Cart in state with Zustand. Checkout creates a pending order, then Stripe or PayPal. When payment hits we set order to PAID. I also snapshot cost on each line so we get real profit later in admin.

**Closing:**  
Same flow in every language. Responsive.

**CTA:**  
Next: the admin dashboard and KPIs.

---

## English — Version B (45s)

**Hook:**  
Customer side: fast, clear, three languages. Here’s how.

**Body:**  
Storefront is the same Next.js app under locale. So every URL is localized. Homepage, categories, products. Products can be simple or have variants — TVs with 43, 55, 65 inches, each with its own SKU, price, stock. All copy from next-intl — JSON per locale. Arabic gets dir RTL so the layout flips. Cart: Zustand. Checkout: we create a pending order in the DB, then send them to Stripe or PayPal. When payment succeeds we update to PAID and send confirmation. And we store cost on each order line when we have it — that’s what makes exact profit possible in the admin.

**Closing:**  
One flow. Works the same in every language.

**CTA:**  
Next: admin dashboard — KPIs and where they link.

---

## English — Version C (60s)

**Hook:**  
The storefront had to be fast, clear, and work in three languages. Here’s how I did it.

**Body:**  
Same Next.js app, every page under locale. So the URL is always slash en, fr, or ar, then the path. Customers see homepage, categories, brands, product pages. Products can be simple — one price — or have variants like TV sizes: 43, 55, 65, each with SKU, price, stock. I use next-intl for all text — product titles, descriptions, buttons — from JSON per locale. Arabic gets dir RTL so the whole layout flips. Cart lives in state with Zustand. At checkout we create a pending order in the database, then redirect to Stripe or PayPal. When payment succeeds we set the order to PAID and send confirmation. And when we create the order we snapshot cost on each line so the admin can show exact profit later. Responsive. Same actions in every language.

**Closing:**  
So: browse, cart, checkout, payment, done. In en, fr, and ar.

**CTA:**  
Next: admin dashboard — KPIs, orders, product analytics.

---

## Hybrid Creator-Style (45s)

**Hook:**  
Storefront in three languages. How it works.

**Body:**  
Everything’s under locale. Products, variants like TV sizes. Cart with Zustand. Checkout: pending order, then Stripe or PayPal. We snapshot cost on each line so profit in the admin is real. RTL for Arabic. Same flow everywhere.

**Closing:**  
Clean and consistent.

**CTA:**  
Next: admin and the KPI links.

---

# EPISODE 4 — The Admin Dashboard

---

## English — Version A (30s)

**Hook:**  
Admin is where the real work happens. Here’s what’s in the dashboard.

**Body:**  
Separate area, same app. Admin auth — username, password. Sidebar is permission-based. Dashboard is a grid of KPI cards: revenue, profit, sales, stock, then pending, paid, shipped, delivered, failed, cancelled, refunded, AOV, low stock. Each card links to the right place — Revenue and Profit go to product analytics with the right view. Orders go to the orders list with the right filter. I use next-intl Link without locale in the path so we never get slash ar slash ar admin.

**Closing:**  
One dashboard. Every card goes where it should.

**CTA:**  
Next: the hardest bugs and how I fixed them.

---

## English — Version B (45s)

**Hook:** **Admin side is where the real work happens. Here’s the dashboard and how it’s wired.**

**Body:**  
Same app, separate area. You log in with username and password. NextAuth keeps customers and admins separate. Sidebar is permission-based — you only see what your role allows. Dashboard: grid of KPI cards. Revenue, profit, sales count, stock. Then order status — pending, paid, shipped, delivered, failed. Then cancelled, refunded, average order value, low stock. The fix that mattered: every card had to open the right place. Revenue and profit go to product analytics with the right view. Orders cards go to the orders list with the right filter. Stock and low stock go to product analytics. I made each card a Link with the correct path and query. And I used next-intl Link with paths without the locale prefix so the router adds it once. No more slash ar slash ar admin.

**Closing:** **From the dashboard you drill into orders, products, and product analytics. Cards are the entry points.**

**CTA:** **Next: the bugs that cost me the most time.**

---

## English — Version C (60s)

**Hook:** **The admin is where the real work happens. Here’s what’s in the dashboard and how it’s wired.**

**Body:** **Admin is a separate area — same app, different auth. Username and password. NextAuth uses a different provider so customers and admins never mix. Sidebar is permission-based. You only see Orders, Products, Customers, Categories, Brands, Ads, Settings if your role has the right keys. The dashboard is a grid of KPI cards. First row: revenue, profit estimate, sales count, total stock. Second: pending, paid, shipped, delivered, failed. Third: cancelled, refunded, average order value, low stock. The important fix: each card had to open the right place. Revenue and profit go to product analytics with the right view. Orders go to the orders list with the right filter. Stock and low stock go to product analytics. I made every card a Link with the correct href and query params. And I used the next-intl Link with paths without the locale — so the framework prepends it once. That killed the duplicate locale bug — no more slash ar slash ar admin. From there you drill into orders, products, and the product analytics page with revenue, profit, stock, low stock by product.**

**Closing:** **Dashboard is the hub. Cards are the entry points.**

**CTA:** **Next: the hardest bugs I faced and how I fixed them.**

---

## Hybrid Creator-Style (45s)

**Hook:** **Admin dashboard — and why every card had to link right.**

**Body:** **KPI grid: revenue, profit, orders, stock, low stock, all of it. I had a bug where clicking Revenue didn’t go to product analytics. And in Arabic we had slash ar slash ar in the URL. Fix: every card is a proper Link with the right path and query. And I use next-intl Link without the locale in the path so it only gets added once. Now Revenue opens product analytics, Orders opens the list with the right filter. Permission-based sidebar. Clean.**

**Closing:** **One dashboard, correct links.**

**CTA:** **Next: the bugs that hurt the most.**

---

# EPISODE 5 — The Hardest Bugs

---

## English — Version A (30s)

**Hook:** **Not everything worked first try. Here are the bugs that cost me the most.**

**Body:** **One: product images 404. Wrong URLs — double slash uploads or missing slash. I added a normalizeImageUrl helper and a custom route to serve uploads. Two: duplicate locale — slash ar slash ar admin. I was putting the locale in the Link. Fix: pass path without locale so next-intl adds it once. Three: KPI cards didn’t open the right page. I wired each card to the correct href and query. Four: no real profit. I added Product cost, OrderItem costPrice, and snapshot cost when creating orders. Now we show exact when we have cost, estimated when we don’t.**

**Closing:** **Four big bugs. Fixed all of them.**

**CTA:** **Next: how I deployed on a VPS.**

---

## English — Version B (45s)

**Hook:** **Nothing worked the first time. Here are the bugs that cost me the most and how I fixed them.**

**Body:** **First: product images 404. URLs were inconsistent — sometimes double prefix, sometimes no leading slash. I added a normalizeImageUrl helper so every display path is one slash uploads something. And I serve uploads with a custom route handler so Next.js doesn’t optimize them and return 400. Second: duplicate locale. We had slash ar slash ar admin. I was building links with the locale already in the path, and next-intl prepends it again. Fix: use next-intl Link with paths like slash admin orders — no locale. Framework adds it once. Third: dashboard cards. Clicking Revenue didn’t go to product analytics. I gave every card the right href and query. Fourth: profit was always “estimated.” No cost on the base product, no cost on the order line. I added Product.cost, OrderItem.costPrice, and in create-pending-order I set costPrice when creating each line. Now we show exact when we have it.**

**Closing:** **Four major bugs. Each one had a clear fix.**

**CTA:** **Next: deployment — VPS, PM2, Caddy.**

---

## English — Version C (60s)

**Hook:** **Not everything worked the first time. Here are the bugs that cost me the most and exactly how I fixed them.**

**Body:** **First: product images not showing — 404. I was saving URLs like slash uploads filename, but sometimes we had double prefix or no leading slash. I added a normalizeImageUrl helper: one canonical form for every URL. And I serve uploads with a custom route at app uploads bracket path — reads from the upload folder, returns the file with the right content-type. For images we skip Next.js optimization when the src is slash uploads so it doesn’t break. Second: duplicate locale. We had slash ar slash ar admin. I was using a Link with the locale already in the path; next-intl prepends it, so we doubled it. Fix: use next-intl Link and pass paths without locale. So slash admin orders, slash admin analytics products. Framework adds the locale once. Third: KPI cards. Clicking Revenue or Profit didn’t open product analytics. I wired each card to the correct href — analytics products with view revenue or profit, orders with the right filter. Fourth: profit was always estimated. No cost on the base product, no cost snapshot on the order line. I added Product.cost, OrderItem.costPrice, and in create-pending-order I set costPrice from product or variant when creating each line. Admin form has cost fields. Analytics shows exact when we have cost, estimated when we don’t.**

**Closing:** **So: image URLs, upload serving, duplicate locale, wrong KPI links, missing cost. Five problems, five fixes.**

**CTA:** **Next: how I deployed this on a VPS with PM2 and Caddy.**

---

## Hybrid Creator-Style (45s)

**Hook:** **Real talk — these bugs almost broke the project.**

**Body:** **Images 404: wrong URLs. Fixed with a normalizer and a custom route for uploads. Then slash ar slash ar in the URL — I was putting locale in the Link. Now I never put locale in the path; next-intl adds it. Dashboard cards: Revenue didn’t go to analytics. Fixed the hrefs. And profit was always estimated because we didn’t store cost. Added cost on product and variant, and costPrice on the order line when we create it. Now we have exact when we have cost.**

**Closing:** **Four big ones. All fixed.**

**CTA:** **Next: VPS and PM2.**

---

# EPISODE 6 — Deployment (VPS, PM2, Caddy)

---

## English — Version A (30s)

**Hook:** **Running it locally is one thing. Putting it on a real server is another. Here’s how I did it.**

**Body:** **VPS. The server already had Caddy for other sites. I didn’t replace it — I added a block for my domain. App runs on Node, port 3001. Caddy proxies 80 and 443 to 3001. PM2 runs next start — keeps it alive, restarts on crash. pm2 startup so it comes back after reboot. Env and secrets in .env.production on the server only. MongoDB in Docker. Build, pm2 restart, reload Caddy. Done.**

**Closing:** **No Kubernetes. One app, one process, stable.**

**CTA:** **Next: how I fixed image uploads and 404s.**

---

## English — Version B (45s)

**Hook:** **Local is easy. Production on a real server — that’s where it got real. Here’s my setup.**

**Body:** **I deploy on a VPS. The server already runs Caddy for other sites. I didn’t touch that — I added one block for my domain. The app runs as a Node process on port 3001. Caddy proxies 80 and 443 to 3001. So the internet talks to Caddy; Caddy talks to the app. I use PM2 to run next start. PM2 keeps it alive and restarts on crash. I ran pm2 startup so it comes back after a reboot. I have an ecosystem config so the app loads .env.production and listens on 3001. No secrets in the repo — I create .env.production on the server with DATABASE_URL, AUTH_SECRET, API keys. MongoDB runs in Docker on the same host. When I push I build, then pm2 restart onestore. Caddy: new block, reverse proxy to 3001, validate, reload.**

**Closing:** **One VPS, PM2, Caddy. Simple and it’s been stable.**

**CTA:** **Next: image uploads and why they were 404ing.**

---

## English — Version C (60s)

**Hook:** **It’s one thing to run it locally. Deploying on a real server was a whole other level. Here’s how I did it.**

**Body:** **I deploy on a VPS. The server already had Caddy for other sites. I didn’t want to break anything so I didn’t replace Caddy — I added a new block only for my domain. The app runs as a Node process on an internal port, 3001. Caddy proxies 80 and 443 to that port. So the outside world only talks to Caddy; Caddy talks to the app. I use PM2 to run the Next.js app — next start in production. PM2 keeps it alive, restarts on crash. I set up pm2 startup so it comes back after a server reboot. I have an ecosystem config so the app loads env from .env.production and listens on 3001. No secrets in the repo — I create .env.production on the server with DATABASE_URL, AUTH_SECRET, AUTH_URL, and any API keys. MongoDB runs in Docker on the same host. When I push code I build on the server — or build locally and upload — then pm2 restart onestore. Caddy gets a new block for my domain with reverse_proxy to 3001, I validate the config and reload Caddy. That’s it.**

**Closing:** **So: VPS, Node, PM2, Caddy, MongoDB in Docker. No Kubernetes. One app, one process, one reverse proxy. Stable.**

**CTA:** **Next: how image uploads work and why they were 404ing.**

---

## Hybrid Creator-Style (45s)

**Hook:** **Deploy was: one server, no overkill.**

**Body:** **VPS with Caddy already on it. I added a block for my domain. App on Node, port 3001. Caddy proxies to it. PM2 runs next start — restarts on crash, survives reboot. .env.production only on the server. MongoDB in Docker. Build, restart PM2, reload Caddy. Done.**

**Closing:** **No K8s. Just works.**

**CTA:** **Next: fixing image 404s.**

---

# EPISODE 7 — Image Upload and Product Media

---

## English — Version A (30s)

**Hook:** **Product images were 404ing. Here’s what was wrong and how I fixed it.**

**Body:** **Two issues. One: Next.js wasn’t serving files from my upload folder. And the image optimizer was returning 400 for some uploads. So I added a route handler — app uploads bracket path — that reads from the upload folder and returns the file with the right content-type. For display we skip optimization when the src is slash uploads. Two: inconsistent URLs — sometimes slash uploads uploads, sometimes no slash. I added normalizeImageUrl so every path is one slash uploads something. No more 404s.**

**Closing:** **Upload to disk with Sharp. Serve with our route. Normalize every URL.**

**CTA:** **Next: multilingual and the duplicate locale bug.**

---

## English — Version B (45s)

**Hook:** **Product images were 404ing. Two separate issues. Here’s the fix.**

**Body:** **Files go to a folder on disk. The upload API uses Sharp — resize, WebP — and saves there. It returns a URL like slash uploads filename. First problem: serving. Next.js doesn’t serve that folder by default. And the Image component’s optimizer was returning 400 for some uploads. So I added a route handler at app uploads bracket path. It takes the path after slash uploads, resolves it inside the uploads folder with a security check, and returns the file with the right content-type. And in the image component we skip optimization when the src is slash uploads or blob. Second problem: inconsistent URLs in the DB and UI — double prefix or no leading slash. I added normalizeImageUrl. One canonical form. Every place we show a product image uses it. After that, no more 404s.**

**Closing:** **Upload with Sharp. Serve with our route. Normalize before display.**

**CTA:** **Next: three languages and the slash ar slash ar bug.**

---

## English — Version C (60s)

**Hook:** **Product images were 404ing. Here’s what was wrong and exactly how I fixed it.**

**Body:** **Two issues. One: where files live and how we serve them. Files go to a fixed folder on disk. The upload API receives the file, checks size and type, and for images uses Sharp to resize and convert to WebP. Then it writes to that folder and returns a URL like slash uploads product something dot webp. But Next.js doesn’t serve arbitrary folders. And when we used the Image component with slash uploads, the optimizer sometimes returned 400. So I added a route handler — app uploads bracket dot dot dot path. It takes the path after slash uploads, resolves it inside the uploads directory with a security check, and returns the file with the right content-type. And we skip optimization for slash uploads and blob so Next.js doesn’t touch them. Second issue: inconsistent URLs. Sometimes we had slash uploads uploads, sometimes no leading slash. I added normalizeImageUrl — one canonical form — and every place we display a product image uses it. No more 404s.**

**Closing:** **So: upload to disk with Sharp, serve via custom route, normalize every URL before display.**

**CTA:** **Next: multilingual and the duplicate locale fix.**

---

## Hybrid Creator-Style (45s)

**Hook:** **Images 404. Classic. Here’s the fix.**

**Body:** **Uploads go to disk. Sharp for resize and WebP. But Next.js wasn’t serving them — and the optimizer was breaking some. So I added a route handler for slash uploads that reads the file and returns it. And we don’t optimize slash uploads in the Image component. Then URLs were messy — double prefix or no slash. normalizeImageUrl everywhere. One format. Done.**

**Closing:** **Upload, serve, normalize. No more 404.**

**CTA:** **Next: the locale bug.**

---

# EPISODE 8 — Multilingual + RTL + Routing

---

## English — Version A (30s)

**Hook:** **Three languages, RTL for Arabic, and one bug that broke every admin link.**

**Body:** **I use next-intl. En, fr, ar. Every page under locale. Arabic gets dir RTL so the layout flips. The bug: duplicate locale in the URL. We had slash ar slash ar admin. I was building links with the locale already in the path. next-intl Link also prepends the locale. So we doubled it. Fix: use next-intl Link with paths without locale. Slash admin orders. Slash admin analytics products. The framework adds the locale once. Sidebar and dashboard — same rule. One locale, no duplicate.**

**Closing:** **next-intl for routes and RTL. Never put locale in the path.**

**CTA:** **Next: real profit per product — exact vs estimated.**

---

## English — Version B (45s)

**Hook:** **Three languages, RTL for Arabic. And one routing bug that broke every admin link. Here’s the fix.**

**Body:** **next-intl. Three locales — en, fr, ar. Every page lives under locale. Translations in JSON per locale. Arabic: dir RTL so the whole layout flips. Mobile admin sidebar opens from the right in RTL, left in LTR. The bug: duplicate locale. We had slash ar slash ar admin dashboard. I had links like slash locale admin orders — so for Arabic that’s slash ar admin orders. But next-intl Link also prepends the locale. So we got slash ar slash ar. Fix: never put the locale in the path when using next-intl Link. Use slash admin orders, slash admin analytics products. The framework adds the current locale once. I did that everywhere — dashboard, sidebar. No more duplicate.**

**Closing:** **So: next-intl for routes and translations, RTL for Arabic. Always path without locale in Link.**

**CTA:** **Next: how I added real product profitability.**

---

## English — Version C (60s)

**Hook:** **Three languages, RTL for Arabic, and one bug that broke every admin link. Here’s what it was and how I fixed it.**

**Body:** **I use next-intl for i18n. Three locales: en, fr, ar. Every page lives under locale so the URL is always slash en or fr or ar then the path. Translations in JSON per locale. We use useTranslations for every string. For Arabic we set dir RTL on the container so the layout flips. The admin sidebar on mobile — the sheet opens from the right in RTL and left in LTR. The bug: duplicate locale. We had slash ar slash ar admin. I was building links with slash locale admin something — so for Arabic that’s slash ar admin. But the next-intl Link component also prepends the locale. So when we used that Link we got slash ar slash ar. Fix: never put the locale in the path. Use next-intl Link with paths like slash admin orders, slash admin analytics products question view profit. The framework adds the locale once. I changed every admin link to that pattern. Sidebar, dashboard, everywhere. One locale, no duplicate.**

**Closing:** **next-intl for routes and translations. RTL for Arabic. Path without locale in every Link.**

**CTA:** **Next: analytics and product profitability — exact vs estimated.**

---

## Hybrid Creator-Style (45s)

**Hook:** **The slash ar slash ar bug. Sounds funny. Broke every link.**

**Body:** **next-intl for en, fr, ar. RTL for Arabic. I was building hrefs with the locale already in them. So slash ar admin. But Link also adds the locale. So we got slash ar slash ar. Fix: pass path without locale. Slash admin orders. Link adds it once. Did that everywhere. Fixed.**

**Closing:** **Never put locale in the path. Let the framework do it.**

**CTA:** **Next: real profit per product.**

---

# EPISODE 9 — Analytics and Product Profitability

---

## English — Version A (30s)

**Hook:** **Revenue is easy. Real profit per product was the hard part.**

**Body:** **I needed cost on the product or variant. And I needed to snapshot it on the order so history doesn’t change when I edit cost today. So I added Product.cost and kept Variant.cost. In create-pending-order I set OrderItem.costPrice when we create each line. In the admin I added cost fields to the product and variant forms. In product analytics we have revenue and profit by product. When costPrice is set we show “Exact.” When it’s null we show “Estimated” or dash. Dashboard KPI cards link to this page with the right view.**

**Closing:** **Cost on product and variant. costPrice on the order line. Honest UI.**

**CTA:** **Last episode: what I learned and the result.**

---

## English — Version B (45s)

**Hook:** **Revenue is easy. Real profit per product — that was the hard part. Here’s how I did it.**

**Body:** **At first we had a global profit estimate. No per-product profit. To get real profit I needed cost at the product or variant level, and I needed to snapshot it on the order so when I change cost tomorrow the old orders don’t change. So I added Product.cost for simple products. Variant already had cost. In the admin form I added cost fields — product and variant. When we create an order, in create-pending-order we set OrderItem.costPrice from the product or variant at that moment. So every order line has price and, when we have it, cost. In product analytics we have tabs: revenue by product, profit by product, stock, low stock. For profit we use costPrice when it’s set — that’s “Exact.” When it’s null we use current cost if we have it, or show “Estimated” or dash. The dashboard KPI cards link here with the right view — revenue, profit, stock, low stock.**

**Closing:** **Cost on product and variant. costPrice snapshot on the order line. Exact when we have it, estimated when we don’t.**

**CTA:** **Last episode: lessons learned and the final result.**

---

## English — Version C (60s)

**Hook:** **Revenue is easy. Real profit per product was the hard part. Here’s how I did it.**

**Body:** **At first the dashboard had revenue and a global profit estimate — like 25% of revenue. No per-product profit. To get real profit I needed cost at the product or variant level, and I needed to snapshot it on the order so historical reports don’t change when I update cost. So I added Product.cost for simple products. Variant already had cost. In the admin I added cost to the product form and the variant details modal. When we create an order, in create-pending-order we set OrderItem.costPrice from the product or variant cost at that moment. So every order line has the sale price and, when we had it, the cost. That’s the snapshot. In the product analytics page we have tabs: revenue by product, profit by product, stock, low stock. For profit we query order items from PAID orders. When OrderItem.costPrice is set we use it — that’s “Exact” and we show the badge. When it’s null we can use current product or variant cost if we have it, or show “Estimated” or dash. So the UI is honest. The KPI cards on the dashboard link to this page with the right view so one click takes you to the right breakdown.**

**Closing:** **Cost on product and variant. costPrice on the order line. Analytics show exact when we have cost, estimated when we don’t.**

**CTA:** **Last episode: what I learned and the final result.**

---

## Hybrid Creator-Style (45s)

**Hook:** **Real profit per product. Not just revenue.**

**Body:** **I added cost on product and variant. When we create an order we set costPrice on each line — snapshot. So old orders don’t change when I edit cost today. In analytics we show “Exact” when we have costPrice, “Estimated” when we don’t. Dashboard cards link to the right view. Clean.**

**Closing:** **Cost plus snapshot. Honest numbers.**

**CTA:** **Last one: lessons and result.**

---

# EPISODE 10 — Lessons and Final Result

---

## English — Version A (30s)

**Hook:** **After building, debugging, and deploying — here’s what I’d do again.**

**Body:** **One: document the fix. When something breaks, write down what you did. That’s how we got the normalizer, the upload route, the locale rule. Two: schema and data flow matter. Adding cost and costPrice wasn’t “one more field” — it changed how we create orders and report. Three: deploy simple. PM2 and Caddy were enough. No Kubernetes. Result: storefront in three languages, admin with correct KPI links, real profit where we have cost. Live and stable.**

**Closing:** **Fix the first bugs properly. Then ship. You’ve got this.**

**CTA:** **Full series in the playlist. Link in bio.**

---

## English — Version B (45s)

**Hook:** **After building, debugging, and deploying — here’s what I learned and what the result is.**

**Body:** **First: document the real problems. When something breaks — images 404, wrong URLs, wrong links — write down what you did to fix it. That’s how we got normalizeImageUrl, the uploads route handler, the rule to never put locale in Link. Next time I’ll do that from day one. Second: schema and data flow matter. Adding Product.cost and OrderItem.costPrice wasn’t “one more field.” It changed how we create orders and how we report. Think about what you need to report before you lock the schema. Third: deployment is part of the product. PM2 and Caddy and one VPS were enough. I didn’t need Kubernetes. Know what you need. Don’t over-engineer. The result: storefront in three languages with RTL, admin with KPIs that open the right pages, product management with cost, orders with Stripe and PayPal, uploads that work, product analytics with exact and estimated profit. Live. Stable. Something I can improve over time.**

**Closing:** **If you’re building something similar: clear data model, fix the first bugs properly, ship. You’ve got this.**

**CTA:** **Full series in the playlist. Link in bio.**

---

## English — Version C (60s)

**Hook:** **After building, debugging, and deploying — here’s what I’d do again and what I’d do differently.**

**Body:** **Main lesson: document the real problems. When something breaks — images 404, wrong URLs, wrong links — write down what you did to fix it. That’s how we got the normalizeImageUrl helper, the uploads route handler, and the rule to never put locale in Link hrefs. Next time I’ll do that from day one. Second: schema and data flow matter. Adding Product.cost and OrderItem.costPrice wasn’t “one more field.” It changed how we create orders and how we report. So think about what you need to report before you lock the schema. Third: deployment is part of the product. PM2 and Caddy and a single VPS were enough for me. I didn’t need Kubernetes or a big DevOps setup. Know what you need. Don’t over-engineer. The result: a storefront that works in three languages with RTL, an admin dashboard with KPIs that actually open the right pages, product management with variants and cost, orders with Stripe and PayPal, uploads that work and don’t 404, and product analytics with real exact vs estimated profit. It’s live. It’s stable. It’s something I can improve over time. If you’re building something similar: start with a clear data model, fix the first bugs properly so they don’t come back, and ship.**

**Closing:** **You’ve got this.**

**CTA:** **Full series in my playlist — from stack to deployment to bugs. Link in bio.**

---

## Hybrid Creator-Style (45s)

**Hook:** **What I’d do again. And the result.**

**Body:** **Document the fix. Every time something broke I wrote it down. Normalizer, upload route, locale rule. Schema matters — cost and costPrice changed how we create orders and report. Deploy simple. PM2, Caddy, one VPS. No K8s. Result: storefront, admin, three languages, real profit, uploads that work. Live and stable.**

**Closing:** **Fix the first bugs right. Then ship. You’ve got this.**

**CTA:** **Playlist in bio.**

---

# FRENCH — Versions parlées (par épisode)

*Phrases courtes. Style naturel à l’oral. Même structure Hook / Corps / Chute / CTA.*

---

## Épisode 1 — Version 45s (FR)

**Hook:** C’est pas un projet de tuto. J’ai monté une vraie plateforme e-commerce et je l’ai mise en prod.

**Body:** Boutique côté client. Dashboard admin. Trois langues — anglais, français, arabe avec RTL. Produits avec variantes. Stripe et PayPal. Vrai profit par produit, pas juste le chiffre d’affaires. J’ai ajouté le coût et je le snapshot sur chaque commande.

**Chute:** C’est pas “juste du CRUD”. J’ai débuggé de vrais bugs pour en arriver là.

**CTA:** Prochaine vidéo : la stack exacte que j’ai utilisée.

---

## Épisode 5 — Version 45s (FR) — Les bugs

**Hook:** Tout n’a pas marché du premier coup. Les bugs qui m’ont coûté le plus de temps — et comment je les ai corrigés.

**Body:** Un : les images produits en 404. URLs incohérentes. J’ai fait un helper normalizeImageUrl et une route custom pour servir les uploads. Deux : le locale en double — slash ar slash ar admin. Je mettais le locale dans le Link. Fix : passer le path sans locale, next-intl l’ajoute une fois. Trois : les cartes KPI n’ouvraient pas la bonne page. J’ai branché chaque carte sur le bon href. Quatre : pas de vrai profit. J’ai ajouté Product.cost, OrderItem.costPrice, et je snapshot le coût à la création de la commande. Maintenant on affiche “Exact” quand on a le coût, “Estimé” sinon.

**Chute:** Quatre gros bugs. Tous corrigés.

**CTA:** Prochaine : le déploiement sur un VPS.

---

## Épisode 8 — Version 30s (FR) — Multilingue

**Hook:** Trois langues, RTL pour l’arabe. Un bug qui cassait tous les liens admin.

**Body:** next-intl. En, fr, ar. Le bug : on avait slash ar slash ar admin. Je mettais le locale dans le path du Link, et next-intl le préfixait aussi. Fix : Link avec path sans locale. Le framework ajoute le locale une fois. Sidebar et dashboard — même règle.

**Chute:** next-intl pour les routes et le RTL. Ne jamais mettre le locale dans le path.

**CTA:** Prochaine : le vrai profit par produit.

---

# DARIJA — Versions parlées (échantillon)

*Même contenu, ton naturel en darija marocaine. Tu peux mélanger avec du français technique (stack, 404, route, etc.).*

---

## Épisode 1 — Version 45s (Darija)

**Hook:** Hadi ma hiyaš projet mnem tutorial. Bnina vraie plateforme e-commerce w ddernaha f production.

**Body:** Storefront. Dashboard admin. Tlat lughat — anglais, français, 3arabi b RTL. Produits b variantes. Stripe w PayPal. Vrai profit par produit, ma hiyaš ghir revenue. Zdt cost w kan snapshotiw f chaque commande.

**Chute:** Ma hiyaš “just CRUD”. Debugina vrais bugs bach wselna l had l 7ala.

**CTA:** L vidéo jeya : exactement ašnu stack sté3mlna.

---

## Épisode 5 — Version 45s (Darija) — Les bugs

**Hook:** Kollši ma tbdāš mra w7da. L bugs li costewni bzzaf d l waqt — w kifac fixitom.

**Body:** Wa7ed : suwar l produits 404. URLs ma mconsistentinš. Drt normalizeImageUrl w route custom bach nserviw l uploads. Juj : locale f double — slash ar slash ar admin. Knt kat7ot l locale f Link. Fix : path bla locale, next-intl yzidha mra wa7da. Tlata : l cartes KPI ma katft7uš l page l mzyan. Wselt kol carte b l href l mzyan. Rab3a : profit ma kanoš vrai. Zdt Product.cost, OrderItem.costPrice, w kan snapshotiw cost f create d l commande. Daba kanbano “Exact” ila 3andna cost, “Estimé” ila ma 3andnaš.

**Chute:** Arb3a bugs kbarin. Kollhom fixina.

**CTA:** L vidéo jeya : deployment f VPS.

---

## Épisode 8 — Version 30s (Darija) — Locale

**Hook:** Tlat lughat, RTL pour l 3arabi. Bug wa7ed katkasser kol l liens admin.

**Body:** next-intl. En, fr, ar. L bug : 3andna slash ar slash ar admin. Knt kat7ot l locale f path d Link, next-intl yzidha. Fix : Link b path bla locale. L framework yzid l locale mra wa7da. Sidebar w dashboard — nfs l 9anun.

**Chute:** next-intl pour routes w RTL. 7ta l 7ta ma t7otš l locale f path.

**CTA:** Jeya : vrai profit par produit.

---

# QUICK REFERENCE — Hooks only (all episodes, EN)

1. This isn’t a tutorial. I built a real e-commerce platform and put it in production.  
2. Everyone asks what I used. Here’s the stack.  
3. The storefront had to work in three languages. Here’s how I built it.  
4. Admin is where the real work happens. Here’s what’s in the dashboard.  
5. Not everything worked first try. Here are the bugs that cost me the most.  
6. Running it locally is one thing. Putting it on a real server is another.  
7. Product images were 404ing. Here’s what was wrong and how I fixed it.  
8. Three languages, RTL for Arabic, and one bug that broke every admin link.  
9. Revenue is easy. Real profit per product was the hard part.  
10. After building, debugging, and deploying — here’s what I’d do again.

---

*End of short-form scripts. Use A/B/C by episode length. Use FR or Darija for local audience. Same story, same technical truth.*
