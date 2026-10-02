# OneStore Admin: Settings, RBAC & Admin Users

## Overview

The Admin Settings system provides a single place to configure the store, manage roles/permissions (RBAC), and manage admin users. All settings are stored in the database; the sidebar and server actions enforce permissions.

## Routes

- **Settings (all locales):**
  - `/en/admin/settings`
  - `/fr/admin/settings`
  - `/ar/admin/settings`

Access requires the **settings.read** permission. Editing and saving require **settings.write**.

## Permissions (RBAC)

Permission keys are defined in `src/lib/permissions.ts` and enforced in:

- **UI:** Admin sidebar hides menu items when the current user lacks the required permission for that path.
- **Server:** Every settings, admin-user, and audit-log action checks permissions via `getAdminPermissions(session)` and returns Unauthorized when missing.

### Permission keys

| Key | Description |
|-----|-------------|
| `products.read` / `products.write` / `products.delete` | Products |
| `orders.read` / `orders.write` | Orders |
| `customers.read` / `customers.write` / `customers.delete` | Customers |
| `categories.read` / `categories.write` | Categories |
| `brands.read` / `brands.write` | Brands |
| `settings.read` / `settings.write` | Settings page |
| `users.read` / `users.write` | Team & admin users |
| `audit.read` | Audit logs |

### Default roles (seeded)

| Role    | Scope |
|---------|--------|
| **Owner**  | All permissions (cannot be removed or reduced). |
| **Admin**  | All permissions. |
| **Manager**| Products, orders, customers, categories, brands (full CRUD where applicable). |
| **Support**| Orders and customers (read + update statuses). |
| **Content**| Categories, brands, products (read + write, no delete). |
| **Viewer** | Read-only: products, orders, customers, categories, brands, audit. |

Roles are stored in `AdminRole` with a `permissions` JSON array. You can change permissions for any role except **Owner** from **Settings → Roles & Permissions**.

## Creating admin users

1. Go to **Settings → Team & Admin Users**.
2. Click **Add Admin User**.
3. Enter **Full name** and **Email**, choose **Role** and **Status** (Active/Suspended).
4. Click **Create & generate credentials**.
   - The system generates:
     - **Username:** `onestore_<first>.<last>` (with random suffix if the username exists).
     - **Password:REPLACE_WITH_PRIVATE_ENV_VALUE 14–18 character random string (letters, numbers, symbols).
   - Credentials are shown once; use **Copy** to store them safely.
5. Optionally use **Regenerate credentials** to get a new password (implement reset via **Reset password** action if you add it).
6. **Send credentials by email** is a placeholder (“Coming soon”) unless you wire your email provider.

Passwords are stored as bcrypt hashes. New admin users get an **Audit log** entry (`admin.create`).

## Database

- **AdminRole** – name, `isSystem`, `permissions` (JSON array of permission keys).
- **Admin** – username, passwordHash, displayName, email, roleId (→ AdminRole), status, lastLoginAt. Legacy `role` enum is still used when `roleId` is null (backward compatibility).
- **AuditLog** – actorId, action, entityType, entityId, meta, createdAt.
- **SiteSettings** – single document (id `"default"`) with `data` (JSON) and `updatedAt`.

## Commands

```bash
# Push schema (MongoDB)
npm run db:push

# Seed default admin roles (Owner, Admin, Manager, Support, Content, Viewer)
npm run db:seed-roles

# Create initial admin user (legacy script; set ADMIN_PASSWORD)
ADMIN_PASSWORD=YourSecurePass123! npm run db:admin
```

After seeding roles, you can create additional admins from **Settings → Team & Admin Users** with generated credentials.

## i18n & RTL

All settings UI strings use `admin.settings.*` in `src/messages/{en,fr,ar}.json`. For Arabic, the settings page uses `dir="rtl"` when `locale === "ar"`.

## Security notes

- **Owner** role cannot be edited (permissions or removal).
- **Settings** and **Team & Admin Users** are only visible and usable if the user has the corresponding read/write permissions.
- Server actions (e.g. `updateSettings`, `createAdminUser`, `updateRolePermissions`) always verify permissions; typing a URL or calling an action without permission returns an error or redirect.
