---
# Vibe Coding Starter Kit (Codex Edition)
> Build production-ready web apps using a structured, Codex-driven workflow with enforced architecture, RLS-by-default security, and PR-based deployment discipline.

This template is designed for **Codex-powered development**, using a strict Feature-Files system and phase-based execution model.

It does **not** rely on Claude Skills.
It uses structured prompt modes defined in `AGENTS.md`.
---

# What This Template Enforces

* Next.js (App Router) + TypeScript
* Supabase (Email OTP + Postgres + RLS-by-default)
* GitHub PR workflow
* Vercel (Preview → Production)
* Playwright E2E testing
* Feature-Files system (one feature = one file)
* Human-in-the-loop checkpoints
* No phase skipping

---

# Quick Start

## 1. Clone & Install

```bash
git clone https://github.com/YOUR_USERNAME/vibe-coding-starter-kit.git my-project
cd my-project
npm install
```

---

## 2. Supabase Setup (If Backend Needed)

1. Create a project at [https://supabase.com](https://supabase.com)
2. Copy `.env.local.example` → `.env.local`
3. Add:

   * `NEXT_PUBLIC_SUPABASE_URL`
   * `NEXT_PUBLIC_SUPABASE_ANON_KEY`
4. Apply migrations from `supabase/migrations/`

All user-owned tables must use RLS-by-default.

---

## 3. Start Development

```bash
npm run dev
```

Open:
[http://localhost:3000](http://localhost:3000)

---

# How This System Works

This template follows a strict **Feature-Files lifecycle**.

All work happens through structured Codex prompts defined by roles in `AGENTS.md`.

There are no slash commands.

You copy structured prompts into Codex for each phase.

---

# Development Workflow (Strict)

```
1. Requirements   → Define feature spec
2. Architecture   → Design schema + RLS + integration
3. Frontend       → Implement UI
4. Backend        → Implement migrations + RLS + server logic
5. QA             → Validate acceptance criteria + security
6. Deploy         → PR → Preview → Merge → Production
```

You may not skip phases.

---

# Feature Tracking

All features live in:

```
features/
  INDEX.md
  PROJ-1-<slug>.md
  PROJ-2-<slug>.md
```

Each feature file accumulates:

* Requirements
* Architecture
* QA results
* Deployment record

Status states:

* 🔵 Planned
* 🟡 In Progress
* ✅ Deployed

---

# Repo Structure (Codex Standard)

```
app/                     Routes + Server Actions
lib/                     Utilities (Supabase client, helpers)
supabase/migrations/     Schema source of truth
tests/e2e/               Playwright tests
features/                Feature specs
docs/                    PRDs + runbooks
AGENTS.md                Codex governance rules
```

Nested rule files:

```
app/AGENTS.md
supabase/AGENTS.md
tests/AGENTS.md
```

Codex must always read before modifying.

---

# RLS-by-Default (Non-Negotiable)

Every user-owned table must:

* Include `user_id uuid references auth.users(id)`
* Enable Row Level Security
* Define SELECT/INSERT/UPDATE/DELETE policies
* Restrict access via:

  ```
  user_id = auth.uid()
  ```

No feature may weaken isolation.

RLS failure = Critical severity.

---

# Authentication Standard

Only Email OTP is allowed.

Flow:

1. `/login`
2. 6-digit OTP sent via Supabase
3. `/verify`
4. Secure session cookies
5. Protected routes require auth

No password auth unless explicitly approved.

---

# Daily Git Workflow

1. Create branch:

   ```
   feature/<slug>
   ```
2. Implement via Codex
3. Commit:

   ```
   feat(PROJ-X): description
   ```
4. Push
5. Open PR
6. Vercel creates Preview
7. CI runs:

   * lint
   * typecheck
   * Playwright
8. Merge only if green

Only PRs ship changes.

---

# Definition of Done

A feature is complete only if:

* Acceptance criteria implemented
* RLS enforced
* No Critical/High QA bugs
* Playwright updated
* CI passes
* Feature file updated
* INDEX.md updated
* Production verified

---

# Scripts

```bash
npm run dev
npm run build
npm run lint
npm run start
```

---

# Tooling Stack

| Layer           | Tool                           |
| --------------- | ------------------------------ |
| UI              | Next.js + Tailwind + shadcn/ui |
| Backend         | Supabase                       |
| Email           | Resend                         |
| Domain          | Porkbun                        |
| CI              | GitHub Actions                 |
| Hosting         | Vercel                         |
| Testing         | Playwright                     |
| Coding Operator | Codex                          |

---

# Context Discipline

This system is designed for deterministic AI development.

Principles:

* State lives in files, not memory.
* Codex must read before editing.
* No guessing.
* No phase skipping.
* No direct production edits.
* PR-based shipping only.

---

# How To Add A Feature

1. Update `features/INDEX.md`
2. Create `features/PROJ-X-<slug>.md`
3. Write requirements
4. Move through all phases
5. Merge PR

---

# Governance

All operational rules are defined in: AGENTS.md
