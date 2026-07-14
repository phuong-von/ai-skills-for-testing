# POM Standard

> Purpose: Define Page Object Model implementation guidelines.

## 1. Page Class Structure

```ts
export class LoginPage {
  private readonly page: Page;

  // Locators declared as readonly properties, initialized in constructor
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly submitButton: Locator;
  readonly errorMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.getByTestId('login-email');
    this.passwordInput = page.getByTestId('login-password');
    this.submitButton = page.getByRole('button', { name: 'Log in' });
    this.errorMessage = page.getByTestId('login-error');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }
}
```

- One Page Object per distinct page or major UI section (not one per test).
- Locators are declared once, as class properties initialized in the constructor — never re-queried inline inside every method.
- Page Objects hold no assertions about business outcomes (no `expect()` calls for "did the login succeed") — that belongs in the test. Page Objects only encapsulate interaction and, at most, simple state getters (e.g. `isErrorVisible()`).
- Reusable UI fragments that appear across multiple pages (navbar, modal, cookie banner) become **Components**, not duplicated inside every Page Object.

## 2. Method Naming Conventions

- Methods are named for **user intent**, not implementation: `login()` not `fillFormAndClickSubmit()`.
- Action methods are verbs: `login()`, `submitOrder()`, `openSettings()`.
- Getter/state methods start with `get`/`is`/`has`: `getErrorText()`, `isSubmitEnabled()`, `hasValidationError()`.
- Navigation methods are named `goto()` or `open<Page>()` and are the only place `page.goto()`/direct URL navigation happens for that page.
- Methods that return another Page Object (e.g. after a navigating action) are typed to return that Page Object, enabling fluent chaining: `async submitAndGoToDashboard(): Promise<DashboardPage>`.

## 3. Element Organization

- Group locators logically within the class (e.g. all form fields together, then action buttons, then messages) — order should be scannable, roughly matching the page's visual layout.
- Elements scoped to a repeated section (e.g. a table row) are exposed via a method that returns a scoped locator or a row-specific helper object, not as a single flat locator.
- Do not expose raw Playwright `Locator` objects for elements the test shouldn't interact with directly — keep the Page Object's public surface limited to what tests actually need.
- Constants specific to a page (e.g. max character limits shown in that page's UI) live alongside that Page Object, not scattered in the test file.

## 4. Inheritance Patterns

- A `BasePage` class holds truly shared behavior across all pages (e.g. `waitForPageLoad()`, common header/footer components, shared `page` reference) — all Page Objects extend it.
- Prefer **composition over inheritance** for shared UI sections: a page that includes a `NavbarComponent` holds it as a property (`this.navbar = new NavbarComponent(page)`), rather than inheriting navbar methods.
- Avoid deep inheritance chains (Page → Page → Page) — one level of `BasePage` inheritance is sufficient for nearly all cases; anything deeper usually signals the shared logic should be a component or utility instead.
- Page Objects for variants of the same page (e.g. `CheckoutPage` for guest vs. logged-in) should share a common base only if the majority of behavior is truly identical — otherwise keep them separate to avoid conditional logic inside the Page Object.

---
*Referenced by: [Automation Review Checklist](../checklists/automation-review-checklist.md).*
