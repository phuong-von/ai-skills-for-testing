# Test Case Review Checklist

> Purpose: Ensure test cases meet quality standards before approval.

Use this checklist to review a test case (or a full test suite for a feature) before it is marked approved/ready for execution or automation.

## 1. Coverage Verification

- [ ] Every acceptance criterion in the source requirement maps to at least one test case.
- [ ] Positive (happy path), negative, and boundary scenarios are all represented where applicable.
- [ ] Risk-based priority (P0/P1/P2) reflects the feature's actual impact, per [Test Strategy](../project-knowledge/test-strategy.md).
- [ ] No duplicate test cases validating the exact same behavior.
- [ ] Out-of-scope items noted in the requirement are correctly excluded, not silently tested or silently missing.

## 2. Test Level & Cross-Cutting Coverage

- [ ] The suite considers which functional levels apply (unit / integration / API / system / smoke / sanity / regression / E2E / UAT) per [Test Strategy](../project-knowledge/test-strategy.md) — not every level needs a test case here, but an omission should be a deliberate, noted call rather than an oversight.
- [ ] For features flagged High risk during Requirement Analysis, negative and boundary scenarios are present (per the Risk-Based Testing Approach in Test Strategy).
- [ ] **Cross-Browser / Cross-Platform** coverage is considered for any UI-facing feature — the review states which browsers/devices/OS are in scope, or explicitly notes "single browser only" and why.
- [ ] **Performance, Security, and Accessibility** implications are considered when the feature touches auth, payments, public-facing pages, or user data — flagged for dedicated non-functional testing if in scope, not silently skipped.
- [ ] Automation-eligible test cases are tagged appropriately (e.g. `@smoke`, `@regression`) so they can be picked up by the right CI run.

## 3. Step Clarity Check

- [ ] Each step describes a single, unambiguous action (no compound steps like "do X and then check Y and also verify Z").
- [ ] Preconditions are explicit and reproducible (test data, user state, environment).
- [ ] Steps use consistent, specific language (e.g. "click", "enter", "select") rather than vague verbs ("go to", "handle").
- [ ] No missing setup/teardown steps that would make the test non-repeatable.

## 4. Expected Results Validation

- [ ] Every step with an observable outcome has an explicit expected result — no