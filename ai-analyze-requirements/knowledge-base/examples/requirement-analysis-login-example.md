# Example: Requirement Analysis Report — User Login

> Purpose: Show what a completed requirement analysis report looks like, including a **Not Ready** outcome — use this as a calibration reference for finding real gaps rather than rubber-stamping. See [Requirement Analysis Report Template](../templates/requirement-analysis.template.md) for the blank structure.

---

# Requirement Analysis Report: US-001 - User Login

**Source requirement:** `requirements/authentication/US-001-login.md`
**Analysis scope:** Both (Completeness + Testability)
**Readiness status:** Not Ready

## Summary

The story covers the happy-path login and a generic invalid-credentials error, but two of its four acceptance criteria are not independently testable, and it omits negative/boundary behavior that this project's test strategy requires for auth features (account lockout, field-level validation). Clarify the items below before handing off for test design.

## Completeness Findings

| # | AC / Section | Finding | What's Missing |
|---|---|---|---|
| C1 | Story-level | No actor/role definition beyond "registered user." | Whether other roles (e.g. admin) share this login flow, and any permission differences post-login. |
| C2 | AC-2 | No AC for repeated failed login attempts. | A lockout/throttling rule (threshold + unlock method) — required for auth features per [Test Strategy](../project-knowledge/test-strategy.md). |
| C3 | Story-level | No AC for empty or malformed email/password input. | Required-field and format validation behavior (e.g. empty password, malformed email). |
| C4 | Story-level | No non-functional criteria despite this being an authentication feature. | Security expectations such as transport encryption, session timeout, no password logging. |
| C5 | Story-level | No postcondition stated. | What state is established after a successful login (e.g. session created, last-login timestamp updated). |

## Testability Findings

| # | AC / Section | Finding | Why It's Not Testable |
|---|---|---|---|
| T1 | AC-3 | "The login page should load fast" has no measurable threshold. | "Fast" is subjective; no pass/fail boundary (e.g. load time in seconds) is defined — fails the [Requirement Quality Standard](../standards/requirement-quality-standard.md)'s Testability Requirements. |
| T2 | AC-4 | "Passwords must be entered correctly" does not describe a distinct, verifiable behavior. | It overlaps with AC-1 (correct password → success) and AC-2 (incorrect password → error) without adding new observable behavior. |

## Ambiguities Detected

| # | Term / Statement | Location | Issue |
|---|---|---|---|
| A1 | "fast" | AC-3 | Vague qualifier, no defined threshold. |
| A2 | "entered correctly" | AC-4 | Vague/tautological — unclear what new condition it verifies. |
| A3 | "shows an error message" | AC-2 | Doesn't state whether the message differs for "email not found" vs "wrong password," which affects both UX and security testing (user enumeration risk). |

## Clarification Questions

### Completeness
1. What should happen after N consecutive failed login attempts — is there an account lockout or throttling, and if so, what's the threshold and how is the account unlocked? *(Relates to: C2)*
2. What validation applies when the email or password field is left empty or malformed (e.g. missing "@", no password entered)? *(Relates to: C3)*
3. Do other roles (e.g. admin) use this same login flow, and do they have different post-login destinations or permissions? *(Relates to: C1)*
4. What system state should exist after a successful login (e.g. is a session/token created, is a "last login" timestamp updated)? *(Relates to: C5)*

### Testability
1. What is the maximum acceptable load time for the login page (e.g. under 2 seconds on a standard connection)? *(Relates to: T1)*
2. AC-4 states "Passwords must be entered correctly" — what specific behavior does this verify that isn't already covered by AC-1 and AC-2? Please clarify or remove. *(Relates to: T2)*

### Business Rules
1. Does the error message in AC-2 distinguish between "email not found" and "wrong password," or is it intentionally generic for security reasons? *(Relates to: A3)*

### Non-Functional
1. Are there security requirements (e.g. HTTPS-only credential transmission, session timeout duration, no password values in logs) that should be captured as explicit non-functional criteria? *(Relates to: C4)*

## Readiness Assessment

Per the [Requirement Quality Standard](../standards/requirement-quality-standard.md) Definition of Ready (DoR) checklist:

- [x] Title, ID, and user story statement present.
- [ ] All acceptance criteria follow Given/When/Then (or documented rule format) and meet the Testability Requirements — **AC-3 and AC-4 fail this** (T1, T2).
- [ ] Actors/roles, preconditions, and postconditions stated — **no postcondition, and actor scope is unclear** (C1, C5).
- [ ] Negative and boundary scenarios covered, not just the happy path — **no lockout/throttling scenario, no field-validation scenario** (C2, C3).
- [x] Business rules stated explicitly (none apply beyond the flagged error-message ambiguity, A3).
- [ ] Out-of-scope items explicitly listed — **not present in the source requirement at all.**
- [ ] No unresolved blocking clarifications — **7 open questions above, several blocking** (C2/C3 in particular — test design cannot proceed without knowing the lockout and validation rules).

**Result: Not Ready** — route back to the requirement author via the clarification questions above before handing off to `ai-design-test-cases`. No single ambiguity changes core system behavior on its own, but the combined gaps (missing lockout rule, missing field validation, two untestable ACs) block reliable test design until clarified.
