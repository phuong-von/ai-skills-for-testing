# Test Strategy

> Purpose: Document the overall testing approach and scope for [Project Name].

## 1. Test Levels

### Functional Levels

| Level | Scope | Owner | Trigger |
|---|---|---|---|
| Unit | Individual functions/components in isolation | Developers | Every commit / PR (CI) |
| Integration | Interaction between modules, services, or the API + DB layer | Developers + QA | Every PR (CI) |
| API / Service | Contract, status codes, payloads, and error handling of individual API endpoints, independent of the UI | QA (automation) | Every PR (CI) |
| System | The application as a whole, across integrated modules, in an environment close to production | QA | Before each release / nightly |
| Smoke | A small, fast subset of critical-path checks confirming the build isn't fundamentally broken | QA (automation) | Every deploy, before deeper testing runs |
| Sanity | Narrow, quick check that a specific bug fix or small change works as expected | QA (manual or automation) | After a targeted fix, before full regression |
| Regression | Re-running existing test coverage to confirm previously working functionality still works | QA (automation) | Every PR to main / nightly / pre-release |
| End-to-End (E2E) | Full user flows through the UI, as a real user would experience them | QA (automation) | Every PR to main / nightly / pre-release |
| User Acceptance (UAT) | Validating the delivered feature meets business/user needs, typically with the Product Owner or real users | QA + Product Owner | Before release sign-off |
| Manual / Exploratory | Flows not (yet) automated, new features, usability, edge cases | QA (manual) | Every feature before release; ad hoc exploratory sessions |

### Non-Functional Levels

| Level | Scope | Owner | Trigger |
|---|---|---|---|
| Performance (load/stress/spike) | System behavior under expected, peak, and beyond-peak load | QA / Performance Engineer | Before major releases; on capacity-sensitive changes |
| Security | Authentication, authorization, input validation, common vulnerability classes (e.g. injection, XSS) | QA / Security | Before release; whenever auth or data-handling code changes |
| Accessibility | Compliance with accessibility standards (e.g. WCAG), keyboard nav, screen reader support | QA | Every new UI feature; periodic full audits |
| Cross-Browser / Cross-Platform (Compatibility) | Behavior across supported browsers (Chrome, Firefox, Safari, Edge), operating systems (Windows, macOS, iOS, Android), devices, and screen sizes/resolutions | QA (manual + automation) | Before release; on UI-heavy changes; Playwright `projects` config covers browser matrix in CI |
| Usability | Ease of use, clarity of flows, user friction points | QA / UX | Major feature releases |

[Adjust which levels are in scope for this project — not every project needs all of the above.]

## 2. Test Coverage Goals

- Unit test coverage target: [e.g. 80% line coverage on core business logic]
- Integration coverage: [e.g. all API endpoints have at least one happy-path + one error-path test]
- E2E automation coverage: [e.g. all P0/P1 user flows automated; P2+ covered manually]
- Every user story must pass through [Requirement Analysis](../standards/requirements-standard.md) before test design, and reach a [Requirement Quality Rubric](../standards/requirement-quality-rubric.md) score of 3 or higher before test cases are finalized.
- Coverage is tracked in: [test management tool, e.g. TestRail 