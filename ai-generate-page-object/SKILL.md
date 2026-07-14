---
name: ai-generate-page-object
description: Generate a Playwright Page Object class for a page or UI section, following the project's POM and locator standards.
---

# Skill: AI Generate Page Object

> **Path convention:** this packaged skill bundles its own reference copy of the knowledge base it needs (`knowledge-base/...`, sibling to this file) so it works standalone right after install — no project setup required. If you're instead running it from inside this project's own `skills/ai-generate-page-object/SKILL.md` (2 folders below the project root, per [Project Structure](knowledge-base/project-knowledge/project-structure.md)), prefer the project's live copy at `../../knowledge-base/...` when it exists — it's likely more current or customized than this bundled snapshot. Output paths like `pages/` below are relative to whichever project you're running this skill in — create/use them at that project's root, not inside this skill's own installed folder.

---

# 1. Purpose

Generate a Playwright Page Object (POM) class for a page or reusable UI section, with correctly prioritized locators and user-intent-named methods, ready to be consumed by automation scripts.

---

# 2. When to Use

Use this skill when you need to:

- Create a new Page Object for a page or UI section that doesn't have one yet.
- Extend an existing Page Object with new elements/methods needed by a new test case.

**Manual execution:**
```
/ai-generate-page-object test-cases/TC-US001-User-Login.md
```

---

# 3. Do Not Use When

This skill is NOT responsible for:

- Writing the automation test script itself (use `ai-implement-automation` instead)
- Designing test cases (use `ai-design-test-cases` instead)
- Reviewing an existing Page Object for standards compliance (use `ai-review-page-object` instead)
- Adding business-outcome assertions (`expect()` calls) — those belong in the test script, not the Page Object
- Modifying the application's UI or adding `data-testid` attributes to the app itself (flag missing ones instead — see Section 8)

---

# 4. Inputs

## Required

- One test case document, OR a description of the page/UI section and the elements/actions it needs to support.

  Example: `test-cases/TC-US001-User-Login.md`

## Optional

- A list of known `data-testid` values or other stable selectors already implemented in the app, if available.
- The existing `BasePage` class or shared `Component` classes, if this page should extend/reuse them.

### Existing Page Object Check

Before creating a new file, check whether a Page Object for this page already exists in `pages/`.
1. If it exists → extend it with the new elements/methods needed; do not create a duplicate.
2. If it doesn't exist → create a new one following Section 5.

---

# 5. Outputs

Generate a single Playwright Page Object class (TypeScript).

The generated file shall:
- Be saved in the `pages/` folder, grouped by feature area.
- Contain locators declared as `readonly` class properties, initialized in the constructor.
- Contain methods named for user intent, grouped logically (Section 8).

## Output File Naming Convention

File location
```
pages/<feature-area>/
```

File naming convention
```
<PageName>Page.ts
```

Rules
- PascalCase class name, matching the file name.
- Name reflects the page or UI section, not the test that uses it (e.g. `LoginPage`, not `US001Page`).

Examples
```
pages/authentication/LoginPage.ts
pages/checkout/CheckoutPage.ts
pages/shared/NavbarComponent.ts   (Component, not Page — see Section 8)
```

---

# 6. Workflow

High-level workflow only:

```
Load Knowledge
        │
        ▼
Read Input (test case / UI description)
        │
        ▼
Check for Existing Page Object
        │
        ▼
Identify Elements & Actions
        │
        ▼
Select Locators (priority order)
        │
        ▼
Draft Class (locators + methods)
        │
        ▼
Self-Validate
        │
        ▼
Generate Output (Page Object file)
```

---

# 7. Knowledge Sources

Before generating a Page Object, load:

## Standards
- [POM Standard](knowledge-base/standards/pom-standard.md)
- [Locator Standard](knowledge-base/standards/locator-standard.md)
- [Automation Coding Standard](knowledge-base/standards/automation-coding-standard.md)

## Checklists
- [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) (Section 2: Locator & Page Object Compliance)

## Examples
- [Automation Script — User Login Example](knowledge-base/examples/tests-script-login-example.md) (`LoginPage` class)

---

# 8. Execution Rules

Execution sequence:

1. Read the input test case(s) and extract every UI element referenced and every user action performed on the page.
2. Load all Knowledge Sources listed above.
3. Run the Existing Page Object Check (Section 4). If extending, read the existing file fully before adding to it.
4. For each element, select a locator using the [Locator Standard](knowledge-base/standards/locator-standard.md) priority order: `data-testid` → semantic role/accessible name → label/placeholder/text → stable `id` → CSS (last resort) → XPath (disallowed unless justified with a comment).
5. If an element's `data-testid` (or other stable selector) is not known/provided, do not invent one — insert a `// TODO: confirm selector for <element>` comment and flag it explicitly to the user in the response, rather than guessing a brittle CSS path.
6. Group locators in the class in the order they visually appear on the page (form fields, then actions, then messages), per POM Standard Section 3.
7. Write methods:
   - Action methods (verbs, user-intent named): `login()`, not `fillFormAndClickSubmit()`.
   - Getter/state methods (`get`/`is`/`has` prefix): `getErrorText()`, `isSubmitEnabled()`.
   - A `goto()`/`open<Page>()` method as the only place this page's direct navigation happens.
   - Methods that navigate to another page return that page's Page Object, typed for fluent chaining.
8. Do NOT add `expect()` assertions about business outcomes inside the Page Object — only simple state getters (Section 8.7) are allowed; outcome assertions belong in the automation script.
9. If shared UI (navbar, modal, cookie banner) is needed, reference/create a `Component` class and hold it as a property, rather than duplicating its locators/methods inline.
10. Extend `BasePage` only for genuinely shared behavior across all pages; do not create deep inheritance chains.
11. Execute Self-Validation (Section 9) before producing the final output.

General rules:
- One Page Object per distinct page or major UI section — never one Page Object per test case.
- Follow the naming conventions in the [Automation Coding Standard](knowledge-base/standards/automation-coding-standard.md) for file, class, and variable names.
- Keep the Page Object's public surface limited to what tests actually need — don't expose raw locators for elements tests never interact with.

---

# 9. Self-Validation

Mandatory validation before output. Before generating the final Page Object, execute [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) Section 2 (Locator & Page Object Compliance) and confirm:
- Locators follow the priority order and naming patterns in the Locator Standard.
- The class follows the POM Standard (one class per page/component, methods named for user intent).
- No duplicate Page Object was created for a page that already has one.
- No business-outcome assertions were added inside the class.
- Any unresolved/unknown selectors are flagged with a `TODO` comment, not guessed.

If the draft fails self-check:
- AI MUST revise internally.
- AI MUST NOT output partial results.
- AI MUST NOT expose reasoning or checklist.
- Fix any failures before saving. Do not generate a review-of-the-review report. Only use the checklist internally to verify quality.

---

# 10. Success Criteria

- Every UI element and action referenced by the source test case(s) is represented in the Page Object.
- All applicable items in [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) Section 2 are satisfied.
- Locators use the highest-priority strategy available for each element; any unknowns are flagged, not guessed.
- No business-outcome assertions exist inside the Page Object.
- The output file is saved to `pages/<feature-area>/` following the `<PageName>Page.ts` naming convention.
