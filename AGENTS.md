---

# AGENTS.md

**Codex AI Coding Starter Kit — Standard Operating Manual**

---

## 1. Purpose

This repository follows a structured, Codex-driven development workflow.

All features must follow:

* Feature-Files lifecycle
* RLS-by-default enforcement
* PR-based deployment discipline
* CI + Playwright gating
* Human-in-the-loop checkpoints

No feature may bypass workflow phases.

---

## 2. Standard Stack (Non-Negotiable)

* **Next.js (App Router) + TypeScript**
* **Supabase (Postgres + Email OTP + RLS-by-default)**
* **Tailwind CSS + shadcn/ui**
* **Playwright (E2E)**
* **GitHub PR workflow**
* **Vercel (Preview → Production)**

Optional integrations:

* Resend (transactional email)
* Porkbun (DNS)

---

## 3. Repo Structure (Required)

```
app/                     # Routes + Server Actions
lib/                     # Utilities (supabase client, helpers)
supabase/migrations/     # Database schema source of truth
tests/e2e/               # Playwright tests
features/                # Feature specs
  INDEX.md               # Feature status tracker
docs/
  PRD.md                 # Product Requirements Document
AGENTS.md                # This file
app/AGENTS.md            # UI rules
supabase/AGENTS.md       # Migration + RLS rules
tests/AGENTS.md          # Playwright rules
```

Codex must never invent alternative structure.

---

## 4. Feature-Files System (Single Source of Truth)

Every feature:

`features/PROJ-X-<slug>.md`

Each file accumulates:

1. Requirements
2. Architecture
3. QA results
4. Deployment record

Status lifecycle:

* 🔵 Planned
* 🟡 In Progress
* ✅ Deployed

All skills must read `features/INDEX.md` before operating.

---

## 5. RLS-by-Default (Critical Security Rule)

Every user-owned table must:

* Include `user_id uuid references auth.users(id)`
* Enable RLS
* Define policies for:

  * SELECT
  * INSERT
  * UPDATE
  * DELETE
* Restrict access via:

  ```
  user_id = auth.uid()
  ```

No feature may weaken isolation.

RLS violations = Critical severity.

---

## 6. Authentication Standard

Auth method:

* Email OTP only

Flow:

1. `/login` → enter email
2. Supabase sends 6-digit code
3. `/verify` → enter code
4. Session via secure cookies
5. Protected routes enforce session

No password auth unless explicitly approved.

---

## 7. Codex Skill Team (Prompt Modes)

The system uses structured prompt roles:

* Requirements Engineer
* Solution Architect
* Frontend Developer
* Backend Developer
* QA Engineer
* DevOps
* Help

These are prompt modes, not commands.

Each role:

* Reads project context
* Operates only within its phase
* Requires user review before next phase

No phase skipping allowed.

---

## 8. Daily Development Flow (Enforced)

1. Write/update feature spec
2. Create `feature/<slug>` branch
3. Codex implements changes
4. Commit with:

   ```
   feat(PROJ-X): description
   ```
5. Push branch
6. Open PR
7. Vercel creates Preview
8. CI runs:

   * lint
   * typecheck
   * Playwright E2E
9. If green → merge to `main`
10. Vercel deploys Production

Only PRs ship code.

---

## 9. Definition of Done

A feature is complete only if:

* Acceptance criteria fully implemented
* RLS policies enforced
* No Critical/High QA bugs
* Playwright tests updated
* CI passes
* Feature file updated with deployment info
* INDEX.md updated to ✅ Deployed
* Production verified

---

## 10. Security Rules

* No service-role keys client-side
* No secrets in git
* No bypassing RLS
* No unvalidated write endpoints
* Zod required for POST/PUT validation

---

## 11. Testing Rules

Every feature must:

* Have Playwright coverage for core flow
* Test auth isolation if user-owned
* Pass E2E before merge
* Avoid external network calls in tests

---

## 12. Commit Conventions

```
feat(PROJ-X): add feature
fix(PROJ-X): fix bug
test(PROJ-X): add QA results
deploy(PROJ-X): release feature
docs(PROJ-X): update spec
```

---

## 13. State Discipline

All features must follow:

Requirements → Architecture → Frontend → Backend → QA → Deploy

Skipping phases is prohibited.

---

## 14. Human-in-the-Loop Rule

No role may:

* Auto-approve its own output
* Move to next phase without confirmation
* Modify production logic during QA
* Deploy without QA approval

---
