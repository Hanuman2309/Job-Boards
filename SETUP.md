# DevJobs – Quick Setup Checklist

## ✅ Step 1: Install dependencies & run locally

```bash
cd devjobs
npm install
cp .env.example .env   # fill in your values
npm run db:push        # create SQLite schema
npm run db:seed        # load 10 sample jobs
npm run dev            # http://localhost:3000
```

---

## ✅ Step 2: Push to GitHub

```bash
# Create a new repo on github.com first (no README), then:
cd devjobs
git init
git add .
git commit -m "feat: initial commit – DevJobs"
git remote add origin https://github.com/YOUR_USERNAME/devjobs.git
git branch -M main
git push -u origin main
```

---

## ✅ Step 3: Connect to Vercel

```bash
npm install -g vercel
vercel login
vercel link      # link to existing or create new project
```

Or go to https://vercel.com/new → Import Git Repository → select `devjobs`.

---

## ✅ Step 4: Set GitHub Secrets for CI/CD

Go to: **GitHub repo → Settings → Secrets and variables → Actions**

| Secret name | Value |
|---|---|
| `VERCEL_TOKEN` | Get from https://vercel.com/account/tokens |
| `NEXTAUTH_SECRET` | `openssl rand -base64 32` |

---

## ✅ Step 5: Set Vercel Environment Variables

Go to: **Vercel project → Settings → Environment Variables**

| Variable | Value |
|---|---|
| `DATABASE_URL` | PostgreSQL URL (Supabase / Neon / PlanetScale) |
| `NEXTAUTH_SECRET` | Same random string from Step 4 |
| `NEXTAUTH_URL` | `https://your-app.vercel.app` |
| `GITHUB_ID` | GitHub OAuth App Client ID (optional) |
| `GITHUB_SECRET` | GitHub OAuth App Client Secret (optional) |

**Recommended free PostgreSQL providers:**
- [Neon](https://neon.tech/) – Free tier, serverless PostgreSQL
- [Supabase](https://supabase.com/) – Free tier, PostgreSQL + extras
- [PlanetScale](https://planetscale.com/) – MySQL-compatible (requires schema change)

---

## ✅ Step 6: Update Prisma for PostgreSQL

Edit `prisma/schema.prisma`:

```prisma
datasource db {
  provider = "postgresql"   # ← change from "sqlite"
  url      = env("DATABASE_URL")
}
```

Then push the schema:

```bash
npx prisma migrate deploy
```

---

## ✅ Step 7: Trigger Deployment

```bash
git push origin main
```

The GitHub Actions pipeline will:
1. Run ESLint + TypeScript type check
2. Build the Next.js app
3. Deploy to Vercel production

---

## 🎉 Done!

Your DevJobs app is live. Every push to `main` auto-deploys.
Pull requests get a preview URL commented automatically.
