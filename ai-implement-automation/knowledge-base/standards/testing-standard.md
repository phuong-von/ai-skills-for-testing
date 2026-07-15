# Testing Standard

> Purpose: Overall testing standards and practices.

## 1. Test Naming Conventions

- Test case titles state the scenario and expected behavior: `"User logs in successfully with valid credentials"`, not `"Login test 1"`.
- Automated test titles follow the same pattern and include the source test case ID: `test('logs in with valid credentials — TC-US001-01', ...)`.
- `describe` blocks group by feature/flow (`Login`, `Checkout`), not by test type (`Positive tests`, `Negative tests`) — the scenario name itself should communicate positive/negative.
- File names match the feature under test: `login.spec.ts`, not `test1.spec.ts`.

## 2. Assertion Guidelines

- One logical assertion focus per test — a test can have multiple `expect()` calls if they verify the same outcome (e.g. checking both URL and page heading after navigation), but shouldn't verify multiple unrelated behaviors.
- Assertions must be specific and verifiable: assert exact text, exact status code, or exact visible state — not just "no error was thrown".
- Prefer built-in, auto-retrying assertions (e.g. Playwright's `expect(locator)...`) over manual polling or a single synchronous check.
- Negative/error test cases assert the specific failure condition (message, code, blocked state) — not just that *something* different happened.
- Every acceptance criterion maps to at least one assertion somewhere in the suite — untested criteria should be flagged, not silently skipped.

## 3. Test Independence Rules

- Every test must be able to run alone, and in any order, without failing.
- No test depends on state left behind by another test (shared mutable fixtures, reused records without isolation).
- Each test creates its own test data (or uses a uniquely scoped subset) rather than relying on a single shared account/record across the whole suite.
- Tests must be safe to run in parallel — no reliance on execution order or a single shared browser/session across tests.
- If a test's setup depends on another feature (e.g. checkout tests need a logged-in user), that setup is handled via fixtures/helpers, not by silently depending on another test having run first.

## 4. Cleanup Procedures

- Any state a test creates (records, uploaded files, sent messages) is cleaned up after the test, via `afterEach`/fixture teardown — not left behind to accumulate.
- Cleanup runs even if the test fails — use fixtures or `afterEach` (which runs regardless of test outcome), not cleanup code placed only at the end of the test body.
- Tests that intentionally leave state for inspection (e.g. debugging) must not be merged into the main suite without removing or gating that behavior.
- Shared/expensive setup (e.g. seeding a large dataset) is scoped appropriately (`beforeAll` only when truly safe to share across tests) — don't default to per-test setup if it makes the suite unreasonably slow, but don't share state that could cause cross-test interference either.
- CI environments are reset or use isolated data per run so failed cleanup in one run doesn't cascade into false failures in the next.

---
*See also: [Playwright Standard](./playwright-standard.md) for framework-specific implementation of these rules, and [Test Case Review Checklist](../checklists/test-case-review-checklist.md) for reviewing compliance.*
