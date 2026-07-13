# Test Design Techniques

> Purpose: Document approved test design methodologies.

Use these techniques when designing test cases (see [Test Case Template](../templates/test-case.template.md)) to generate cases systematically instead of guessing. Select the technique(s) that fit the requirement — not every technique applies to every feature (see the Anti-Fixed-Quota principle in `ai-design-test-cases`).

## 1. Equivalence Partitioning

Divide inputs into groups ("partitions") where every value in a group should behave the same way. Test one representative value per partition instead of every possible value.

**Example:** A discount field accepting 0–100 (%).
- Valid partition: 1–100 → test with `50`.
- Invalid partition (below range): negative numbers → test with `-1`.
- Invalid partition (above range): over 100 → test with `101`.
- Invalid partition (wrong type): non-numeric → test with `"abc"`.

Use when: an input has a range or a set of categories, and testing every value would be redundant.

## 2. Boundary Value Analysis

Test at the edges of each partition, not just the middle — most defects cluster at boundaries.

**Example:** Same discount field (valid range 0–100):
- Just below the lower boundary: `-1`
- At the lower boundary: `0`
- Just above the lower boundary: `1`
- Just below the upper boundary: `99`
- At the upper boundary: `100`
- Just above the upper boundary: `101`

Use when: a rule involves a numeric limit, threshold, character count, or date range — pairs naturally with Equivalence Partitioning.

## 3. Decision Tables

Map out every combination of conditions and the expected action for each, when a feature's behavior depends on multiple independent factors.

**Example:** Shipping cost rule — free shipping if (order ≥ $50) AND (member = true); otherwise a flat fee, except international orders always pay a fee regardless of the above.

| Order ≥ $50 | Member | International | Shipping Cost |
|---|---|---|---|
| Yes | Yes | No | Free |
| Yes | No | No | Flat fee |
| No | Yes | No | Flat fee |
| No | No | No | Flat fee |
| Yes | Yes | Yes | International fee |

Use when: multiple conditions combine to determine an outcome — this technique makes missed combinations visible (see also [Business Rule Checklist](../checklists/business-rule-checklist.md)).

## 4. State Transition Testing

Model the feature as states and the transitions allowed (or disallowed) between them. Test valid transitions, invalid transitions, and behavior when an event occurs in an unexpected state.

**Example:** An order's status: `Draft → Submitted → Paid → Shipped → Delivered`, with `Cancelled` reachable from `Draft`, `Submitted`, or `Paid` (but not after `Shipped`).

Test:
- Valid transitions: `Draft → Submitted`, `Submitted → Paid`, etc.
- Invalid transitions: attempting `Draft → Shipped` directly should be rejected.
- Boundary transitions: attempting `Cancelled` from `Shipped` should be blocked.
- Re-entrant/no-op cases: attempting to submit an already-`Submitted` order.

Use when: a feature has a defined lifecycle or workflow with states and rules about what can happen from each state.

## Choosing a Technique

| Requirement shape | Likely technique |
|---|---|
| A single input with a range or category | Equivalence Partitioning + Boundary Value Analysis |
| A numeric limit or threshold | Boundary Value Analysis |
| Multiple independent conditions affecting one outcome | Decision Tables |
| A workflow/lifecycle with statuses | State Transition Testing |

Multiple techniques often apply to the same feature — e.g. a checkout flow may need Decision Tables for pricing rules and State Transition Testing for the order lifecycle.
