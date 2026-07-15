# Test Script Template

> Purpose: Blank structure for a Playwright automation script. See [Automation Script — User Login Example](../examples/tests-script-login-example.md) for a filled-in reference, and [Playwright Standard](../standards/playwright-standard.md) / [Automation Coding Standard](../standards/automation-coding-standard.md) for the rules behind each part.

---

## File location

```
tests/<feature-area>/<feature>.spec.ts
```

## Top-of-file comment (required)

```ts
// Feature/requirement covered: <UC-ID> - <Use Case Name>
// Non-obvious setup assumptions: <e.g. requires a seeded test user, mocked payment gateway>
```

## Imports

```ts
import { test, expect } from '@playwright/test';
import { <PageName>Page } from '../../pages/<feature-area>/<PageName>Page';
import { create<Entity> } from '../../data/<feature-area>.data';
```

Import order: external packages → Playwright fixtures → Page Objects/components → utils/data.

## Test Structure

```ts
test.describe('<Feature/Flow>', () => {
  test('<scenario, states behavior> — TC-<UC ID>-<sequence> @<tag>', async ({ page }) => {
    // Arrange: generate isolated test data, instantiate Page Object(s)
    const <entity> = await create<Entity>();
    const <pageName>Page = new <PageName>Page(page);

    // Act: call Page Object methods only — no direct locator queries here
    await <pageName>Page.goto();
    await <pageName>Page.<actionMethod>(...);

    // Assert: web-first, auto-retrying assertions
    await expect(page).toHaveURL('<expected-url>');
    await expect(<pageName>Page.<elementLocator>).toHaveText('<expected-text>');
  });
});
```

## Notes

- One `test()` per test case scenario; group related scenarios in one `describe` block per feature/flow.
- Test title states the scenario and includes the source `TC-` ID, per [Testing Standard](../standards/testing-standard.md).
- Never query a locator directly in the test file — only call Page Object methods (see [POM Standard](../standards/pom-standard.md)). If the needed Page Object doesn't exist yet, use `ai-generate-page-object` first.
- No `page.waitForTimeout()` except as a documented last resort with a comment explaining why.
- Generate isolated test data per test (via a `data/` factory) — do not depend on a shared fixture account.
- Add cleanup (`afterEach`/fixture teardown) for any state the test creates.
- Before marking this script ready for review, self-check it against the [Automation Review Checklist](../checklists/automation-review-checklist.md).
