---
name: ai-review-page-object
description: Review a Playwright Page Object class against the project's POM and locator standards before merge.
---

# Skill: AI Review Page Object

> **Path convention:** this packaged skill bundles its own reference copy of the knowledge base it needs (`knowledge-base/...`, sibling to this file) so it works standalone right after install — no project setup required. If you're instead running it from inside this project's own `skills/ai-review-page-object/SKILL.md` (2 folders below the project root, per [Project Structure](knowledge-base/project-knowledge/project-structure.md)), prefer the project's live copy at `../../knowledge-base/...` when it exists — it's likely more current or customized than this bundled snapshot. Output paths like `page-object-reviews/` below are relative to whichever project you're running this skill in — create/use them at that project's root, not inside this skill's own installed folder.

---

# 1. Purpose

Review a Playwright Page Object class for locator strategy, structure, method naming, and reuse, and produce a review report with a clear merge/no-merge outcome.

---

# 2. When to Use

Use this skill when you need to:

- Review a new or changed Page Object class before it's merged.
- Re-review a Page Object after it's been revised in response to prior feedback.

**Manual execution:**
```
/ai-review-page-object pages/authentication/LoginPage.ts
```

---

# 3. Do Not Use When

This skill is NOT responsible for:

- Creating or updating a Page Object (use `ai-generate-page-object` instead)
- Reviewing the full automation test script that consumes this Page Object (use `ai-review-automation` instead)
- Designing or reviewing test cases (use `ai-design-test-cases` / `ai-review-test-case` instead)
- Rewriting or fixing the Page Object on the author's behalf — this skill reports findings; fixing the code is a separate step
- Standalone/portable Playwright review outside this project's `knowledge-base/` (use the self-contained `review-playwright-auto` skill instead)

---

# 4. Inputs

## Required

- One Page Object (or Component) class file.

  Example: `pages/authentication/LoginPage.ts`

## Optional

- The test case(s) or automation script(s) that consume this Page Object, to check the public surface actually matches what's used and nothing unused is exposed.

---

# 5. Outputs

Generate a single Markdown review report for the Page Object reviewed.

The generated file shall:
- Be saved in the `page-object-reviews/` folder.
- Contain specific, evidence-based review comments per issue found, an issue table (severity + description), and a clear outcome.

## Output File Naming Convention

File location
```
page-object-reviews/
```

File naming convention
```
POR-<PageName>Page.md
```

Rules
- Prefix with `POR-` (Page Object Review).
- `<PageName>Page` matches the reviewed class name exactly.

Examples
```
POR-LoginPage.md
```

---

# 6. Workflow

High-level workflow only:

```
Load Knowledge
        │
        ▼
Read Input (Page Object + optional consumers)
        │
        ▼
Execute Automation Review Checklist (Locator & Page Object section)
        │
        ▼
Check Structure, Naming, Reuse
        │
        ▼
Draft Review Comments + Issue Table
        │
        ▼
Determine Outcome (Approved / Needs Revision)
        │
        ▼
Self-Validate
        │
        ▼
Generate Output (Review Report)
```

---

# 7. Knowledge Sources

Before reviewing a Page Object, load:

## Standards
- [POM Standard](knowledge-base/standards/pom-standard.md)
- [Locator Standard](knowledge-base/standards/locator-standard.md)
- [Automation Coding Standard](knowledge-base/standards/automation-coding-standard.md)

## Checklists
- [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) (Section 2: Locator & Page Object Compliance, plus applicable items from Sections 1 and 4)

## Examples
- [Automation Script — User Login Example](knowledge-base/examples/tests-script-login-example.md) (`LoginPage` class as the calibration baseline)

---

# 8. Execution Rules

Execution sequence:

1. Read the Page Object class fully, plus any consumers provided.
2. Load all Knowledge Sources listed above.
3. Check locator strategy against the [Locator Standard](knowledge-base/standards/locator-standard.md) priority order for every declared locator — flag any that skip a higher-priority option without justification (e.g. CSS used where `data-testid` was available).
4. Check structure against the [POM Standard](knowledge-base/standards/pom-standard.md):
   - One class per distinct page/UI section — not one per test.
   - Locators declared once as `readonly` properties, initialized in the constructor — never re-queried inline inside methods.
   - No `expect()` business-outcome assertions inside the class — only simple state getters are allowed.
   - Shared UI fragments implemented as `Component`s and composed in, not duplicated.
   - Methods named for user intent (verbs for actions; `get`/`is`/`has` for state); navigation only via `goto()`/`open<Page>()`.
5. Check naming and file organization against the [Automation Coding Standard](knowledge-base/standards/automation-coding-standard.md) (PascalCase class, `<PageName>Page.ts` file name).
6. If consumers were provided, check that every exposed public method/locator is actually used, and that consumers aren't reaching into the class for something that should have its own method.
7. For each finding, write a specific, evidence-based comment referencing the exact locator/method — not a generic "clean this up."
8. Classify each finding by severity: **Blocking** (e.g. brittle/deep CSS chain, business-outcome assertion inside the class, duplicate Page Object for an existing page) vs. **Minor** (e.g. locator grouping order, a naming nit).
9. Compile findings into an issue table and determine the outcome: **Approved** (no blocking issues) or **Needs revision** — never partial approval.
10. Execute Self-Validation (Section 9) before producing the final report.

General rules:
- The AI must behave as a reviewer, not a rewriter — do NOT silently fix the Page Object; report the issue and let a human or `ai-generate-page-object` make the change.
- Do not flag the same issue more than once across different checklist items.

---

# 9. Self-Validation

Mandatory validation before output. Before generating the final review report, confirm:
- Every declared locator was checked against the Locator Standard priority order.
- Structure was checked against all applicable POM Standard rules (one class per page, no business assertions, method naming, composition over duplication).
- Every finding has a severity and references a specific locator/method.
- A single, unambiguous outcome (Approved / Needs revision) was assigned.

If the review fails self-check:
- AI MUST revise internally.
- AI MUST NOT output partial results.
- AI MUST NOT expose reasoning or checklist.
- Fix any failures before saving. Do not generate a review-of-the-review report. Only use the checklist internally to verify quality.

---

# 10. Success Criteria

- Every locator and method in the Page Object has been checked against the Locator and POM Standards.
- Every finding is specific, evidence-based, and severity-classified.
- The Page Object has a clear, unambiguous outcome.
- The output file is saved to `page-object-reviews/` following the `POR-<PageName>Page.md` naming convention.
