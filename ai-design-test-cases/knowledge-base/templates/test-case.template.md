# Test Case Template

> Purpose: Blank structure for writing a test case. See [Login Test Case Example](../examples/test-case-login-example.md) for a filled-in reference, and [Test Design Techniques](../standards/test-design-techniques.md) for how to choose coverage.

---

**Test Case ID:** TC-\<UC ID\>-\<sequence\>
**Title:** \<States the scenario and expected behavior — not "Test 1"\>
**Source Requirement:** `<UC-ID>-<Use-Case-Name>` (`requirements/<feature-area>/<UC-ID>-<Use-Case-Name>.md`)
**Priority:** \<P0 / P1 / P2 — per [Test Strategy](../project-knowledge/test-strategy.md) risk-based approach\>
**Type:** \<Functional — Positive / Negative / Boundary | Non-Functional — Performance / Security / Accessibility\>
**Tags:** \<e.g. `@smoke`, `@regression`, `@<feature-area>`\>

**Preconditions:**
- \<State the required starting state — test data, user role/session, environment. Must be reproducible without guessing.\>

**Test Data:**
| Field | Value |
|---|---|
| \<field\> | \<value or reference to test data store\> |

**Steps:**

| # | Step | Expected Result |
|---|---|---|
| 1 | \<Single, unambiguous action\> | \<Specific, observable, verifiable outcome — not "works correctly"\> |

**Postconditions:**
- \<Any resulting state change — session, data, downstream effect.\>

---

## Notes

- One scenario per test case — split compound scenarios into separate test case IDs (e.g. `TC-US001-01`, `TC-US001-02`).
- Every acceptance criterion this test case covers should be traceable back to the source requirement or its [Requirement Analysis Report](../../analysis-reports).
- Before marking this test case ready for review, self-check it against the [Test Case Review Checklist](../checklists/test-case-review-checklist.md).
- Do not include real secrets/credentials in Test Data — reference a test data store or factory instead.
