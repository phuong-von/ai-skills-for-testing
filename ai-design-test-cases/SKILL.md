---
name: ai-design-test-cases
description: Generate test cases from an analyzed requirement, applying approved test design techniques for complete positive, negative, and boundary coverage.
---

# Skill: AI Design Test Cases

> **Path convention:** this packaged skill bundles its own reference copy of the knowledge base it needs (`knowledge-base/...`, sibling to this file) so it works standalone right after install — no project setup required. If you're instead running it from inside this project's own `skills/ai-design-test-cases/SKILL.md` (2 folders below the project root, per [Project Structure](knowledge-base/project-knowledge/project-structure.md)), prefer the project's live copy at `../../knowledge-base/...` when it exists — it's likely more current or customized than this bundled snapshot. Output paths like `test-cases/` below are relative to whichever project you're running this skill in — create/use them at that project's root, not inside this skill's own installed folder.

---

# 1. Purpose

Generate a complete, traceable set of test cases from an analyzed requirement, using approved test design techniques so coverage is systematic rather than guessed.

---

# 2. When to Use

Use this skill when you need to:

- Turn a requirement analysis report (or a requirement already confirmed Ready) into test cases before automation or manual execution.
- Add test case coverage for a new feature or a changed acceptance criterion.

**Manual execution:**
```
/ai-design-test-cases analysis-reports/RA-US001-User-Login.md
```

---

# 3. Do Not Use When

This skill is NOT responsible for:

- Analyzing requirement completeness or testability (use `ai-analyze-requirements` instead)
- Writing Playwright automation scripts from a test case (use `ai-implement-automation` instead)
- Generating Page Object classes (use `ai-generate-page-object` instead)
- Reviewing or approving existing test cases (use `ai-review-test-case` instead)
- Minor wording or formatting fixes to existing test cases
- Rewriting the source requirement itself

---

# 4. Inputs

## Required

- One requirement analysis report. (output of `ai-analyze-requirements`)

  Example: `analysis-reports/RA-US001-User-Login.md`

  If no analysis report exists, ask the user whether to run `ai-analyze-requirements` first — do not design test cases against a requirement that hasn't been checked for completeness/testability gaps.

## Optional

- **Design Scope** — `functional`, `non-functional` (performance/security/accessibility), or `both`. If not provided, default to `functional` and note that non-functional cases were out of scope.
- Preferred test design technique(s), if the user wants a specific one applied (otherwise the AI selects per Section 7).

### Analysis Report Status Check

1. If the source RA report's status is **Ready** → proceed.
2. If **Not Ready** with unresolved blocking clarifications → stop and tell the user which clarifications are still open; do not guess at the missing behavior.
3. If no RA report is provided at all → ask the user to confirm whether to proceed directly from the raw requirement (acceptable for quick/low-risk cases) or run `ai-analyze-requirements` first.

---

# 5. Outputs

Generate a single Markdown test case document per requirement, containing all designed test cases for that requirement.

The generated file shall:
- Be saved in the `test-cases/` folder.
- Contain a numbered list of test cases, each with: ID, title, preconditions, steps, expected results, and a traceability link back to the specific acceptance criterion it covers.

## Output File Naming Convention

File location
```
test-cases/
```

File naming convention
```
TC-<UC ID>-<Use Case Name>.md
```

Rules
- Prefix with `TC-` (Test Case).
- Use the Use Case ID exactly as provided in the source requirement/RA report.
- Use the Use Case Name, spaces replaced with hyphens, original word order preserved.

Examples
```
TC-US001-User-Login.md
TC-US102-Create-User.md
```

### Individual Test Case ID Format

Within the file, each test case gets its own ID: `TC-<UC ID>-<sequence>`, e.g. `TC-US001-01`, `TC-US001-02` — matching the format used in [test-case-login-example.md](knowledge-base/examples/test-case-login-example.md).

---

# 6. Workflow

High-level workflow only:

```
Load Knowledge
        │
        ▼
Read Input (RA report / requirement)
        │
        ▼
Check RA Status (Ready / Not Ready)
        │
        ▼
Select Test Design Technique(s)
        │
        ▼
Draft Test Cases (positive, negative, boundary)
        │
        ▼
Map Each Case to its Acceptance Criterion
        │
        ▼
Self-Validate
        │
        ▼
Generate Output (Test Case Document)
```

---

# 7. Knowledge Sources

Before designing test cases, load:

## Standards
- [Test Design Techniques](knowledge-base/standards/test-design-techniques.md)
- [Requirement Quality Standard](knowledge-base/standards/requirement-quality-standard.md) (Given/When/Then format — test cases trace back to these ACs)
- [Testing Standard](knowledge-base/standards/testing-standard.md) (Test Naming Conventions section)

