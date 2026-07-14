# Example: Automation Script — User Login

> Purpose: Provide reference automation script implementation.

This is a worked example of a Playwright automation script implementing [`TC-US001-01: User logs in successfully`](./test-case-login-example.md), following the standards in [Playwright automation review](../../Playwright%20automation%20review/README.md) (coding, Playwright, locator, and POM standards).

## Playwright Test Structure

```ts
// tests/authentication/login.spec.ts
import { test, expect } from '@playwright/test';
import { LoginPage } from '../../pages/LoginPage';
import { DashboardPage } from '../../pages/DashboardPage';
import { createTestUser } from '../../data/user.data';

test.describe('Login', () => {
  test('logs in with valid credentials — TC-US001-01 @smoke @regression', async ({ page }) => {
    const user = await createTestUser();
    const loginPage = new LoginPage(page);
    const dashboardPage = new DashboardPage(page);

    await loginPage.goto();
    await loginPage.login(user.email, user.password);

    await expect(page).toHaveURL('/dashboard');
    await expect(dashboardPage.userNameHeader).toHaveText(user.name);
  });

  test('shows an error with invalid credentials — TC-US001-02 @regression', async ({ page }) => {
    const loginPage = new LoginPage(page);

    await loginPage.goto();
    await loginPage.login('wrong@example.com', 'wrong-password');

    await expect(loginPage.errorMessage).toBeVisible();
    await expect(loginPage.errorMessage).toHaveText('Invalid email or password.');
  });
});
```

## POM Integration

```ts
// pages/LoginPage.ts
import { Page, Locator } from '@playwright/test';

export class LoginPage {
  private readonly page: Page;
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

The test file only calls `loginPage.login(...)` — it never queries a locator directly, per the [POM Standard](../../Playwright%20automation%20review/pom-standard.md).

## Assertions Best Practices

- `await expect(page).toHaveURL('/dashboard')` — web-first assertion, auto-retries until the URL matches or times out, instead of manually reading `page.url()` once.
- `await expect(dashboardPage.userNameHeader).toHaveText(user.name)` — asserts on the actual rendered value tied to the test's own data, not a hardcoded name that could go stale.
- `await expect(loginPage.errorMessage).toBeVisible()` before checking its text — confirms the element exists before asserting content, giving a clearer failure if the error never appeared at all.
- No `waitForTimeout()` anywhere — waiting is handled by Playwright's auto-waiting and the assertions themselves, per the [Playwright Standard](../../Playwright%20automation%20review/playwright-standard.md).

## Test Data Handling

```ts
// data/user.data.ts
import { faker } from '@faker-js/faker';

export async function createTestUser() {
  const email = faker.internet.email();
  const password = 'Test-' + faker.string.alphanumeric(10);
  const name = faker.person.fullName();

  // In a real project: call a test API/seed script to actually create this user.
  await api.createUser({ email, password, name });

  return { email, password, name };
}
```

- Each test generates its own isolated user rather than relying on one shared fixture account — safe for parallel execution, per the [Playwright Standard](../../Playwright%20automation%20review/playwright-standard.md)'s parallel execution guidance.
- The invalid-credentials test intentionally uses a fixed, clearly-fake email/password since it should never succeed — no need to generate dynamic data for a negative case that doesn't depend on a real account.
- No password value is logged or committed in plaintext anywhere in the repo — generated at runtime only.
