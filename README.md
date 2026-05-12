# Glint

> When a recruiter scans 500 resumes, yours needs a glint to be noticed. Glint finds it.

Glint analyzes your resume against a job advertisement using three methods — AI semantic evaluation, rule-based filtering, and keyword matching — then tells you what to fix. Upload a PDF, paste a job ad, and all three run in parallel. Each returns a score, a breakdown, and concrete suggestions so you can see how automated systems read your resume before a human does.

Analyses are saved to your account so you can track improvements over time.

---

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 16, TypeScript, Tailwind CSS v4 |
| Backend | ASP.NET Core 8, C# |
| Database | SQL Server, Entity Framework Core 8 |
| AI analysis | Google Gemini API |
| Email | Resend |
| PDF parsing | PdfPig |
| Auth | JWT + rotating refresh tokens, email OTC verification |

---

## Project structure

```
glint/
├── glint-frontend/          # Next.js app
│   ├── src/app/             # Pages and layouts
│   ├── src/components/      # UI components
│   └── src/lib/             # Auth helpers, hooks, utilities
│
└── glint-backend/           # ASP.NET Core API
    ├── Controllers/         # HTTP endpoints
    ├── Services/            # Business logic
    ├── Repositories/        # Data access
    ├── Models/              # EF Core entities
    └── DTOs/                # Request/response shapes
```

---

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/) 20+
- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- SQL Server (local or Azure)
- A [Google Gemini API key](https://aistudio.google.com/)
- A [Resend](https://resend.com/) account for email

### Backend

```bash
cd glint-backend

# Restore packages
dotnet restore

# Configure secrets (development)
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "your-connection-string"
dotnet user-secrets set "Jwt:Key" "your-jwt-secret-min-32-chars"
dotnet user-secrets set "Jwt:Issuer" "glint-backend"
dotnet user-secrets set "Jwt:Audience" "glint-frontend"
dotnet user-secrets set "Jwt:ExpiryMinutes" "60"
dotnet user-secrets set "Gemini:ApiKey" "your-gemini-api-key"
dotnet user-secrets set "Resend:ApiKey" "your-resend-api-key"
dotnet user-secrets set "Email:From" "noreply@yourdomain.com"
dotnet user-secrets set "Frontend:BaseUrl" "http://localhost:3000"
dotnet user-secrets set "Cors:AllowedOrigins:0" "http://localhost:3000"

# Apply migrations and run
dotnet ef database update
dotnet run
```

API runs at `https://localhost:7248`. Swagger is available at `/swagger` in development.

### Frontend

```bash
cd glint-frontend

npm install

# Copy and fill in the environment file
cp .env.example .env.local
# Set NEXT_PUBLIC_API_BASE_URL=https://localhost:7248

npm run dev
```

App runs at `http://localhost:3000`.

---

## API overview

| Method | Endpoint | Description |
|---|---|---|
| POST | `/auth/register` | Create account, sends verification email |
| POST | `/auth/login` | Password login, returns JWT + refresh token |
| POST | `/auth/verify-email` | Verify OTC from email |
| POST | `/auth/refresh` | Rotate refresh token |
| POST | `/auth/forgot-password` | Send password reset code |
| POST | `/auth/reset-password` | Set new password with code |
| POST | `/analyze/stream` | SSE stream — all three methods in parallel |
| POST | `/analyze/ai` | AI-only analysis |
| POST | `/analyze/keyword` | Keyword-only analysis |
| POST | `/analyze/rules` | Rule-based-only analysis |
| GET | `/user/history` | Paginated analysis history |
| GET | `/user/statistics` | Score stats and trend data |
| POST | `/user/resume` | Upload a resume PDF |
| GET | `/user/resume` | List saved resumes |
| DELETE | `/user/resume/{id}` | Delete a saved resume |
| GET | `/user/job-advertisement` | List saved job ads |
| DELETE | `/user/account` | Delete account and all data |

Analysis endpoints accept either a PDF upload or a saved `ResumeId`. Authenticated requests are saved to history; guest requests are stateless.

---

## Analysis methods

**AI semantic** — sends resume text and job ad to Gemini, returns a match score plus a gap analysis with specific rewrite suggestions.

**Keyword matching** — extracts domain-specific terms from the job ad using vocabulary rarity and repetition frequency, then checks how many appear in the resume. Returns matched terms, missing terms, and bigram phrases.

**Rule-based** — checks for explicit criteria: years of experience, required certifications, seniority signals, and structural resume qualities. Returns a per-rule pass/fail breakdown.

All three run in parallel over SSE so results appear as each finishes.

---

## Authors

Fredrik Andersson & Max Berglund — Chas Academy, Fullstack .NET 2026
