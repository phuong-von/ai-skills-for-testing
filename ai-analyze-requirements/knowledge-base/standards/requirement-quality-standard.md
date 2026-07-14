# Requirement Quality Standard

> Purpose: Criteria for evaluating requirement quality.

## 1. SMART Criteria (applied to requirements)

Each acceptance criterion should be:

| Criterion | Meaning for a requirement/AC | Bad example | Good example |
|---|---|---|---|
| **Specific** | Describes one concrete behavior, not a general intention. | "The system should be secure." | "Passwords must be at least 8 characters, including one number." |
| **Measurable** | Has a threshold or observable outcome that can be checked. | "The page should load fast." | "The page loads in under 2 seconds on a standard connection." |
| **Achievable** | Technically feasible within the project's known constraints. | "Support offline editing" (with no offline architecture in place) | Scoped to what the current architecture supports, or flagged as a separate initiative. |
| **Relevant** | Ties back to the actual user story's goal, not an unrelated tangent. | An unrelated admin setting bundled into a user-facing login story. | Kept in its own story, or explicitly justified as in-scope. |
| **Time-bound** (where applicable) | States when a state/timeout applies, if time matters to the behavior. | "The session expires eventually." | "The session expires after 30 minutes of inactivity." |

## 2. Testability Requirements

An acceptance criterion is testable only if:

- [ ] It has an observable, verifiable outcome (visible state, status code, message, data change) — not just a stated intention.
- [ ] It doesn't require the tester to reach a subjective judgment ("looks good", "feels right") without a defined standard to judge against.
- [ ] It is independently checkable — doesn't require an untestable AC to pass first.
- [ ] Any external dependency needed to verify it (API, third-party service) is identified, so tests can mock or account for it.
- [ ] It doesn't bundle multiple outcomes into one criterion — split "and" statements into separate ACs when they describe genuinely distinct, independently verifiable behaviors.

## 3. Acceptance Criteria Format

Use **Given / When / Then** as the default format for acceptance criteria:

```
Given <precondition/context>
When <action/event>
Then <expected, observable outcome>
```

**Example:**
```
Given a registered user is on the login page
When they enter a valid email and password and submit
Then they are redirected to the dashboard within 2 seconds
```

Rules:
- One `When` per AC — if there are multiple distinct triggers, write separate ACs.
- `Then` must state an outcome, not a restatement of the action ("Then the form is submitted" is not an outcome).
- `Given` should state only what's necessary for this specific AC, not the entire feature's context repeated verbatim each time.
- Non-flow-based requirements (e.g. a data validation rule) may instead use a plain rule statement, but must still meet the Testability Requirements above.

## 4. Definition of Ready (DoR) Checklist

A requirement is ready for test design when:

- [ ] It has a title, ID, and user story statement.
- [ ] All acceptance criteria follow the Given/When/Then format (or documented rule format) and meet the Testability Requirements above.
- [ ] Actors/roles, preconditions, and postconditions are stated.
- [ ] Negative and boundary scenarios are covered, not just the happy path.
- [ ] Business rules are stated explicitly, not only implied by examples (see [Business Rule Checklist](../checklists/business-rule-checklist.md)).
- [ ] Out-of-scope items are explicitly listed.
- [ ] It has passed [Requirement Analysis](../checklists/requirement-analysis-checklist.md) with no unresolved blocking clarifications.
- [ ] It has a quality rating per the [Requirement Quality Rubric](./requirement-quality-rubric.md) of 3 or higher.

A requirement failing any DoR item should not proceed to test design — route it back through [Clarification Questions](../checklists/clarification-questions.md) first.
