# Example: Test Case — User Login

> Purpose: Show how a well-written test case should look.

This is a worked example of a complete, well-formatted test case for the login feature — use it as a calibration reference when writing or reviewing test cases, not as a literal template (see [Test Case Template](../templates/test-case.template.md) for the blank structure).

---

**Test Case ID:** TC-US001-01
**Title:** User logs in successfully with valid email and password
**Source Requirement:** `US-001-User-Login` (`requirements/authentication/US-001-login.md`)
**Priority:** P0 (critical path)
**Type:** Functional — Positive
**Tags:** `@smoke`, `@regression`, `@authentication`

**Preconditions:**
- A registered, active user account exists with email `qa.user@example.com` and a known valid password.
- The user is logged out and on the Login page (`/login`).

**Test Data:**
| Field | Value |
|---|---|
| Email | `qa.user@example.com` |
| Password | `[valid password from test data store]` |

**Steps:**

| # | Step | Expected Result |
|---|---|---|
| 1 | Navigate to the Login page. | Login page loads, showing Email and Password fields and a "Log in" button. |
| 2 | Enter the valid email in the Email field. | Email field displays the entered value. |
| 3 | Enter the valid password in the Password field. | Password field displays masked characters. |
| 4 | Click the "Log in" button. | User is redirected to `/dashboard` within 3 seconds. |
| 5 | Observe the dashboard page. | The dashboard displays the logged-in user's name/avatar in the header, confirming an active session. |

**Postconditions:**
- A session/auth token exists for the user.
- The user's "last login" timestamp is updated (if applicable per requirement).

---

## Why This Is a Good Example

- **Traceable** — references the exact source requirement ID, not just "login".
- **One scenario per test case** — only the happy path; invalid credentials and lockout are separate test cases (`TC-US001-02`, `TC-US001-03`, etc.), not crammed into this one.
- **Specific expected results** — "redirected to `/dashboard` within 3 seconds", not "login works".
- **Test data is explicit but doesn't leak real secrets** — password is referenced from a data store, not hardcoded in plaintext in the document.
- **Tags support automation and CI** — `@smoke`/`@regression` let this test case map directly to an automated tag once implemented (see [Automation Script Example](./tests-script-login-example.md)).
- **Preconditions are reproducible** — anyone executing this test case has exactly what they need to set up state, without guessing.
