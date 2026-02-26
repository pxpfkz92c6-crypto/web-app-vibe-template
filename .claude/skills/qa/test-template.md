---
# QA Test Results Template (Codex + Feature-Files Standard)
Add this section to the **end** of:
features/PROJ-X-<slug>.md
---

## QA Test Results (QA Engineer)

**Tested:** YYYY-MM-DD  
**Environment:** Localhost | Vercel Preview URL  
**Feature Branch:** feature/<name>  
**Tester:** QA Engineer (Codex Mode)

---

### 1. Acceptance Criteria Verification

#### AC-1: [Criterion Name]
- [x] Sub-criterion passed
- [ ] BUG: Sub-criterion failed  
  - Expected:  
  - Actual:  

#### AC-2: [Criterion Name]
- [x] All sub-criteria passed

> Every acceptance criterion must be explicitly marked pass or fail.

---

### 2. Edge Case Verification

#### EC-1: [Edge Case Name]
- [x] Handled correctly

#### EC-2: [Edge Case Name]
- [ ] BUG: Not handled  
  - Expected behavior:  
  - Actual behavior:  

> Include undocumented edge cases discovered during testing.

---

### 3. Security Audit (Red-Team Perspective)

- [x] Authentication enforced (protected routes blocked when logged out)
- [x] Authorization enforced (User A cannot access User B data)
- [x] RLS policies validated (cross-user DB queries blocked)
- [x] Input validation enforced (no XSS via inputs)
- [x] No service-role or private secrets exposed client-side
- [x] API responses contain no sensitive fields
- [ ] BUG: [Security issue description]

If RLS failure occurs → **Severity = Critical**

---

### 4. Regression Testing

Tested deployed features listed in `features/INDEX.md`:

- [x] Auth flow (Email OTP)
- [x] Core navigation
- [x] Related CRUD flows
- [ ] BUG: Regression detected in [feature name]

---

### 5. Bugs Found

#### BUG-1: [Bug Title]

- **Severity:** Critical | High | Medium | Low  
- **Priority:** P1 | P2 | P3  
- **Area:** Frontend | Backend | RLS | Auth | Deploy | Tests  

**Steps to Reproduce:**
1.  
2.  
3.  

**Expected:**  
**Actual:**  

**Security Impact:** (if applicable)  

---

### 6. Summary

- **Acceptance Criteria:** X / Y passed  
- **Edge Cases:** X / Y passed  
- **Bugs Found:** N total  
  - Critical: C  
  - High: H  
  - Medium: M  
  - Low: L  

- **Security:** Pass | Issues Found  
- **Regression:** Clean | Issues Found  

---

### 7. Production-Ready Decision

- **Production Ready:** YES | NO  
- **Reason:**  

If NO:
> Fix Critical/High bugs first, then re-run QA.

If YES:
> Feature eligible for `/deploy` via PR → Vercel → main merge.

---

# Why This Version Is Better for Your System

This updated template:

* Explicitly validates **RLS isolation**
* Includes **regression testing requirement**
* Includes **environment reference (Preview vs Local)**
* Aligns with your PR-based release gate
* Matches your Codex “skill team” structure
* Enforces production readiness discipline

---

If you want, I can now:

* Embed this directly into your Canvas document as an official QA appendix
* Or create a standardized `tests/QA_TEMPLATE.md` spec for your repo template
