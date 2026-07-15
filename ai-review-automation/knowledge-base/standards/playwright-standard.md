# Playwright Standard

> Purpose: Playwright-specific coding guidelines.

## 1. Playwright Best Practices

- Use `test.describe` to group related tests by feature/flow; use `test.step()` inside long tests to make failure output readable.
- Prefer Playwright's built-in fixtures (`page`, `context`, `browser`) and custom fixtures over manual setup/teardown in every test.
- Use `expect(locator)` web-first assertions (auto-retrying) instead of `expect(await locator.textContent())` (single snapshot, not retried).
- One user-facing behavior per test — avoid testing five unrelated things in a single `test()` block.
- Use `test.beforeEach` for setup shared across tests in a `describe` block; avoid global mutable state across files.
- Keep tests deterministic — no reliance on real current date/time, external live data, or random values without a fixed seed.

## 2. Wait Strategies

- Rely on Playwright's auto-waiting (actions like `click()`, `fill()` already wait for the element to be actionable) — don't add a wait before every action.
- Use explicit condition waits when needed: `await expect(locator).toBeVisible()`, `await page.waitForURL(...)`, `await page.waitForResponse(...)`.
- `page.waitForTimeout()` (arbitrary sleep) is disallowed except as a documented last resort with a comment explaining why no better wait condition exists.
- For network-dependent UI, wait on the actual response/state change (`waitForResponse`, `waitForLoadState('networkidle')` used sparingly) rather than guessing a delay.

## 3. Screenshot / Video Handling

- Screenshots and video are captured automatically on failure only (`screenshot: 'only-on-failure'`, `video: 'retain-on-failure'` in config) — not on every run, to keep CI artifacts manageable.
- Use `await page.screenshot()` manually only for debugging during development; remove before merging unless it's an intentional visual-regression test.
- Visual regression tests (if used) store baseline images in a dedicated `__screenshots__/` folder per test file, and are reviewed explicitly on baseline changes.
- Trace files (`trace: 'retain-on-failure'`) are enabled in CI so failures can be debugged with `npx playwright show-trace`.

## 4. Parallel Execution Config

- Tests must be safe to run in parallel — no shared mutable state, no dependency on execution order, no reused test accounts without isolation.
- Use `fullyParallel: true` at the project level; only disable parallelism for a specific file/describe block when there's a documented reason (e.g. a genuinely shared resource).
- Each worker should use isolated test data (e.g. a uniquely generated user per test) rather than a single shared fixture account, to avoid cross-test collisions.
- CI worker count is tuned separately from local worker count (`workers` in config can be `undefined` locally, fixed in CI based on available cores).
- Sharding (`--shard=1/4` etc.) is used for large suites in CI to parallelize across machines, not just across local cores.

---
*Referenced by: [Automation Review Checklist](../checklists/automation-review-checklist.md).*
