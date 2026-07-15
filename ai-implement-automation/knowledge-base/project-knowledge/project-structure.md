# Project Structure

> Purpose: Define the project folder structure and organization so skills, docs, and knowledge stay consistent and discoverable.

## 1. Folder Hierarchy

```
<project-root>/
  skills/
    <skill-name>/
      SKILL.md
      templates/            (optional — only for self-contained/portable skills)
  knowledge-base/                (Shared across all skills)
    standards/                (requirement-quality-standard.md, testing-standard.md, test-design-techniques.md, playwright-standard.md, locator-standard.md, pom-standard.md, automation-coding-standard.md, ...)
    project-knowledge/        (project-structure.md, test-strategy.md, ...)
    templates/                (*.template.md used by skills)
    checklists/               (*-checklist.md used for self-validation)
    examples/                 (worked examples referenced by skills)
  requirements/
    <feature-area>/           (e.g. authentication/, checkout/)
  analysis-reports/           (output of ai-analyze-requirements)
  test-cases/                 (output of ai-design-test-cases)
  pages/
    <feature-area>/           (Page Object classes, output of ai-generate-page-object)
  tests/
    <feature-area>/           (Playwright automation scripts, output of ai-implement-automation)
  test-case-reviews/          (output of ai-review-test-case)
  automation-reviews/         (output of ai-review-automation)
  page-object-reviews/        (output of ai-review-page-object)
  docs/                       (human-facing project docs: onboarding.md, setup.md, ...)
```

**Why this depth:** Skills live at `skills/<skill-name>/SKILL.md` — exactly 2 folders below the project root (`skills` → `<skill-name>`). This is what makes the shared-resource links inside each `SKILL.md` (`../../knowledge-base/...`) resolve correctly. Don't move skill folders to a different depth without updating those links.

## 2. File Naming Conventions

| Type | Convention | Example |
|---|---|---|
| Skill definition | Always `SKILL.md`, inside its own named folder | `skills/ai-analyze-requirements/SKILL.md` |
| Requirement input | `<UC-ID>-<Use-Case-Name>.md` | `US-001-User-Login.md` |
| Requirement analysis output | `RA-<UC-ID>-<Use-Case-Name>.md` | `RA-US001-User-Login.md` |
| Test case output | `TC-<UC-ID>-<Use-Case-Name>.md` | `TC-US001-User-Login.md` |
| Page Object output | `<Name>Page.ts` | `LoginPage.ts` |
| Automation script output | `<name>.spec.ts` | `login.spec.ts` |
| Test case review output | `TCR-<UC-ID>-<Use-Case-Name>.md` | `TCR-US001-User-Login.md` |
| Automation review output | `AR-<script-name>.md` | `AR-login-spec.md` |
| Page Object review output | `POR-<Name>Page.md` | `POR-LoginPage.md` |
| Template | `<name>.template.md` | `requirement-analysis.template.md` |
| Checklist | `<name>-checklist.md` | `requirement-analysis-checklist.md` |
| Standard/rubric | `<name>-standard.md` / `<name>-rubric.md` / `<name>-criteria.md` | `requirement-quality-standard.md` |
| Project-level doc | Plain descriptive name | `onboarding.md`, `setup.md` |

Rules: use lowercase kebab-case for all filenames; prefixes (`RA-`, `TC-`) are uppercase; replace spaces with hyphens; preserve the original word order from the source Use Case ID/Name.

## 3. Module Organization

- **Skills** (`skills/`) are self-contained instruction sets. Two patterns exist:
  - **Project-bound skills** (e.g. `ai-analyze-requirements`, `ai-design-test-cases`, `ai-generate-page-object`, `ai-implement-automation`, `ai-review-test-case`, `ai-review-automation`, `ai-review-page-object`) reference the shared `knowledge-base/` at the project root via `../../` — they assume they're installed inside this project's structure.
  - **Portable skills** (e.g. `qc-onboarding-guide`, `qc-setup-guide`, `review-playwright-auto`) bundle their own reference files inside the skill package and have no external dependency, so they work standalone in any project.
- **`knowledge-base/`** holds shared, reusable reference material — standards, rubrics, checklists, templates, and examples — consumed by multiple skills. It is not itself executable.
- **`docs/`** holds human-facing reference documentation (onboarding, environment setup) that skills gen