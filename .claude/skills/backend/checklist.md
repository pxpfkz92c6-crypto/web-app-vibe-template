# Backend Implementation Checklist (Codex + Next.js App Router + Supabase RLS)

Use this checklist for every backend feature implementation in this standard setup:
Next.js (App Router) + TypeScript + Supabase (Email OTP + Postgres + RLS-by-default) + Playwright + GitHub PR + Vercel.

---

## 1) Pre-Flight (Before Writing Anything)
- [ ] Read `AGENTS.md` (root) + `supabase/AGENTS.md` (migrations/RLS rules)
- [ ] Read the active feature file `features/PROJ-X-*.md` (incl. Tech Design)
- [ ] Check `features/INDEX.md` for related features + regression risk
- [ ] Inspect existing patterns before creating anything new:
  - [ ] `git ls-files supabase/migrations/`
  - [ ] `git ls-files app/**/route.ts || true`
  - [ ] `git ls-files app/**/actions.ts || true`
  - [ ] `git ls-files lib/ || true` and/or `git ls-files src/lib/ || true`

---

## 2) Database & Security (Supabase) — Non-Negotiable
- [ ] All schema changes are captured in `supabase/migrations/` (source of truth)
- [ ] Every user-owned table includes `user_id uuid references auth.users(id) not null`
- [ ] Row Level Security enabled on ALL new user-owned tables
- [ ] RLS policies exist for **SELECT / INSERT / UPDATE / DELETE**
- [ ] Policies enforce: `user_id = auth.uid()`
- [ ] Foreign keys use correct `ON DELETE` behavior (CASCADE/RESTRICT/SET NULL as appropriate)
- [ ] Indexes added for performance-critical columns:
  - [ ] `user_id`
  - [ ] foreign keys
  - [ ] frequently filtered/sorted columns (e.g., `created_at`, status)
- [ ] No RLS bypasses introduced (no “admin” shortcuts unless explicitly designed and server-only)

---

## 3) Server Logic (Next.js App Router)
- [ ] Uses repo’s established pattern:
  - [ ] Server Actions **or** Route Handlers (`app/**/route.ts`)
- [ ] Authentication is enforced before returning or mutating user data
- [ ] All write inputs validated (Zod or repo standard) for:
  - [ ] create
  - [ ] update
  - [ ] delete (when applicable)
- [ ] Error handling is user-safe (no sensitive info, no leaking internals)
- [ ] No secrets committed; service-role keys remain server-only
- [ ] No N+1 query loops (use joins/batched queries where appropriate)
- [ ] All list endpoints/queries use pagination/limits (avoid unbounded reads)

---

## 4) Integration (Frontend Wiring)
- [ ] Any mock/local-only data is replaced with real backend calls (per repo pattern)
- [ ] Frontend loading/error/empty states remain correct (not removed)
- [ ] Behavior matches Acceptance Criteria exactly (no invented functionality)

---

## 5) Testing (Playwright + Regression)
- [ ] Playwright E2E updated/added if user flows are affected
- [ ] Tests cover:
  - [ ] OTP login (if relevant)
  - [ ] happy-path CRUD
  - [ ] authorization isolation: user A cannot see user B’s data
- [ ] No external network calls in E2E tests (per `tests/AGENTS.md`)
- [ ] Regression spot-check for related deployed features from `features/INDEX.md`

---

## 6) Verification (Before Marking Complete)
- [ ] `npm run build` passes
- [ ] Lint/typecheck passes (per repo commands)
- [ ] Playwright E2E passes (locally and/or CI)
- [ ] Manual sanity test of the core flow (happy path + one key edge case)
- [ ] All Acceptance Criteria addressed (explicitly confirmed against the feature file)

---

## 7) Documentation & Tracking (Required)
- [ ] Feature file updated with **Implementation Notes (Backend)**:
  - [ ] migrations added (names)
  - [ ] tables/policies touched
  - [ ] server actions/route handlers touched (paths only)
  - [ ] assumptions/constraints (if any)
- [ ] `features/INDEX.md` status updated appropriately:
  - [ ] 🟡 In Progress when work starts
  - [ ] ✅ Deployed only after deploy verification (handled by deploy step)
- [ ] Code committed with correct message:
  - [ ] `feat(PROJ-X): implement backend for <feature name>`

---

## 8) Optional (Only If Spec Requires It)
- [ ] Rate limiting for public endpoints
- [ ] Caching for expensive reads
- [ ] Audit logging / activity feed tables (still RLS-by-default)
- [ ] Resend email triggers (server-side only, DNS assumed configured)
