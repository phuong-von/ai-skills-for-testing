# Test Case Review Template

> Purpose: Blank structure for a test case review report. See [Test Case Review Example](../examples/review-test-case-example.md) for a filled-in reference, and [Test Case Review Checklist](../checklists/test-case-review-checklist.md) for what to check.

---

**Reviewed:** \<TC-\<UC ID\>-\<sequence\>, ...\> — list every test case ID covered by this review pass
**Reviewer:** \<name or "AI"\>
**Date:** \<date\>

## Review Comments

**\<TC-\<UC ID\>-\<sequence\> (\<short scenario label\>)\>**
- \<✅ / ⚠️ / ❌\> \<Specific, evidence-based comment referencing the exact step or expected result — not a generic "needs improvement".\>

*(Repeat this block for every test case reviewed.)*

## Issue Identification Summary

| # | Test Case | Severity | Issue |
|---|---|---|---|
| 1 | \<TC-ID\> | \<Blocking / Minor\> | \<Specific issue\> |

## Improvement Suggestions

- \<Concrete, actionable suggestion tied to a specific issue above — not a vague "improve clarity".\>

## Approval Criteria

Per the [Test Case Review Checklist](../checklists/test-case-review-checklist.md) Outcome section:

- [ ] **Approved** — all sections of the checklist pass for this test case.
- [ ] **Needs revision** — list what must change before it can be approved.

A test case is only approved when every applicable checklist item passes — no partial approval ("approved except for X"); split into what's ready now vs. blocked instead.
