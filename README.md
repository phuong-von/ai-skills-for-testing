# AI Skills for Testing

A collection of installable Cowork skills for QA/testing workflows — from requirement analysis through test design, Playwright automation, and code review.

Each folder is a self-contained skill: it bundles its own reference knowledge base, so it works standalone right after install.

## Install

Download the `<skill-name>.skill` file from the skill's folder and install it in Cowork via **Settings > Capabilities > Skills > Install from file**.

## Skills

### QA Workflow Chain

| Skill | Purpose |
|---|---|
| [`ai-analyze-requirements`](./ai-analyze-requirements) | Analyze user stories/requirements for completeness and testability. |
| [`ai-design-test-cases`](./ai-design-test-cases) | Generate test cases from an analyzed requirement using approved test design techniques. |
| [`ai-generate-page-object`](./ai-generate-page-object) | Generate a Playwright Page Object class following POM and locator standards. |
| [`ai-implement-automation`](./ai-implement-automation) | Convert an approved test case into a Playwright automation script. |
| [`ai-review-test-case`](./ai-review-test-case) | Review test cases for coverage, clarity, and traceability. |
| [`ai-review-automation`](./ai-review-automation) | Review a Playwright automation script against coding/Playwright/locator standards. |
| [`ai-review-page-object`](./ai-review-page-object) | Review a Page Object class against POM and locator standards. |

These 7 form a pipeline: `ai-analyze-requirements` -> `ai-design-test-cases` -> `ai-generate-page-object` / `ai-implement-automation` -> the 3 review skills.

### Standalone Utilities

| Skill | Purpose |
|---|---|
| [`qc-onboarding-guide`](./qc-onboarding-guide) | Generate/update a QC onboarding guide (`onboarding.md`) for manual + automation testers. |
| [`qc-setup-guide`](./qc-setup-guide) | Generate/update a QC environment setup guide (`setup.md`), Playwright-specific. |
| [`review-playwright-auto`](./review-playwright-auto) | Standalone Playwright code review (tests, Page Objects, components) — no project setup needed. |

## Notes

- Each skill folder contains both its `SKILL.md` source and the pre-built `<skill-name>.skill` package — install the `.skill` file directly, or read/modify the source and re-zip it yourself.
- The 7 QA-chain skills prefer a live project `knowledge-base/` at `../../knowledge-base/` if one exists (see the "Path convention" note at the top of each `SKILL.md`), falling back to their own bundled copy otherwise.
