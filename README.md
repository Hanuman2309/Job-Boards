# DevJobs 🚀

> A modern, full-stack **job board for developers** — built with Next.js 14, Prisma, Tailwind CSS, and NextAuth.

[![CI/CD Pipeline](https://github.com/YOUR_USERNAME/devjobs/actions/workflows/ci-cd.yml/badge.svg)](https://github.com/YOUR_USERNAME/devjobs/actions/workflows/ci-cd.yml)
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/YOUR_USERNAME/devjobs)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Database](#database)
- [Authentication](#authentication)
- [API Reference](#api-reference)
- [CI/CD Pipeline](#cicd-pipeline)
- [Deployment to Vercel](#deployment-to-vercel)
- [Environment Variables](#environment-variables)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**DevJobs** is a production-ready job board platform that connects software developers with top tech companies. Employers can post job listings in seconds; developers can search, filter, and apply — all within the same platform.

**Business Value:**
- Employers reach a targeted audience of active developers
- Developers find curated, tech-focused opportunities
- Featured listings give companies premium visibility
- Real-time application tracking for both sides

---

## Features

| Feature | Description |
|---|---|
| 🔍 **Smart Search** | Full-text search by title, company, location, and skills |
| 🏷️ **Filter by Type** | Full-time, Part-time, Contract, Remote, Freelance, Internship |
| ⭐ **Featured Jobs** | Promoted listings with priority placement |
| 📋 **Job Posting** | Employers post jobs with rich descriptions, salary, and skill tags |
| 🔒 **Authentication** | Email/password + GitHub OAuth via NextAuth.js |
| 📊 **Dashboard** | Job seekers track all applications and statuses |
| 📱 **Responsive** | Mobile-first, works on all screen sizes |
| 🌐 **SEO-ready** | Open Graph meta tags, semantic HTML |
| 🔌 **REST API** | JSON API for programmatic job and application access |

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | [Next.js 14](https://nextjs.org/) (App Router) |
| **Language** | TypeScript |
| **Styling** | [Tailwind CSS](https://tailwindcss.com/) |
| **Database** | SQLite (dev) / PostgreSQL (prod) via [Prisma](https://prisma.io/) ORM |
| **Auth** | [NextAuth.js](https://next-auth.js.org/) (Credentials + GitHub OAuth) |
| **Validation** | [Zod](https://zod.dev/) |
| **Deployment** | [Vercel](https://vercel.com/) |
| **CI/CD** | GitHub Actions |

---

## Project Structure

```
devjobs/
├── prisma/
│   ├── schema.prisma          # Database schema
│   └── seed.ts                # Seed data (10 sample jobs)
├── src/
│   ├── app/
│   │   ├── layout.tsx         # Root layout (Navbar + Footer)
│   │   ├── page.tsx           # Homepage (Hero + featured + latest jobs)
│   │   ├── jobs/
│   │   │   ├── page.tsx       # Browse all jobs with filters
│   │   │   └── [id]/page.tsx  # Job detail with apply button
│   │   ├── post-job/page.tsx  # Post a new job listing
│   │   ├── dashboard/page.tsx # Applicant dashboard
│   │   ├── auth/
│   │   │   ├── signin/page.tsx
│   │   │   └── register/page.tsx
│   │   └── api/
│   │       ├── auth/[...nextauth]/route.ts  # NextAuth handler
│   │       ├── auth/register/route.ts       # User registration
│   │       ├── jobs/route.ts                # GET/POST jobs
│   │       └── applications/route.ts        # GET/POST applications
│   ├── components/
│   │   ├── Navbar.tsx         # Sticky nav with auth state
│   │   ├── Footer.tsx         # Site-wide footer
│   │   ├── JobCard.tsx        # Job listing card
│   │   ├── SearchBar.tsx      # Keyword + location + type filter
│   │   ├── StatsSection.tsx   # Platform statistics
│   │   ├── ApplyButton.tsx    # Apply / submit application form
│   │   └── Providers.tsx      # NextAuth SessionProvider wrapper
│   ├── lib/
│   │   ├── db.ts              # Prisma client singleton
│   │   ├── auth.ts            # NextAuth config
│   │   ├── jobs.ts            # Job query helpers
│   │   └── utils.ts           # Date formatting, class helpers
│   └── app/globals.css        # Tailwind + global CSS
├── .github/
│   └── workflows/
│       ├── ci-cd.yml          # Main CI/CD pipeline
│       └── security.yml       # Weekly dependency audit
├── vercel.json                # Vercel deployment config
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── .env.example
```

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) ≥ 18
- npm ≥ 9

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/devjobs.git
cd devjobs
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up environment variables

```bash
cp .env.example .env
```

Edit `.env` and fill in your values (see [Environment Variables](#environment-variables)).

### 4. Set up the database

```bash
# Push the schema to your database
npm run db:push

# Seed with 10 sample job listings
npm run db:seed
```

### 5. Start the development server

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) 🎉

### 6. Explore the database (optional)

```bash
npm run db:studio
```

---

## Database

DevJobs uses **Prisma ORM** with SQLite for development and PostgreSQL for production.

### Schema

```
User           – Accounts (job seekers and employers)
Job            – Job listings with type, tags, salary, description
Application    – Job applications (User → Job many-to-many)
SavedJob       – Bookmarked jobs
```

### Migrations

```bash
# Create a new migration after schema changes
npm run db:migrate

# Push schema changes without migration files (dev only)
npm run db:push

# Seed sample data
npm run db:seed
```

### Switching to PostgreSQL (Production)

1. Update `prisma/schema.prisma`:
   ```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }
   ```
2. Set `DATABASE_URL` to a PostgreSQL connection string in Vercel settings.
3. Run `npx prisma migrate deploy` on first deploy.

---

## Authentication

Authentication is handled by **NextAuth.js** supporting:

| Provider | Description |
|---|---|
| **Credentials** | Email + password (bcrypt hashed) |
| **GitHub OAuth** | Sign in with GitHub account |

### Setting up GitHub OAuth

1. Go to [GitHub Developer Settings](https://github.com/settings/developers)
2. Create a new OAuth App
3. Set **Homepage URL** to your app URL
4. Set **Callback URL** to `https://YOUR_DOMAIN/api/auth/callback/github`
5. Copy the Client ID and Secret to your `.env`

### User Roles

| Role | Permissions |
|---|---|
| `JOBSEEKER` | Browse jobs, apply, view dashboard |
| `EMPLOYER` | Post jobs, view applicants |
| `ADMIN` | Full platform access |

---

## API Reference

All API routes are under `/api/`.

### Jobs

#### `GET /api/jobs`

Returns a list of active jobs.

**Query Parameters:**

| Param | Type | Description |
|---|---|---|
| `q` | string | Full-text search keyword |
| `type` | string | Job type (`FULL_TIME`, `REMOTE`, etc.) |
| `location` | string | Location filter |
| `limit` | number | Results per page (default: 20) |
| `skip` | number | Pagination offset |

**Example:**
```
GET /api/jobs?q=react&type=REMOTE&limit=10
```

#### `POST /api/jobs`

Creates a new job listing.

**Request Body:**
```json
{
  "title": "Senior React Developer",
  "company": "Acme Corp",
  "location": "Remote",
  "type": "FULL_TIME",
  "salary": "$120k–$150k",
  "description": "We are looking for...",
  "tags": ["React", "TypeScript"],
  "url": "https://acme.com/apply"
}
```

---

### Applications

#### `POST /api/applications` _(requires auth)_

Submit a job application.

**Request Body:**
```json
{
  "jobId": "clxxxxxxxx",
  "coverLetter": "I'm excited to apply because..."
}
```

#### `GET /api/applications` _(requires auth)_

Returns all applications for the authenticated user.

---

### Auth

#### `POST /api/auth/register`

Create a new user account.

**Request Body:**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "securepassword",
  "role": "JOBSEEKER"
}
```

---

## CI/CD Pipeline

The pipeline is defined in [`.github/workflows/ci-cd.yml`](.github/workflows/ci-cd.yml) and consists of 4 jobs:

```
Push to main / PR
       │
       ▼
┌─────────────┐
│  Lint &     │  ESLint + TypeScript type check
│  Type Check │
└──────┬──────┘
       │ success
       ▼
┌─────────────┐
│    Build    │  next build (with Prisma client gen)
│             │  Uploads .next/ as artifact
└──────┬──────┘
       │ success
       ├──────────────────────────────┐
       ▼ (main branch push)           ▼ (PR / develop)
┌─────────────────┐         ┌──────────────────────┐
│  Deploy to      │         │  Deploy Preview to   │
│  Vercel (Prod)  │         │  Vercel + comment URL│
└─────────────────┘         └──────────────────────┘
```

### Pipeline Jobs

| Job | Trigger | Description |
|---|---|---|
| `lint` | All pushes/PRs | ESLint + `tsc --noEmit` |
| `build` | After lint | `next build` + upload artifact |
| `deploy-production` | Push to `main` | Deploys to Vercel production |
| `deploy-preview` | PRs + `develop` | Deploys preview + comments URL on PR |

A separate **`security.yml`** workflow runs `npm audit` weekly.

---

## Deployment to Vercel

### Step 1: Install Vercel CLI

```bash
npm install -g vercel
```

### Step 2: Link your project

```bash
cd devjobs
vercel link
```

### Step 3: Set environment secrets in GitHub

Go to your GitHub repo → **Settings → Secrets and variables → Actions** and add:

| Secret | Description |
|---|---|
| `VERCEL_TOKEN` | API token from [vercel.com/account/tokens](https://vercel.com/account/tokens) |
| `NEXTAUTH_SECRET` | Random 32-char string (generate with `openssl rand -base64 32`) |

### Step 4: Set environment variables in Vercel

Go to your Vercel project → **Settings → Environment Variables** and add:

| Variable | Value |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string (e.g., from [Supabase](https://supabase.com/), [PlanetScale](https://planetscale.com/), or [Neon](https://neon.tech/)) |
| `NEXTAUTH_SECRET` | Same as GitHub secret |
| `NEXTAUTH_URL` | Your Vercel deployment URL |
| `GITHUB_ID` | GitHub OAuth App Client ID |
| `GITHUB_SECRET` | GitHub OAuth App Client Secret |

### Step 5: Push to main

```bash
git push origin main
```

The CI/CD pipeline will automatically build and deploy to Vercel. ✅

### Manual Deploy

```bash
vercel --prod
```

---

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `DATABASE_URL` | ✅ | Database connection string |
| `NEXTAUTH_SECRET` | ✅ | Secret key for NextAuth JWT signing |
| `NEXTAUTH_URL` | ✅ | Full URL of your app (e.g., `https://devjobs.vercel.app`) |
| `GITHUB_ID` | Optional | GitHub OAuth App client ID |
| `GITHUB_SECRET` | Optional | GitHub OAuth App client secret |

> **Tip:** Generate a strong `NEXTAUTH_SECRET` with:
> ```bash
> openssl rand -base64 32
> ```

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Make your changes and write tests
4. Ensure lint and type checks pass: `npm run lint && npx tsc --noEmit`
5. Commit with a descriptive message: `git commit -m "feat: add company profiles page"`
6. Push and open a Pull Request

### Commit Convention

This project uses [Conventional Commits](https://www.conventionalcommits.org/):

| Prefix | Description |
|---|---|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation changes |
| `style:` | Formatting changes |
| `refactor:` | Code refactoring |
| `test:` | Adding tests |
| `chore:` | Build / tooling changes |

---

## License

MIT © 2024 DevJobs. See [LICENSE](LICENSE) for details.