## Project Knowledge
- [Test Strategy](knowledge-base/project-knowledge/test-strategy.md)

## Templates
- [Test Case Template](knowledge-base/templates/test-case.template.md)

## Checklists
- [Test Case Review Checklist](knowledge-base/checklists/test-case-review-checklist.md)
- [Business Rule Checklist](knowledge-base/checklists/business-rule-checklist.md) (when the requirement contains computed/guard rules)

## Examples
- [Login Test Case Example](knowledge-base/examples/test-case-login-example.md)

---

# 8. Execution Rules

Execution sequence:

1. Read the requirement analysis report (or the raw requirement, if the user explicitly opted to skip analysis).
2. Load all Knowledge Sources listed above.
3. Run the Analysis Report Status Check (Section 4). Stop and ask if blocked.
4. For each acceptance criterion, select the applicable technique(s) from [Test Design Techniques](knowledge-base/standards/test-design-techniques.md) using its "Choosing a Technique" table (e.g. a range/threshold → Equivalence Partitioning + Boundary Value Analysis; multiple independent conditions → Decision Tables; a lifecycle/workflow → State Transition Testing).
5. Design test cases covering, for every acceptance criterion: the happy path, at least one negative/error case, and any boundary or edge case implied by the criterion — do not stop at the happy path.
6. If the requirement contains a computed or guard rule (e.g. an action blocked by a derived condition), apply the [Business Rule Checklist](knowledge-base/checklists/business-rule-checklist.md) to make sure the rule's trigger, non-trigger, and boundary conditions are each covered by a case.
7. Assign each test case a unique ID (`TC-<UC ID>-<sequence>`) and an explicit traceability reference to the acceptance criterion or finding it covers.
8. Follow the Testing Standard's naming conventions when titling each test case (state the scenario and expected behavior, not "Test 1").
9. Execute Self-Validation (Section 9) before producing the final output.

General rules:
- The AI must behave as a deterministic test designer, not a requirement rewriter — do NOT alter the requirement's intent to make it easier to test.
- Every test case must state observable, verifiable expected results (per the Requirement Quality Standard's Testability Requirements) — not vague outcomes like "works correctly."
- Every test case must trace to at least one acceptance criterion or explicitly note it covers a gap flagged in the RA report (e.g. a missing negative scenario).
- Flag any acceptance criterion that cannot be made testable as designed, rather than inventing an assumption to force a test case.

### Coverage Expectations

- Every acceptance criterion has at least one positive (happy path) test case.
- Every acceptance criterion with an error condition, permission rule, or validation rule has at least one negative test case.
- Every numeric range, threshold, character limit, or date boundary has Boundary Value Analysis cases at and around the boundary.
- Every multi-condition business rule has Decision Table coverage for the combinations that matter (not necessarily every mathematically possible combination — see Anti-Fixed-Quota Rule).
- Every stateful workflow has valid and invalid transition cases.
- Cross-browser/cross-platform, performance, security, or accessibility cases are included only when the requirement or [Test Strategy](knowledge-base/project-knowledge/test-strategy.md) calls for them, and Design Scope includes non-functional.

### Anti-Fixed-Quota Rule

The AI MUST NOT:
- Use a fixed number of test cases per requirement or per acceptance criterion.
- Force equal case counts across all acceptance criteria.
- Inflate case count with redundant variations that don't add new coverage, or under-cover a criterion to save time.

All test case design MUST be based on: the actual behavior and risk of each acceptance criterion, applicable test design technique(s), and the Coverage Expectations above — not a target number.

---

# 9. Self-Validation

Mandatory validation before output. Before generating the final test case document, execute the [Test Case Review Checklist](knowledge-base/checklists/test-case-review-checklist.md) and confirm:
- Every acceptance criterion in the source requirement has at least one test case.
- Positive, negative, and boundary cases are present where applicable (Section 8 Coverage Expectations).
- Every test case has a unique ID, clear title, preconditions, steps, expected results, and a traceability reference.
- Business Rule Checklist applied where the requirement contains computed/guard rules.
- Naming conventions followed (Testing Standard).

If the draft fails self-check:
- AI MUST revise internally.
- AI MUST NOT output partial results.
- AI MUST NOT expose reasoning or checklist.
- Fix any failures before saving. Do not generate a review-of-the-review report. Only use the checklist internally to verify quality.

---

# 10. Success Criteria

- Every acceptance criterion in the source requirement is covered by at least one test case.
- All items in the [Test Case Review Checklist](knowledge-base/checklists/test-case-review-checklist.md) are satisfied.
- Every test case has observable, verifiable expected results and a traceability link.
- No fixed-quota shortcuts were used — case count and technique selection match actual requirement complexity and risk.
- The output file 