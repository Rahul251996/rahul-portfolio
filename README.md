# Rahul Mishra portfolio

The existing React/JavaScript portfolio, migrated in place to static Vercel hosting with live Supabase recommendations. There is no Spring Boot, Maven, Docker or separately hosted application server to run.

## What stays

Dark/light themes, typography, animations, responsive navigation, Home, About, Experience, Skills, Projects, project details, Resume, Contact and Recommendations. The eight career projects and six roles keep their resume-backed content. Four former AI modules are retained as non-live portfolio project descriptions. The original public resume PDF is preserved byte-for-byte.

Recommendations are dynamic: a Supabase Edge Function accepts validated, CAPTCHA-protected submissions; PostgreSQL saves PENDING records; Supabase Auth plus database-enforced admin authorization permits approval/rejection. Public reads return approved records only. No testimonials are seeded. Contact uses email/phone/social links.

## Run locally

Use Node.js 24 LTS. Open `frontend` in VS Code:

```powershell
npm ci
npm run dev
```

Open http://127.0.0.1:5173/. Portfolio pages and resume work without service credentials. Until Supabase and Turnstile are configured, the wall explains that it is unavailable and submission/admin actions remain disabled; it does not show fake recommendations.

Copy `frontend/.env.example` to `frontend/.env.local` and fill only public configuration after following [Supabase setup](docs/SUPABASE_SETUP.md). Restart Vite after environment changes. Admin route: `/#/admin/recommendations`.

## Project structure

```text
content/portfolio.json             Static career/project content
frontend/src/pages/               Existing React JSX pages
frontend/src/components/          Reusable UI, review form/cards and CAPTCHA
frontend/src/services/            Supabase recommendations, Auth and asset URLs
frontend/public/resume/           Existing public resume PDF
supabase/migrations/              Schema, constraints, grants, RLS and atomic submission
supabase/functions/               JavaScript Edge Function and shared validation
frontend/scripts/test-policies.mjs Real PostgreSQL RLS/constraint tests
frontend/vercel.json              Vercel install, validation, build and output
.github/workflows/ci.yml           Push / pull request checks
scripts/build_resume.py           Optional PDF regeneration
```

## Setup and deployment

1. [Read the migration report](docs/MIGRATION_REPORT.md).
2. [Push this existing project to GitHub and deploy with Vercel](docs/DEPLOYMENT.md). The first static deployment can run without service credentials.
3. [Create Supabase Free, configure Auth/RLS/Edge Function and free CAPTCHA](docs/SUPABASE_SETUP.md), then add public values in Vercel and redeploy.
4. [Run local checks and the hosted verification checklist](docs/VALIDATION.md).

The target is ₹0/month within provider free-tier quotas, not unlimited hosting or guaranteed availability. No accounts, paid services, GitHub repository, credentials, domain or deployment have been created automatically.

## Checks

```powershell
cd frontend
npm ci
npm run lint
npm test
npm run build
npm run test:e2e
```

The browser suite uses explicit Supabase/CAPTCHA mocks, and tests both desktop and mobile. SQL policy tests use a disposable PostgreSQL database ending in `_test` and roll back their changes:

```powershell
# Set POLICY_TEST_DATABASE_URL privately for a NEW empty test database.
npm run test:policies
```

PostgreSQL is required only for optional local SQL-policy testing; the deployed database is Supabase. GitHub Actions runs these tests with an isolated job-local database. The website itself has no local database/server dependency. Actual results and unexecuted hosted checks are recorded in [VALIDATION.md](docs/VALIDATION.md).

## Content, privacy and maintenance

Edit `content/portfolio.json` and push to main when ready to publish changes. Once connected, Vercel rebuilds and deploys the production branch automatically. Your laptop and Vite dev server do not need to stay running. Never add invented achievements or demo URLs. Reviews are not stored in this JSON. The PDF generator reads the same file and writes to `frontend/public/resume`; it requires Python/reportlab/pypdfium2/pypdf and visual checking of both pages.

Never put Supabase secret/service-role keys, admin passwords or access tokens in VITE variables or GitHub files. The published JavaScript intentionally contains only public service configuration. Administrative authority comes from a private database allowlist, never user-editable metadata. Review identity is self-asserted; the moderator should check it before approval.

The removed implementation was saved outside this project in a local archive before deletion. Historical validation documents are reference only and do not describe the current runtime.
