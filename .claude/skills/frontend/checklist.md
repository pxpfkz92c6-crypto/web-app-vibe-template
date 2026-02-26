# Frontend Implementation Checklist (Codex + Next.js App Router + Tailwind + shadcn/ui)

Use this checklist for every frontend feature implementation in the standard setup:
Next.js (App Router) + TypeScript + Supabase (Email OTP + RLS-by-default) + Playwright + GitHub PR + Vercel.

---

## 1) Pre-Flight (Before Writing Anything)
- [ ] Read `AGENTS.md` (root) + `app/AGENTS.md` (UI/brand conventions)
- [ ] Read the active feature file `features/PROJ-X-*.md` (incl. Tech Design + ACs)
- [ ] Check `features/INDEX.md` for related features + regression risk
- [ ] Inspect existing UI patterns before creating anything new:
  - [ ] Routes: `git ls-files app/ || git ls-files src/app/`
  - [ ] Components: `git ls-files components/ || git ls-files src/components/`
  - [ ] Hooks: `git ls-files hooks/ || git ls-files src/hooks/`
  - [ ] Shared libs: `git ls-files lib/ || git ls-files src/lib/`

---

## 2) shadcn/ui (Non-Negotiable)
- [ ] Checked shadcn/ui for EVERY UI component needed
  - UI folder is usually: `components/ui/` (or `src/components/ui/` if repo uses `src/`)
- [ ] No custom duplicates of shadcn components created
- [ ] Missing shadcn components installed via:
  - [ ] `npx shadcn@latest add <component> --yes`

---

## 3) Design & UX Alignment
- [ ] Follow the component architecture from Solution Architect (feature file Tech Design)
- [ ] If no mockups/design guidance exists, clarify ONLY blocking UI decisions:
  - [ ] Layout (sidebar vs top-nav vs simple page)
  - [ ] Key interactions (search/filter/sort, pagination, drag & drop)
  - [ ] Any required brand tokens (from `app/AGENTS.md`)

---

## 4) Implementation (Quality Baseline)
- [ ] All planned routes/screens implemented (App Router conventions)
- [ ] All planned components implemented (reuse where possible)
- [ ] Tailwind CSS only:
  - [ ] No inline styles
  - [ ] No CSS modules
  - [ ] No ad-hoc custom CSS (unless template already includes it)
- [ ] All data-driven UI includes:
  - [ ] Loading state (spinner/skeleton)
  - [ ] Error state (user-friendly message)
  - [ ] Empty state (“No data yet” with guidance)
- [ ] Auth-aware UI behavior (if applicable):
  - [ ] Protected routes enforce auth UX (redirect or gated UI)
  - [ ] Unauthorized states handled gracefully
  - [ ] No secrets/service-role keys anywhere client-side

---

## 5) Responsive + Accessibility
- [ ] Responsive verified at:
  - [ ] 375px (mobile)
  - [ ] 768px (tablet)
  - [ ] 1440px (desktop)
- [ ] Accessibility baseline:
  - [ ] Semantic HTML
  - [ ] Labels for inputs
  - [ ] ARIA where needed
  - [ ] Keyboard navigation works for key flows

---

## 6) Verification (Must Run Before “Done”)
- [ ] `npm run build` passes
- [ ] `npm run lint` passes (if configured)
- [ ] No TypeScript errors
- [ ] All acceptance criteria from the feature file are addressed in the UI
- [ ] `features/INDEX.md` status updated to 🟡 In Progress (when work starts)
- [ ] Feature file updated with **Implementation Notes (Frontend)** (paths only)

---

## 7) Completion Gate
- [ ] User reviewed and approved UI in browser (local or Preview)
- [ ] Code committed to git with:
  - [ ] `feat(PROJ-X): implement frontend for <feature name>`

---

## Optional (Only If Spec Requires It)
- [ ] Playwright E2E tests updated/added for new UI flows
- [ ] No regressions in shared navigation/layout
