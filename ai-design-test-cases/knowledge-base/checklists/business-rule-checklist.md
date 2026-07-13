# Business Rule Checklist

> Purpose: Ensure all business rules are captured and tested.

Use this checklist when a requirement contains calculations, thresholds, conditional logic, or policy — anywhere the system must enforce a rule rather than just display data.

## 1. Rule Identification Criteria

A statement is a business rule (not just descriptive text) if it meets any of these:

- [ ] It constrains what values, states, or transitions are allowed (e.g. "an order cannot be submitted with zero items").
- [ ] It defines a calculation, threshold, or limit (e.g. "free shipping applies above $50").
- [ ] It changes behavior based on a condition (e.g. role, plan tier, region, time).
- [ ] It defines precedence between two other rules when both could apply.
- [ ] Violating it should produce an observable system response (error, block, warning) — if nothing observable happens when it's violated, it isn't a testable rule yet.

For each identified rule, confirm it is stated **explicitly** in the requirement — not only implied by an example. If only implied, raise a clarification question (see [Clarification Questions](./clarification-questions.md)) to confirm the rule as a general statement, not just the one example given.

## 2. Validation Scenarios

For each business rule, confirm test coverage exists for:

- [ ] The rule being satisfied (valid case).
- [ ] The rule being violated at exactly the boundary (e.g. exactly $50, exactly the limit).
- [ ] The rule being violated clearly past the boundary (e.g. well below/above the limit).
- [ ] The system's response to a violation is specific and verifiable (error message, blocked action, fallback behavior) — not just "handled gracefully".
- [ ] If the rule has exceptions (by role, plan, flag), each exception path is covered, not just the default case.

## 3. Edge Case Considerations

- [ ] What happens when required inputs to the rule are missing, null, or zero?
- [ ] What happens when the rule's inputs come from an unexpected source (e.g. bulk import, API, admin override) rather than the normal UI flow?
- [ ] Does the rule behave consistently under concurrent/simultaneous actions (e.g. two users triggering a limit at the same time)?
- [ ] Does the rule need to be re-evaluated if underlying data changes after the fact (e.g. a discount rule when an item is later removed from the order)?
- [ ] Are there currency, timezone, locale, or rounding considerations that affect whether the rule evaluates correctly?

## 4. Dependency Mapping

- [ ] List other rules or features this rule depends on (e.g. a shipping rule depends on the cart total rule being correct first).
- [ ] List other rules or features that depend on this rule (what breaks downstream if this rule changes?).
- [ ] Note any rule defined in configuration/admin settings rather than hardcoded — confirm whether tests should cover the default config, or also verify the rule respects a changed config value.
- [ ] If the rule interacts with an external system (payment provider, tax service), note what's mocked vs. tested against a real/sandbox dependency.

## 5. Quality Dimensions

For every rule, confirm it is:

- [ ] **Complete** — every rule implied by the requirement is explicitly documented, not left to inference.
- [ ] **Consistent** — doesn't contradict another rule, AC, or existing system behavior.
- [ ] **Accurate** — reflects actual business intent, confirmed with the source rather than assumed from a single example.
- [ ] **Testable** — has a specific, observable pass/fail outcome — not just descriptive text.
- [ ] **Traceable** — references its source requirement/AC ID.
- [ ] **Risk Assessed** — impact (financial, security, data integrity) is rated to set test depth and priority.
- [ ] **Data Validated** — inputs the rule depends on are validated (required, type, range) before the rule logic runs.
- [ ] **Workflow Valid** — enforced at the correct step(s), and re-checked if relevant state changes later in the flow.
- [ ] **Security Compliant** — can't be bypassed via API, admin override, or race condition without proper authorization.
- [ ] **AI Safe** — if an AI/automated system evaluates or enforces the rule, its decision is auditable, bounded, and has a fallback for cases it can't confidently resolve.

## Example: Guard Rule

> "A coupon cannot be applied if it would cause the order's profit margin to go negative."

Check the rule fires exactly at the threshold, when the value is already past it, and when the action itself is what causes it to cross — and confirm it's re-checked if state changes later (e.g. cart contents change after the coupon is applied).

Similar patterns: refund exceeding original payment, price override below cost, withdrawal exceeding balance, role change without a prerequisite condition, stacked discounts exceeding a cap.

## Outcome

- [ ] **Approved** — every rule has full coverage across sections 1-5.
- [ ] **Needs revision** — list which rules are missing coverage, referencing the section numbers above.
