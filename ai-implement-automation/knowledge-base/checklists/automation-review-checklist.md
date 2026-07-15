# Automation Review Checklist

> Purpose: Verify automation code quality before merging.

Use this checklist to review a Playwright automation script (test, Page Object, or reusable component) before it is approved for merge.

## 1. Coding Standards Compliance

- [ ] Code follows [Automation Coding Standard](../standards/automation-coding-standard.md) (naming, file organization, imports).
- [ ] Playwright-specific conventions from [Playwright Standard](../standards/playwright-standard.md) are followed (wait strategies, fixtures, parallel-safe config).
- [ ] Test file and test names follow the project's naming convention and clearly describe the scenario under test.
- [ ] No hardcoded credentials, environment URLs, or secrets — values come from `.env`/config, per [Environment Setup Guide](../../docs/setup.md).
- [ ] No commented-out or dead code left in the file.

## 2. Locator & Page Object Compliance

- [ ] Locators follow the priority order and naming patterns in [Locator Standard](../standards/locator-standard.md) (e.g. `data-testid` > `id` > semantic role > CSS, avoiding brittle XPath).
- [ ] Page classes and components follow [POM Standard](../standards/pom-standard.md) — one class per page/component, methods named for user intent (not implementation).
- [ ] Existing Page Objects/components are reused where applicable — no duplicate Page Object created for a page that already has one.
- [ ] New Page Objects/components are added only when genuinely missing, not as a copy-paste of an existing one with minor edits.

## 3. Error Handling Check

- [ ] No arbitrary hardcoded waits (`waitForTimeout` used only as a last resort, with a comment explaining why); prefer Playwright's auto-waiting and explicit condition waits.
- [ ] Assertions use Playwright's web-first assertions (`expect(locator).toBeVisible()`, etc.) rather than manual polling.
- [ ] Failure output is informative — a failed assertion should make it obvious what went wrong without needing to re-run in debug mode.
- [ ] Flows that can legitimately fail (e.g. network calls, conditional UI) have explicit handling, not silent catch-and-ignore blocks that could mask real failures.
- [ ] Test does not depend on execution order or leftover state from another test.

## 4. Maintainability Review

- [ ] No duplicated logic that should be extracted into a shared Page Object method, fixture, or utility.
- [ ] Test data is either generated dynamically or clearly isolated per test — not shared mutable state across tests.
- [ ] Setup/teardown (`beforeEach`/`afterEach` or fixtures) cleans up any state the test creates (e.g. created records, uploaded files).
- [ ] Test is independent — can run alone or in any order without failing.
- [ ] Comments are used only where intent isn't obvious from the code itself (not restating what the code does).

## 5. Traceability

- [ ] Automation script references its source test case ID (e.g. `TC-US001-User-Login`) in a comment, test title, or tag.
- [ ] The script is tagged appropriately (e.g. `@smoke`, `@regression`) so it runs in the correct CI suite, per [Test Strategy](../project-knowledge/test-strategy.md).
- [ ] If the script deviates from the approved test case (e.g. combines steps, skips a scenario), the deviation is called out in the PR description.

## Outcome

- [ ] **Approved** — all sections above pass.
- [ ] **Needs revision** — list specific issues found, referencing the item numbers above.

Do not approve automation code with unresolved items in any section.
