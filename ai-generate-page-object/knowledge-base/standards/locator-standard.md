# Locator Standard

> Purpose: Establish rules for element locator strategies.

## 1. Locator Priority

Use locators in this priority order — always prefer the highest option available on the element:

1. **`data-testid` attribute** — `page.getByTestId('login-button')`. Stable, explicit, immune to styling/copy changes. Preferred for all interactive elements the automation touches.
2. **Semantic role / accessible name** — `page.getByRole('button', { name: 'Log in' })`. Preferred when `data-testid` isn't available — also doubles as an accessibility check.
3. **Label / placeholder / text** — `page.getByLabel(...)`, `page.getByPlaceholder(...)`, `page.getByText(...)`. Acceptable for form fields and static content, but breaks if copy changes.
4. **`id` attribute** — only if stable and not auto-generated (e.g. not a framework-generated id that changes per build).
5. **CSS selector** — last resort, only for elements with no better option (e.g. third-party widgets you don't control). Keep selectors shallow — avoid deep descendant chains.
6. **XPath** — disallowed except for genuinely unavoidable cases (e.g. text-relative traversal with no other option); requires a comment explaining why.

## 2. Naming Patterns

- `data-testid` values use kebab-case and describe the element's purpose, not its type: `data-testid="submit-order"` not `data-testid="button-1"`.
- Locator variables in Page Objects are named for what the element is, suffixed by type when it disambiguates: `emailInput`, `submitButton`, `errorMessage`.
- Avoid generic names like `btn1`, `el`, `item` — every locator variable should be understandable without opening the DOM.

## 3. Dynamic Locator Handling

- For lists/repeated elements, use `data-testid` combined with `.filter()` or `.nth()` rather than brittle positional CSS (`:nth-child(3)`).
- For elements whose `data-testid` includes a dynamic ID (e.g. `data-testid="row-{id}"`), use a locator function/method on the Page Object that takes the ID as a parameter, not a hardcoded string.
- Prefer `page.getByRole(...).filter({ hasText: ... })` over chaining multiple brittle CSS combinators when narrowing down dynamic content.
- When an element only appears conditionally (e.g. after an action), assert its state with `expect(locator).toBeVisible()` rather than assuming it exists immediately.

## 4. Anti-Patterns to Avoid

- Do not use deep CSS descendant chains (`div > div > ul > li:nth-child(2) > span`) — one DOM refactor breaks the entire suite.
- Do not use XPath for simple lookups that `getByRole`/`getByTestId` can handle.
- Do not hardcode text in locators that's likely to change (marketing copy, dynamic labels) unless it's the actual subject of the test.
- Do not scrape locators from browser DevTools without reviewing whether `data-testid` exists first — auto-generated selectors are usually the most brittle option.
- Do not reuse a single generic locator variable for multiple different elements across a test — each element should have its own clearly named locator.

---
*Referenced by: [Automation Review Checklist](../checklists/automation-review-checklist.md).*
