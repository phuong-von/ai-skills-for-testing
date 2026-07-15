---
name: ai-review-automation
description: Review a Playwright automation test script against the project's coding, Playwright, and locator standards before merge.
---

# Skill: AI Review Automation

> **Path convention:** this packaged skill bundles its own reference copy of the knowledge base it needs (`knowledge-base/...`, sibling to this file) so it works standalone right after install — no project setup required. If you're instead running it from inside this project's own `skills/ai-review-automation/SKILL.md` (2 folders below the project root, per [Project Structure](knowledge-base/project-knowledge/project-structure.md)), prefer the project's live copy at `../../knowledge-base/...` when it exists — it's likely more current or customized than this bundled snapshot. Output paths like `automation-reviews/` below are relative to whichever project you're running this skill in — create/use them at that project's root, not inside this skill's own installed folder.

---

# 1. Purpose

Review a Playwright automation test script for coding standards, locator/Page Object usage, error handling, maintainability, and traceability, and produce a review report with a clear merge/no-merge outcome.

---

# 2. When to Use

Use this skill when you need to:

- Review a new or changed automation test script before it's merged.
- Re-review a script after it's been revised in response to prior feedback.

**Manual execution:**
```
/ai-review-automation tests/authentication/login.spec.ts
```

---

# 3. Do Not Use When

This skill is NOT responsible for:

- Writing new automation scripts (use `ai-implement-automation` instead)
- Reviewing a Page Object class in isolation (use `ai-review-page-object` instead)
- Designing or reviewing test cases (use `ai-design-test-cases` / `ai-review-test-case` instead)
- Generating Page Objects (use `ai-generate-page-object` instead)
- Rewriting or fixing the script on the author's behalf — this skill reports findings; fixing the code is a separate step
- Standalone/portable Playwright review outside this project's `knowledge-base/` (use the self-contained `review-playwright-auto` skill instead)

---

# 4. Inputs

## Required

- One Playwright automation test script.

  Example: `tests/authentication/login.spec.ts`

## Optional

- The source test case(s) it implements, for traceability verification.

  Example: `test-cases/TC-US001-User-Login.md`

If no source test case is provided, traceability is checked against the script's own stated test case ID reference (comment/title/tag) only, and this limitation is noted in the output.

---

# 5. Outputs

Generate a single Markdown review report for the script reviewed.

The generated file shall:
- Be saved in the `automation-reviews/` folder.
- Contain specific, evidence-based review comments per issue found, an issue table (severity + description), and a clear outcome.

## Output File Naming Convention

File location
```
automation-reviews/
```

File naming convention
```
AR-<script-name>.md
```

Rules
- Prefix with `AR-` (Automation Review).
- `<script-name>` derived from the reviewed file name, hyphens replacing dots/slashes.

Examples
```
AR-login-spec.md
```

---

# 6. Workflow

High-level workflow only:

```
Load Knowledge
        │
        ▼
Read Input (automation script + optional test case)
        │
        ▼
Execute Automation Review Checklist (all 5 sections)
        │
        ▼
Identify Issues
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

Before reviewing an automation script, load:

## Standards
- [Automation Coding Standard](knowledge-base/standards/automation-coding-standard.md)
- [Playwright Standard](knowledge-base/standards/playwright-standard.md)
- [Locator Standard](knowledge-base/standards/locator-standard.md)
- [POM Standard](knowledge-base/standards/pom-standard.md)
- [Testing Standard](knowledge-base/standards/testing-standard.md)

## Project Knowledge
- [Test Strategy](knowledge-base/project-knowledge/test-strategy.md) (tagging/CI suite expectations)

## Checklists
- [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md)

## Examples
- [Test Case Review Example](knowledge-base/examples/review-test-case-example.md) (review report structure/tone to mirror)

---

# 8. Execution Rules

Execution sequence:

1. Read the automation script fully, plus the source test case(s) if provided.
2. Load all Knowledge Sources listed above.
3. Execute the [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) in full:
   - Coding Standards Compliance
   - Locator & Page Object Compliance
   - Error Handling Check
   - Maintainability Review
   - Traceability
4. For each finding, write a specific, evidence-based comment referencing the exact line/block — not a generic "needs cleanup."
5. Classify each finding by severity: **Blocking** (e.g. hardcoded secrets, inline locators bypassing an existing Page Object, arbitrary `waitForTimeout` with no justification, test depends on execution order) vs. **Minor** (e.g. naming clarity, an extractable duplicate).
6. Compile findings into an issue table (checklist item, severity, issue), and determine the outcome: **Approved** (no blocking issues) or **Needs revision** (list what must change) — never partial approval.
7. Execute Self-Validation (Section 9) before producing the final report.

General rules:
- The AI must behave as a reviewer, not a rewriter — do NOT silently fix the script; report the issue and let a human or `ai-implement-automation` make the change.
- If the script deviates from its source test case (steps combined, a scenario skipped) and the deviation isn't explained, flag it as a Traceability finding.
- Do not flag the same issue more than once across different checklist sections.

---

# 9. Self-Validation

Mandatory validation before output. Before generating the final review report, confirm:
- All 5 sections of the [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md) were executed.
- Every finding has a severity and references a specific line/block.
- A single, unambiguous outcome (Approved / Needs revision) was assigned.
- No finding is duplicated across sections.

If the review fails self-check:
- AI MUST revise internally.
- AI MUST NOT output partial results.
- AI MUST NOT expose reasoning or checklist.
- Fix any failures before saving. Do not generate a review-of-the-review report. Only use the checklist internally to verify quality.

---

# 10. Success Criteria

- The script has been reviewed against all 5 sections of the [Automation Review Checklist](knowledge-base/checklists/automation-review-checklist.md).
- Every finding is specific, evidence-based, and severity-classified.
- The script has a clear, unambiguous outcome.
- The output file is saved to `automation-reviews/` following the `AR-<script-name>.md` naming convention.
