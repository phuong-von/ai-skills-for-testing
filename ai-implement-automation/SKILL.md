---
name: ai-implement-automation
description: Convert an approved test case into a Playwright automation script, using existing Page Objects and project automation standards.
---

# Skill: AI Implement Automation

> **Path convention:** this packaged skill bundles its own reference copy of the knowledge base it needs (`knowledge-base/...`, sibling to this file) so it works standalone right after install — no project setup required. If you're instead running it from inside this project's own `skills/ai-implement-automation/SKILL.md` (2 folders below the project root, per [Project Structure](knowledge-base/project-knowledge/project-structure.md)), prefer the project's live copy at `../../knowledge-base/...` when it exists — it's likely more current or customized than this bundled snapshot. Output paths like `tests/` below are relative to whichever project you're running this skill in — create/use them at that project's root, not inside this skill's own installed folder.

---

# 1. Purpose

Convert an approved test case into a Playwright automation script that consumes existing Page Objects, following the project's coding, Playwright, and testing standards.

---

# 2. When to Use

Use this skill when you need to:

- Automate a test case that already has (or can reuse) a Page Object for the page(s) involved.
- Add new automated coverage for a scenario currently only executed manually.

**Manual execution:**
```
/ai-implement-automation test-cases/TC-US001-User-Login.md
```

---

# 3. Do Not Use When

This skill is NOT responsible for:

- Designing the test case itself (use `ai-design-test-cases` instead)
- Creating or updating the Page Object(s) the script depends on (use `ai-generate-page-object` instead)
- Reviewing existing automation code for standards compliance (use `ai-review-automation` instead)
- Reviewing an existing Page Object in isolation (use `ai-review-page-object` instead)
- Standalone/portable Playwright review outside this project's `knowledge-base/` (use the self-contained `review-playwright-auto` skill instead)

---

# 4. Inputs

## Required

- One approved test case document.

  Example: `test-cases/TC-US001-User-Login.md`

## Optional

- Explicit tag preference (`@smoke`, `@regression`) if it differs from the default risk-based assignment in [Test Strategy](knowledge-base/project-knowledge/test-strategy.md).

### Page Object Dependency Check

Before writing the script:
1. Identify every page/UI section the test case touches.
2. Check whether a Page Object exists for each in `pages/`.
3. If any are missing → stop and tell the user which Page Object(s) need `ai-generate-page-object` first, rather than inlining locators directly into the test.

---

# 5. Outputs

Generate a single Playwright test spec file (TypeScript).

The generated file shall:
- Be saved in the `tests/` folder, grouped by feature area.
- Import and use existing Page Objects — never query locators directly in the test file.
- Reference its source test case ID in a comment, test title, or tag.

## Output File Naming Convention

File location
```
tests/<feature-area>/
```

File naming convention
```
<feature>.spec.ts
```

Rules
- kebab-case file name, named for the feature/flow, not the individual test case ID.
- `describe` block groups by feature/flow; each `test()` title states the scenario and references its source `TC-` ID.

Examples
```
tests/authentication/login.spec.ts
tests/checkout/checkout.spec.ts
```

---

# 6. Workflow

High-level workflow only:

```
Load Knowledge
        │
        ▼
Read Input (test case)
        │
        ▼
Check Page Object Dependencies
        │
        ▼
Draft Test Structure (describe/test blocks)
        │
        ▼
Implement Steps via Page Object Methods
        │
        ▼
Implement Assertions
        │
        ▼
Self-Validate
        │
        ▼
Generate Output (Automation Script)
```

---

# 7. Knowledge Sources

Before implementing automation, load:

## Standards
- [Automation Coding Standard](knowledge-base/standards/automation-coding-standard.md)
- [Playwright Standard](knowledge-base/standards/playwright-standard.md)
- [Testing Standard](knowledge-base/standards/testing-standard.md)
- [POM Standard](knowledge-base/standards/pom-standard.md) (to consume Page Objects correctly, not duplicate their logic)

## Project Knowledge
- [Test Strategy](knowledge-base/project-knowledge/test-strategy.md) (tagging/risk-based execution guidance)

## Templates
- [Test Script Template](knowledge-base/templates/test-script.template.md)

## Checklists
- [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md)

## Examples
- [Automation Script — User Login Example](knowledge-base/examples/tests-script-login-example.md)

---

# 8. Execution Rules

Execution sequence:

1. Read the test case: preconditions, steps, expected results, and any data requirements.
2. Load all Knowledge Sources listed above.
3. Run the Page Object Dependency Check (Section 4). Stop and ask if any Page Object is missing.
4. Structure the script: one `test()` per test case scenario, grouped in a `describe` block by feature/flow, per [Testing Standard](knowledge-base/standards/testing-standard.md) naming conventions.
5. Implement each step by calling the relevant Page Object method(s) — never query a locator or interact with the DOM directly inside the test file.
6. Implement assertions using Playwright's web-first, auto-retrying `expect(locator)...` — never `expect(await locator.textContent())` or manual polling.
7. Use `page.waitForTimeout()` only as a documented last resort with a comment explaining why no better wait condition exists; prefer auto-waiting and explicit condition waits.
8. Generate test data via a factory (`create<Entity>()` in `data/`) scoped to the test, rather than a shared fixture account — tests must be safe to run in parallel.
9. Add cleanup (`afterEach`/fixture teardown) for any state the test creates.
10. Tag the test (`@smoke`, `@regression`, etc.) per the risk-based approach in [Test Strategy](knowledge-base/project-knowledge/test-strategy.md).
11. Add the required top-of-file comment: which feature/requirement/test case it covers, and any non-obvious setup assumptions.
12. Execute Self-Validation (Section 9) before producing the final output.

General rules:
- One test file per feature/flow — do not mix unrelated features in a single spec file.
- If the script must deviate from the approved test case (e.g. combining steps, skipping a scenario), state the deviation explicitly in the output rather than silently diverging.
- No hardcoded credentials, environment URLs, or secrets — reference config/`.env`, per the project's Environment Setup Guide (`docs/setup.md`).

---

# 9. Self-Validation

Mandatory validation before output. Before generating the final script, execute the [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) in full and confirm:
- Coding Standards Compliance (naming, file organization, imports, no hardcoded secrets).
- Locator & Page Object Compliance (script uses existing Page Objects; no inline locators).
- Error Handling Check (no arbitrary waits, web-first assertions, no silent catch blocks).
- Maintainability Review (no duplicated logic, isolated test data, cleanup present, test independent).
- Traceability (references source test case ID; tagged correctly).

If the draft fails self-check:
- AI MUST revise internally.
- AI MUST NOT output partial results.
- AI MUST NOT expose reasoning or checklist.
- Fix any failures before saving. Do not generate a review-of-the-review report. Only use the checklist internally to verify quality.

---

# 10. Success Criteria

- Every step and expected result in the source test case is implemented and asserted.
- All items in the [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) are satisfied.
- The script uses existing Page Objects exclusively — no inline locators, no duplicated Page Object logic.
- The script is independent, parallel-safe, and cleans up any state it creates.
- The output