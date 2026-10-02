# One Store – Production deployment on VPS (Caddy)

Deploy **onestoreofficiel.com** on VPS `SERVER_IP` without affecting existing Caddy-hosted sites.

---

## 1. VPS audit (run on the server first)

**Why:** Ensure we know the current OS, Caddy setup, ports, and existing sites before changing anything.

SSH into the VPS, then run these commands and **save the output** for reference:

```bash
# ─── OS & basics ───
cat /etc/os-release
uname -a

# ─── Caddy ───
which caddy
caddy version
systemctl status caddy --no-pager 2>/dev/null || true
# Config location (common paths)
ls -la /etc/caddy/Caddyfile 2>/dev/null || ls -la /etc/Caddyfile 2>/dev/null || true

# ─── Current Caddy config (BACKUP before any edit) ───
sudo cp /etc/caddy/Caddyfile /etc/caddy/Caddyfile.bak.$(date +%Y%m%d-%H%M%S) 2>/dev/null || true
sudo cat /etc/caddy/Caddyfile
# Or if Caddy uses JSON:
# sudo cat /etc/caddy/caddy.json 2>/dev/null

# ─── Listening ports ───
sudo ss -tlnp | grep -E ':(80|443|3000|3001|27017)' || true
sudo ss -tlnp

# ─── Docker / Node / PM2 ───
docker --version 2>/dev/null || echo "Docker not installed"
docker compose version 2>/dev/null || docker-compose --version 2>/dev/null || echo "Docker Compose not found"
node -v 2>/dev/null || echo "Node not installed"
npm -v 2>/dev/null || echo "npm not found"
pm2 -v 2>/dev/null || echo "PM2 not installed"

# ─── Firewall ───
sudo ufw status 2>/dev/null || sudo iptables -L -n 2>/dev/null | head -30

# ─── Existing sites (from Caddyfile) ───
sudo grep -E '^\s*[a-zA-Z0-9].*\{|^\s*[a-zA-Z0-9].*$' /etc/caddy/Caddyfile 2>/dev/null || true
```

**What to check:**

- **OS:** Note if Debian/Ubuntu or other (affects package commands).
- **Caddy:** Version and path of Caddyfile. Do **not** overwrite it; we will only **append** a new block.
- **Ports:** Ensure **3001** (or your chosen app port) is **not** used. If 3001 is taken, pick another (e.g. 3002) and use it consistently below.
- **Existing domains:** Our new block will only add `onestoreofficiel.com` and `www.onestoreofficiel.com`. No changes to other server blocks.
- **Docker/Node/PM2:** If Node and PM2 are present, we use **Node.js + PM2**. If the VPS already runs other apps with Docker Compose and you prefer that, we can add a production Dockerfile later; this guide uses **Node + PM2** by default.

---

## 2. Project deployment strategy

**Decision: Node.js + PM2 (with optional MongoDB via Docker)**

| Criterion | Finding |
|-----------|--------|
| `docker-compose.yml` | Only defines **MongoDB** + init. No Next.js service, no Dockerfile for the app. |
| `package.json` | Standard Next.js: `build` → `next build`, `start` → `next start`. |
| Database | Prisma uses **MongoDB** (`provider = "mongodb"`). |

**Conclusion:**

- The existing Docker Compose is **not** production-ready for the app (no app container). Adding a Dockerfile + app service would be a larger change.
- **Node.js + PM2** fits the current repo and keeps integration with your existing Caddy setup simple: one internal port (e.g. **3001**), Caddy proxies 80/443 to it.
- **MongoDB** can stay in Docker (only the `mongo` service from your existing `docker-compose.yml`) so you don’t need to install Mongo on the host. The app will use `DATABASE_URL=mongodb://127.0.0.1:27017/onestore` (or your DB name).

**Chosen layout:**

- App directory: `/var/www/onestoreofficiel`
- App port: **3001** (internal only; Caddy listens 80/443)
- Process manager: **PM2** (restart on crash and on reboot)
- MongoDB: Docker (existing compose), or external MongoDB if you already have one

---

## 3. DNS records

Point the domain to your VPS so Caddy can issue certificates and serve the site.

**Required records (at your DNS provider):**

| Type | Name | Value | TTL |
|------|------|--------|-----|
| **A** | `@` | `SERVER_IP` | 300 (or 3600) |
| **A** | `www` | `SERVER_IP` | 300 (or 3600) |

Optional (recommended):

| Type | Name | Value | TTL |
|------|------|--------|-----|
| **AAAA** | `@` | *(your VPS IPv6 if you have one)* | 300 |
| **AAAA** | `www` | *(same IPv6)* | 300 |

**Check before going live:**

