# One Store - Professional Tech E-commerce

## Architecture
- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript
- **Styling**: TailwindCSS v3.4 (for shadcn/ui compatibility), shadcn/ui, Framer Motion
- **Database**: MongoDB (via Prisma ORM)
- **Auth**: Auth.js (NextAuth v5 beta) with Credentials Provider
- **Payments**: Stripe
- **i18n**: next-intl (EN, FR, AR)
- **State Management**: React Context / Zustand (if needed) / Server Components
- **Form Validation**: Zod + React Hook Form

## Environment Variables
- `DATABASE_URL`: MongoDB connection string
- `AUTH_SECRET`: Secret key for Auth.js encryption
- `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`: Stripe PK
- `STRIPE_SECRET_KEY`: Stripe SK
- `NEXT_PUBLIC_APP_URL`: Base URL
- **Admin Seeding**:
  - `ADMIN_EMAIL`: Email for the initial admin user.
  - `ADMIN_PASSWORD`: Password for the initial admin user (min 12 chars, 1 letter, 1 number).
  - `ADMIN_NAME`: Name for the initial admin user.

## Project Structure
- `src/app`: App Router pages and layouts
- `src/components`: Reusable UI components
  - `ui`: shadcn/ui primitives
  - `common`: Shared components
- `src/lib`: Utility functions, Prisma client, shared logic
- `src/actions`: Server Actions
- `src/types`: TypeScript definitions
- `src/i18n`: Internationalization configuration
- `src/messages`: Translation files (en.json, fr.json, ar.json)
- `src/data/seed`: JSON seed files for database population

## Database & Prisma
### Commands
- **Generate Client**: `npx prisma generate`
- **Push Schema**: `npx prisma db push`
- **Seed Database**: `npm run db:seed`
- **Seed Admin User**: `npm run db:admin`
- **Studio (GUI)**: `npx prisma studio`

### Seed Data
Seed data is located in `src/data/seed/`. To reset and populate the database:
1. Ensure your MongoDB is running.
2. Run `npm run db:seed`.
This will clear existing products, categories, and brands, and re-populate them from the JSON files.

### Admin User Creation
**SECURITY WARNING**: Never commit default admin credentials to version control.
To create the initial Admin user:
1. Set `ADMIN_EMAIL`, `ADMIN_PASSWORD`, and `ADMIN_NAME` in your `.env` file.
2. Run `npm run db:admin`.
   - The script validates password strength (min 12 chars, alphanumeric).
   - It will NOT overwrite an existing admin's password.

## Authentication & RBAC
### Roles
- **ADMIN**: Full access to all resources.
- **EDITOR**: Can manage products, categories, and brands.
- **SUPPORT**: Read-only access to orders and users.
- **CUSTOMER**: Standard customer role (no admin access).

### Implementation
- **Auth.js**: Handles session management via JWT.
- **Middleware**: `src/auth.ts` configures providers and callbacks.
- **Rate Limiting**: Simple in-memory rate limiting implemented for login attempts (5 attempts / 10 mins). *For production, consider using Redis.*
- **Protection**:
  - `src/app/[locale]/admin/layout.tsx`: Checks for valid session and role before rendering admin pages. Redirects to login with `callbackUrl` if unauthenticated.
  - API Routes: Should implement server-side checks using `auth()` and `hasRole()`.
  - `src/lib/rbac.ts`: Helper functions for role verification.

## API Endpoints (Read-Only)
- `GET /api/products`: List all products. Supports filtering by `category`, `brand`, `minPrice`, `maxPrice`, `query` (search), and `sort`.
- `GET /api/products/[slug]`: Get a single product by slug.
- `GET /api/categories`: List all categories.
- `GET /api/brands`: List all brands.

## Admin API Endpoints (Protected)
- **Categories**:
  - `GET /api/admin/categories`: List all (ADMIN, EDITOR)
  - `POST /api/admin/categories`: Create (ADMIN, EDITOR)
  - `PUT /api/admin/categories/[id]`: Update (ADMIN, EDITOR)
  - `DELETE /api/admin/categories/[id]`: Delete (ADMIN only)
- **Brands**:
  - `GET /api/admin/brands`: List all (ADMIN, EDITOR)
  - `POST /api/admin/brands`: Create (ADMIN, EDITOR)
  - `PUT /api/admin/brands/[id]`: Update (ADMIN, EDITOR)
  - `DELETE /api/admin/brands/[id]`: Delete (ADMIN only)

**Locale Support**: All endpoints accept an optional `?locale=en|fr|ar` query parameter. If provided, localized fields (title, description, name) will be returned as strings in the requested language. If omitted, full translation objects are returned.

## Key Decisions
- **Single Source of Truth**: This file tracks major decisions.
- **Server Actions**: Used for mutations to ensure progressive enhancement and security.
- **RTL Support**: Built-in with Tailwind `rtl:` modifiers and `dir="rtl"` attribute.
- **Accessibility**: Strict adherence to WCAG via shadcn/ui primitives.
- **Database Schema**:
  - `Product` uses `Json` type for flexible `specs`, `title`, and `description` to support i18n and varying technical specifications.
  - `User` roles: ADMIN, EDITOR, SUPPORT, CUSTOMER.
