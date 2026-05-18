# Glint

**Resume intelligence for job seekers.** Upload a resume, paste a job ad, and get a match score from three analysis engines in seconds.

🌐 **[glint-analysis.xyz](https://glint-analysis.xyz)**

---

## What it does

Glint analyses how well a resume fits a specific job posting across three dimensions:

- **AI Semantic** — Gemini reads both documents and identifies conceptual overlap, experience gaps, and actionable rewrites.
- **Keyword Match** — A vocabulary-rarity algorithm extracts domain-specific terms from the job ad and checks which ones appear in the resume, weighted by importance and repetition.
- **Rule-Based** — Structured criteria checks: years of experience, seniority indicators, required certifications, and resume structure.

All three run in parallel and stream results back as each finishes. No account required to start.

---

## Repositories

| Repo | Stack | Description |
|---|---|---|
| `glint-frontend` | Next.js 16, TypeScript, Tailwind CSS | Web interface, SSE result streaming, auth flows |
| `glint-backend` | ASP.NET Core 8, PostgreSQL, EF Core | REST API, analysis orchestration, JWT auth |

---

## Stack

**Frontend**
- Next.js 16 (App Router)
- Tailwind CSS v4
- Geist + JetBrains Mono
- chart.js, anime.js, GSAP
- react-toastify

**Backend**
- ASP.NET Core 8
- PostgreSQL via Npgsql + EF Core
- Google Gemini API (`google-genai`)
- PdfPig for PDF text extraction
- Resend for transactional email
- BCrypt password hashing, JWT + refresh token auth

---

## Features

- Three-engine resume analysis with SSE streaming
- Guest mode (no account needed) and authenticated mode with history tracking
- Save up to 5 resumes and 5 job advertisements per account
- Dashboard with score-over-time chart, per-method statistics, and paginated history
- Email verification and password reset via one-time codes
- PDF validation: size limit, MIME check, magic bytes, malicious content scan
- Rate limiting per user/IP on auth and analysis endpoints
- Light and dark theme

---

## Self-hosting

See the individual READMEs for setup instructions:

- [`glint-frontend/README.md`](./glint-frontend/README.md) — Next.js dev server, environment variables
- [`glint-backend/README.md`](./glint-backend/README.md) — .NET setup, user secrets, database migrations

You will need a PostgreSQL database, a Google Gemini API key, and a Resend account for email delivery.

---

## Limits

| Resource | Limit |
|---|---|
| Resumes per user | 5 |
| Saved job ads per user | 5 (oldest archived when exceeded) |
| PDF upload size | 5 MB |
| Job ad text | 10,000 characters |
| Auth requests | 8 / minute |
| Analysis requests | 10 / 3 min (authenticated), 4 / 3 min (guest) |

---

## License

MIT