```bash
# From your laptop or the VPS
dig +short onestoreofficiel.com
dig +short www.onestoreofficiel.com
# Both should return SERVER_IP
```

Only enable the Caddy block for this domain **after** both resolve to `SERVER_IP`, so Caddy can obtain HTTPS without errors.

---

## 4. Deployment commands

Assume app path **`/var/www/onestoreofficiel`** and app port **3001**. Run as root or with sudo where needed.

### 4.1 Prepare directory and app files

```bash
sudo mkdir -p /var/www/onestoreofficiel
sudo chown "$USER:$USER" /var/www/onestoreofficiel
cd /var/www/onestoreofficiel
```

Upload your project (from your **local** machine, from the project root):

```bash
# From your Windows machine (PowerShell), in "c:\Users\one store":
scp -r .next package.json package-lock.json prisma public src next.config.ts tsconfig.json .env.production [[EMAIL REDACTED]]:/var/www/onestoreofficiel/
# Or use rsync (if available):
# rsync -avz --exclude node_modules --exclude .git . [[EMAIL REDACTED]]:/var/www/onestoreofficiel/
```

**Important:** Do **not** upload `.env` or `.env.local` with real secrets over SCP in plain text; create `.env.production` **on the server** (see 4.3).

If you prefer to clone and build on the server:

```bash
cd /var/www/onestoreofficiel
# Clone or upload source, then:
npm ci
npx prisma generate
npm run build
```

### 4.2 Node.js and PM2 (if not already installed)

```bash
# Example for Node 20 LTS (adjust for your OS)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# PM2
sudo npm install -g pm2
pm2 startup
# Run the command it prints (e.g. sudo env PATH=... pm2 startup systemd -u YOUR_USER --hp /home/YOUR_USER)
```

### 4.3 Environment variables on the server

Create **only on the server** (do not commit):

```bash
sudo nano /var/www/onestoreofficiel/.env.production
```

**Required and recommended variables** (set `PORT=3001` so Caddy and PM2 match):

```env
NODE_ENV=production
PORT=3001

# Database (Mongo on same host via Docker or external)
DATABASE_URL="mongodb://127.0.0.1:27017/onestore"

# Auth (Next-Auth v5)
AUTH_SECRET="<generate with: openssl rand -base64 32>"
AUTH_URL="https://onestoreofficiel.com"

# App URL (for emails, redirects)
NEXT_PUBLIC_APP_URL="https://onestoreofficiel.com"

# Optional: store name, support
NEXT_PUBLIC_STORE_NAME="One Store"
# DEFAULT_SUPPORT_EMAIL=
# RESEND_API_KEY=
REPLACE_WITH_PRIVATE_ENV_VALUE RESEND_FROM=
# STRIPE_SECRET_KEY=
REPLACE_WITH_PRIVATE_ENV_VALUE NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=
# STRIPE_WEBHOOK_SECRET=
REPLACE_WITH_PRIVATE_ENV_VALUE PAYPAL_CLIENT_ID=
# PAYPAL_CLIENT_SECRET=
REPLACE_WITH_PRIVATE_ENV_VALUE PAYPAL_ENV=live
# PAYPAL_WEBHOOK_ID=
# TWILIO_ACCOUNT_SID=
# TWILIO_AUTH_TOKEN=
REPLACE_WITH_PRIVATE_ENV_VALUE TWILIO_FROM=
```

Secure the file:

```bash
sudo chown "$USER:$USER" /var/www/onestoreofficiel/.env.production
chmod 600 /var/www/onestoreofficiel/.env.production
```

### 4.4 MongoDB (Docker, if you use existing compose)

On the VPS, in a directory that has your existing `docker-compose.yml` (only Mongo):

```bash
docker compose up -d mongo
# Ensure replica set is initialized (your compose has mongo-init)
docker compose ps
```

Then in `.env.production` use:

```env
DATABASE_URL="mongodb://127.0.0.1:27017/onestore"
```

If Prisma requires a replica set, use the same URL format you use locally (e.g. with `?replicaSet=rs0`). Push schema and seed if needed:

```bash
cd /var/www/onestoreofficiel
npx prisma db push
# npm run db:seed   # if you use a seed script
```

### 4.5 Build and run with PM2

```bash
cd /var/www/onestoreofficiel
export NODE_ENV=production
npm ci --omit=dev
npx prisma generate
npm run build
```

Start with PM2 (load env from `.env.production`):

```bash
cd /var/www/onestoreofficiel
pm2 start npm --name onestore -- start
# Or with explicit env file and port:
pm2 start npm --name onestore -- start --env production
pm2 save
pm2 list
```

If you need to force port 3001 in the process:

```bash
# Create ecosystem.config.cjs (see below) and then:
pm2 start ecosystem.config.cjs
pm2 save
```

