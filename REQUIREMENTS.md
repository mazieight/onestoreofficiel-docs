# One Store - Setup Requirements

This guide lists everything you need to install and configure to run this project on a new computer.

## 1. System Requirements (Prerequisites)

Before starting, make sure you have these installed on your machine:

*   **Node.js**: Version 18.17.0 or higher (Required for Next.js 16).
    *   Download: [nodejs.org](https://nodejs.org/) (Recommended: LTS version)
    *   Verify: `node -v`
*   **Git**: Version control system.
    *   Download: [git-scm.com](https://git-scm.com/)
    *   Verify: `git --version`
*   **MongoDB**: Database.
    *   **Option A (Recommended)**: Use [MongoDB Atlas](https://www.mongodb.com/atlas) (Cloud). Get your connection string (URI).
    *   **Option B**: Install [MongoDB Community Server](https://www.mongodb.com/try/download/community) locally.
*   **VS Code**: Recommended code editor.

## 2. Environment Variables

Create a file named `.env` in the root directory of the project and add the following keys. **You must fill in the values.**

```env
# Database Connection (MongoDB)
DATABASE_URL="mongodb+srv://<username>:<password>@<cluster>.mongodb.net/one-store"

# Authentication (Auth.js)
AUTH_SECRET="your-super-secure-random-secret-key" # Generate with: openssl rand -base64 32

# Public App URL (For Stripe redirects)
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# Stripe Payments (Get these from Stripe Dashboard)
STRIPE_SECRET_KEY="REPLACE_WITH_PRIVATE_ENV_VALUE"
STRIPE_WEBHOOK_SECRET="REPLACE_WITH_PRIVATE_ENV_VALUE" # Generated when you run the Stripe CLI listener
```

## 3. Installation Steps

Open your terminal (PowerShell, CMD, or Git Bash) in the project folder and run these commands step-by-step:

### Step 1: Install Dependencies
This installs all libraries listed in `package.json` (Next.js, React, Tailwind, Stripe, Prisma, etc.).

```bash
npm install
```

### Step 2: Generate Database Client
This creates the Prisma client based on your schema.

```bash
npx prisma generate
```

### Step 3: Push Schema to Database
This creates the collections/tables in your MongoDB database.

```bash
npx prisma db push
```

### Step 4: Seed the Database (Optional but Recommended)
Adds initial admin user and sample data.

```bash
npm run db:seed
```

## 4. Running the Project

### Development Mode
To start the app locally:

```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### Stripe Webhook Testing (Optional)
To test payments locally, you need the Stripe CLI.
1.  Download Stripe CLI.
2.  Login: `stripe login`
3.  Listen for events:
    ```bash
    stripe listen --forward-to localhost:3000/api/webhooks/stripe
    ```
4.  Copy the webhook secret (`whsec_...`) to your `.env` file.

## 5. Troubleshooting

*   **Error: ENOSPC / No space left on device**:
    *   This usually happens with Next.js cache. Run:
        ```bash
        rm -rf .next
        npm run dev
        ```
*   **Prisma Errors**:
    *   If you change the schema, always run `npx prisma generate` again.

## 6. List of Key Libraries Used

*   **Framework**: Next.js 16 (App Router)
*   **Language**: TypeScript
*   **Styling**: Tailwind CSS + shadcn/ui
*   **State Management**: Zustand (Persisted Cart)
*   **Database**: Prisma (ORM) + MongoDB
*   **Auth**: Auth.js (NextAuth v5)
*   **Payments**: Stripe
*   **Forms**: React Hook Form + Zod
*   **Internationalization**: next-intl (EN/FR/AR)
