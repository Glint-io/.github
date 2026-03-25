# Glint

> When a recruiter scans 500 resumes, yours needs a glint to be noticed. Glint finds it.

Glint is a web application that analyzes your resume against a job advertisement using three distinct methods - AI-based evaluation, rule-based filtering, and keyword matching - then tells you exactly what to fix.

## What it does

Upload your resume, paste a job ad, and Glint runs all three analyses in parallel. Each method returns a score, a breakdown, and concrete suggestions. You see how automated recruitment systems might read your resume before a human ever does.

Results and previous analyses are saved to your account so you can track improvements over time.

## Tech stack

| Layer | Technology |
|-------|------------|
| Frontend | Next.js, TypeScript, Tailwind CSS |
| Backend | ASP.NET, C# |
| Database | SQL, Entity Framework |
| Analysis | OpenAI API, rule-based engine, keyword matcher |

## Getting started

```bash
# Clone the repo
git clone https://github.com/your-username/glint.git

# Backend
cd backend
dotnet restore
dotnet run

# Frontend
cd frontend
npm install
npm run dev
```

> Requires a `.env` file with your API key and database connection string. See `.env.example`.

## Authors

Fredrik Andersson & Max Berglund - Chas Academy, Fullstack .NET 2026