**Optional `ecosystem.config.cjs`** in `/var/www/onestoreofficiel/ecosystem.config.cjs`:

```js
module.exports = {
  apps: [{
    name: 'onestore',
    cwd: '/var/www/onestoreofficiel',
    script: 'node_modules/.bin/next',
    args: 'start',
    instances: 1,
    exec_mode: 'fork',
    env: { NODE_ENV: 'production', PORT: 3001 },
    env_file: '/var/www/onestoreofficiel/.env.production',
    error_file: '/var/www/onestoreofficiel/logs/err.log',
    out_file: '/var/www/onestoreofficiel/logs/out.log',
  }],
};
```

Create logs dir and start:

```bash
mkdir -p /var/www/onestoreofficiel/logs
pm2 start ecosystem.config.cjs
pm2 save
```

Check that the app listens on 3001:

```bash
curl -s -o /dev/null -w "%{http_code}" http://127.0.0.1:3001
# Expect 200 or 307
```

---

## 5. Caddy configuration

**Principle:** Do **not** replace the existing Caddyfile. Only **add** a new block for One Store.

**Where:** Same file you audited, usually `/etc/caddy/Caddyfile`.

**Step 1 – Backup:**

```bash
sudo cp /etc/caddy/Caddyfile /etc/caddy/Caddyfile.bak.before-onestore
```

**Step 2 – Append** the block below at the **end** of the Caddyfile (after any existing `}` and newline):

```caddy
# One Store – onestoreofficiel.com
onestoreofficiel.com, www.onestoreofficiel.com {
    encode gzip zstd
    reverse_proxy 127.0.0.1:3001
    log {
        output file /var/log/caddy/onestoreofficiel.log
        format json
    }
}
```

**Step 3 – Validate and reload:**

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
```

**Notes:**

- Caddy will obtain and renew HTTPS for both hostnames automatically.
- HTTP→HTTPS redirect is default in Caddy 2.
- Only the app port 3001 is used internally; do **not** open 3001 on the firewall for public access.

**If your Caddyfile uses a different structure** (e.g. snippets or imports), add the same block content in the appropriate place so both hostnames use `reverse_proxy 127.0.0.1:3001`.

---

## 6. Security and stability

- **Firewall:** Keep 80 and 443 open for Caddy. Do **not** expose 3001 publicly.
  ```bash
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo ufw allow 22/tcp
  sudo ufw enable
  ```
- **Restart on reboot:** PM2 startup is already configured with `pm2 startup` and `pm2 save`.
- **File permissions:** App and env under `/var/www/onestoreofficiel` owned by the user running PM2; `.env.production` mode `600`.
- **Secrets:** Never log or echo `AUTH_SECRET`, `DATABASE_URL`, or API keys. Use `.env.production` only on the server.

---

## 7. Validation checklist

After deployment:

- [ ] `dig +short onestoreofficiel.com` and `www.onestoreofficiel.com` → `SERVER_IP`
- [ ] `https://onestoreofficiel.com` loads (HTTPS)
- [ ] `https://www.onestoreofficiel.com` loads (HTTPS)
- [ ] `http://onestoreofficiel.com` redirects to HTTPS
- [ ] Existing other sites on the same VPS still load
- [ ] `pm2 list` shows `onestore` online
- [ ] `curl -I http://127.0.0.1:3001` returns 200/307
- [ ] After `sudo reboot`, wait 1–2 minutes and confirm the site and other sites still work

---

## 8. Rollback steps

**If the new site causes issues:**

1. **Remove only the One Store block from Caddy:**
   ```bash
  sudo cp /etc/caddy/Caddyfile.bak.before-onestore /etc/caddy/Caddyfile
  sudo caddy validate --config /etc/caddy/Caddyfile
  sudo systemctl reload caddy
  ```
2. **Stop the app:**
   ```bash
  pm2 stop onestore
  pm2 save
  ```
3. **Restore Caddy from the audit backup** (if you made one):
   ```bash
  sudo cp /etc/caddy/Caddyfile.bak.YYYYMMDD-HHMMSS /etc/caddy/Caddyfile
  sudo systemctl reload caddy
  ```

This leaves all other Caddy sites unchanged and only disables the new domain and app.

---

## Summary

| Item | Value |
|------|--------|
| App path | `/var/www/onestoreofficiel` |
| App port | 3001 (internal) |
| Process manager | PM2 (`onestore`) |
| Caddy | Append new block; no replace of existing config |
| DNS | A records for `@` and `www` → `SERVER_IP` |
| HTTPS | Handled by Caddy for onestoreofficiel.com and www |

By following this guide you integrate One Store into your existing Caddy-based VPS without switching to Nginx and without breaking current hosted projects.
