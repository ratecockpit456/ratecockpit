# Rate Cockpit — Buyer Setup Guide

**Product:** Rate Cockpit (ratecockpit.com)  
**Stack:** Node.js 24, TypeScript, React + Vite, Express 5, PostgreSQL, Drizzle ORM, Stripe  
**License:** Commercial — purchased source, do not resell.

---

## What's included

| File | Contents |
|------|----------|
| `rate-cockpit-source.tar.gz` | Full source code (pnpm monorepo) |
| `rate_cockpit_schema.sql` | PostgreSQL schema — run once on your DB to create tables |

---

## Prerequisites

- Node.js 24+
- pnpm 9+ (`npm install -g pnpm`)
- PostgreSQL 15+ database (Neon, Supabase, Railway, or self-hosted)
- Stripe account (for payments)

---

## Step-by-step setup

### 1. Extract source code
```bash
tar -xzf rate-cockpit-source.tar.gz
cd rate-cockpit
pnpm install
```

### 2. Create your PostgreSQL database and run schema
```bash
psql your_database_url < rate_cockpit_schema.sql
```

### 3. Set environment variables

Create `.env` files or set these in your hosting platform:

**API Server** (`artifacts/api-server/.env`):
```
DATABASE_URL=postgresql://user:password@host:5432/dbname
SESSION_SECRET=your-random-secret-min-32-chars
NODE_ENV=production
PORT=8080
```

**Frontend** (`artifacts/rate-calculator/.env`):
```
VITE_STRIPE_PUBLISHABLE_KEY=pk_live_...
```

**Stripe** (set in API server env):
```
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

### 4. Push DB schema (alternative to SQL file)
```bash
pnpm --filter @workspace/db run push
```

### 5. Run in development
```bash
# Terminal 1 — API server
pnpm --filter @workspace/api-server run dev

# Terminal 2 — Frontend
pnpm --filter @workspace/rate-calculator run dev
```

### 6. Build for production
```bash
pnpm run build
```

---

## Deployment options

| Platform | Recommended for |
|----------|----------------|
| Railway | Easiest — auto-detects pnpm, free PostgreSQL |
| Render | Good free tier for hobby projects |
| Vercel + Neon | Serverless frontend + DB |
| VPS (DigitalOcean/Hetzner) | Full control, cheapest long-term |
| Replit | Zero-config, deploy in 1 click |

---

## Monetization already configured

- **Stripe card payments** — just add your Stripe keys
- **UPI QR** — update `vikash709804@oksbi` → your UPI ID in checkout page
- **PayPal QR** — update QR image in checkout page
- **Affiliate links** — update tool links in benchmarks/about pages
- **Premium gate** — `$6.25/month`, change in `pricing.tsx` + `checkout.tsx` + Stripe dashboard

---

## Key files to customize

| File | What to change |
|------|---------------|
| `artifacts/rate-calculator/src/pages/checkout.tsx` | UPI ID, PayPal QR, price |
| `artifacts/rate-calculator/src/pages/about.tsx` | Founder name, story |
| `artifacts/rate-calculator/src/pages/terms.tsx` | Your company name, jurisdiction |
| `artifacts/api-server/src/routes/stripe.ts` | Stripe product/price IDs |

---

## Support

Original author: Vinay Kumar — ratecockpit.com  
For setup help, contact the seller.
