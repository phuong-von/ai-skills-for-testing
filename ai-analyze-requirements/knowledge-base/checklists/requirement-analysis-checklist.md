# Requirement Analysis Checklist

> Purpose: Systematically analyze requirements for testability.

Use this checklist when analyzing a user story or requirement before test design begins — it complements the [Requirements Standard](../standards/requirements-standard.md) and [Requirement Quality Rubric](../standards/requirement-quality-rubric.md) with a step-by-step pass over the document.

## 1. Completeness Check

- [ ] The requirement has a title, ID, and clear user story statement (`As a... I want... so that...`).
- [ ] Actors/roles and their permissions are stated, not assumed.
- [ ] Preconditions are stated for each flow.
- [ ] An acceptance criterion exists for every distinct behavior described in the story — not just the primary flow.
- [ ] Negative and error flows are covered, not only the happy path.
- [ ] Boundary/validation rules are stated for every input field (required, format, length, range).
- [ ] Postconditions are stated (what state exists after each flow completes).
- [ ] Non-functional expectations are addressed when relevant (performance, security, accessibility) — or explicitly marked out of scope.
- [ ] Out-of-scope items are explicitly listed, not left to inference.

## 2. Ambiguity Detection

- [ ] No vague qualifiers without a measurable threshold ("fast", "easy", "appropriate") — see [Clarification Questions: Common Ambiguity Patterns](./clarification-questions.md).
- [ ] No undefined terms or acronyms used without explanation.
- [ ] No conflicting statements between two acceptance criteria.
- [ ] No unstated assumptions about actor/role for a given flow.
- [ ] No implicit business rule stated only through an example — see [Business Rule Checklist](./business-rule-checklist.md).

## 3. Testability Assessment

For each acceptance criterion, confirm:

- [ ] It describes a single, specific, observable outcome — not a general intention.
- [ ] The outcome is verifiable (visible state, status code, message, data change) without needing to guess at "correct" behavior.
- [ ] It does not depend on a system or process outside the team's control to verify (or if it does, that dependency is noted).
- [ ] It is independently testable — doesn't require another untestable AC to pass first in order to be checked.
- [ ] It aligns with INVEST — particularly Independent, Small, and Testable (see [INVEST Criteria](../standards/invest-criteria.md)).

## 4. Missing AC Identification

Actively check for acceptance criteria that *should* exist but don't, based on the feature type:

- [ ] Required-field / empty-input validation.
- [ ] Duplicate or conflicting data handling.
- [ ] Permission/authorization boundaries (what happens if the wrong role attempts this action).
- [ ] Rate limiting, lockout, or throttling for security-sensitive or abuse-prone actions.
- [ ] Concurrent access / race conditions, if multiple users can affect the same data.
- [ ] Integration failure handling (API timeout, third-party service down), if the feature depends on an external system.
- [ ] State transition rules — what's allowed/disallowed from each state.

Every missing AC identified here should become a clarification question or a documented finding — see [Clarification Questions](./clarification-questions.md).

## Outcome

- [ ] **Ready for test design** — all sections above pass, or gaps are documented with resolved clarifications.
- [ ] **Not ready** — list specific gaps found, referencing the section numbers above, and route as clarification questions before test design begins.
