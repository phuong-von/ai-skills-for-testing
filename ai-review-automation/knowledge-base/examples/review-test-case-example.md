# Example: Test Case Review — User Login

> Purpose: Demonstrate proper test case review process.

This is a worked example of a completed review for [`TC-US001-01: User logs in successfully`](./test-case-login-example.md), following the [Test Case Review Checklist](../checklists/test-case-review-checklist.md). Use it to calibrate what a real review looks like — specific, evidence-based, and actionable.

---

**Reviewed:** `TC-US001-01`, `TC-US001-02` (invalid credentials), `TC-US001-03` (repeated failed attempts)
**Reviewer:** [QA Lead]
**Date:** [date]

## Review Comments

**TC-US001-01 (valid login)**
- ✅ Coverage, steps, and expected results all pass.
- ⚠️ **Issue:** Step 5's expected result says "confirming an active session" but doesn't state how — add an explicit check (e.g. "user's name appears in the header").
  *(Already fixed in the example — flagged here to show the kind of comment expected.)*

**TC-US001-02 (invalid credentials)**
- ✅ Covers wrong password and non-existent email as two separate steps.
- ❌ **Issue:** Expected result says "an error is shown" without specifying the message text. Per [Requirement Analysis Report `RA-US001-User-Login`](../../analysis-reports/RA-US001-User-Login.md), the exact error message is still an open clarification (generic vs. specific). **Blocking** — cannot verify a specific string until that's resolved.
- **Suggestion:** Either wait for the clarification answer, or write the expected result as "a generic error is shown, not specifying whether the email or password was wrong" if that's the intended security behavior, and confirm with the team.

**TC-US001-03 (repeated failed attempts)**
- ❌ **Issue:** No test case exists yet for account lockout after N failed attempts, even though `RA-US001-User-Login` flagged this as a required negative scenario (finding C2). Missing coverage, not just a wording issue.
- **Suggestion:** Add `TC-US001-03` covering: exactly N-1 failed attempts (still allowed), exactly N failed attempts (lockout triggers), and the unlock path, once the clarification on lockout threshold is answered.

## Issue Identification Summary

| # | Test Case | Severity | Issue |
|---|---|---|---|
| 1 | TC-US001-01 | Minor | Expected result not concretely observable |
| 2 | TC-US001-02 | Blocking | Expected error message not yet defined (open clarification) |
| 3 | TC-US001-03 | Blocking | Missing test case entirely (lockout scenario) |

## Improvement Suggestions

- Resolve the two open clarification questions from the requirement analysis before finalizing `TC-US001-02` and writing `TC-US001-03`.
- Once lockout behavior is confirmed, add the missing test case rather than folding it into an existing one — it's a distinct scenario with its own preconditions.
- Tighten TC-US001-01's step 5 expected result to name the specific visible element checked.

## Approval Criteria

Per the [Test Case Review Checklist](../checklists/test-case-review-checklist.md) Outcome section:

- [ ] **Approved** — not yet. Blocking issues remain (missing lockout coverage, undefined error message).
- [x] **Needs revision** — TC-US001-02 and TC-US001-03 must be updated once clarifications are resolved; TC-US001-01 can be approved after the minor wording fix.

A test case is only approved when every item in the checklist's applicable sections passes — partial approval ("approve except for X") is not used; instead, split into what's ready now vs. blocked.
