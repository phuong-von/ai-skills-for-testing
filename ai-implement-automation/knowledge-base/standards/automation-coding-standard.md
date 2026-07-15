# Automation Coding Standard

> Purpose: Define coding conventions for automation scripts.

## 1. Naming Conventions

| Item | Convention | Example |
|---|---|---|
| Test file | `<feature>.spec.ts`, kebab-case | `user-login.spec.ts` |
| Test title | Describes behavior, starts with a verb or "should" | `"logs in with valid credentials"` |
| Page Object file | `<PageName>Page.ts`, PascalCase class name | `LoginPage.ts` |
| Component file | `<ComponentName>Component.ts` | `NavbarComponent.ts` |
| Variable / function | camelCase | `loginButton`, `submitForm()` |
| Constant | UPPER_SNAKE_CASE for true constants | `DEFAULT_TIMEOUT` |
| Test data factory | `create<Entity>()` | `createTestUser()` |

Test titles should read naturally when concatenated with `describe`, e.g. `describe('Login') > it('logs in with valid credentials')` reads as "Login logs in with valid credentials".

## 2. Code Organization

```
tests/
  <feature>/
    <feature>.spec.ts          # test files, grouped by feature
pages/
  <PageName>Page.ts            # one Page Object per page
components/
  <ComponentName>Component.ts  # shared reusable UI components
fixtures/
  <name>.fixture.ts            # custom Playwright fixtures
utils/
  <name>.util.ts                # pure helper functions, no Playwright API calls
data/
  <name>.data.ts                # test data factories/constants
```

- One test file per feature/flow — do not mix unrelated features in a single spec file.
- Imports ordered: external packages → Playwright fixtures → Page Objects/components → utils/data.
- No relative import chains deeper than 2 levels (`../../../`) — restructure or use path aliases instead.
- Keep test files focused on orchestration (calling Page Object methods); keep element interaction logic inside Page Objects, not inline in the test.

## 3. Error Handling Patterns

- Prefer Playwright's built-in auto-waiting and web-first assertions over manual try/catch around actions.
- When an action can legitimately fail as part of a negative test (e.g. expecting an error message), assert the failure explicitly — don't wrap it in a try/catch that swallows the result.
- For flows involving external dependencies (network, third-party services), use Playwright's route interception/mocking rather than depending on the real service being available and stable.
- Never use an empty catch block. If an exception is expected and safe to ignore, log why in a comment.
- Fail fast: don't chain multiple unrelated assertions in a way that hides which one actually failed — prefer one clear assertion per logical check.

## 4. Documentation Requirements

- Every test file starts with a short comment block: which feature/requirement it covers, and any non-obvious setup assumptions.
- Every custom fixture and utility function has a one-line comment explaining its purpose (JSDoc style encouraged for shared utils).
- Complex Page Object methods (multi-step flows) include a comment describing the user-facing action they represent.
- PRs introducing new Page Objects or fixtures briefly explain why in the PR description, not just in code comments.

---
*Referenced by: [Automation Review Checklist](../checklists/automation-review-checklist.md).*
