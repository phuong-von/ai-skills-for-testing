# Clarification Questions Checklist

> Purpose: Guide testers to ask the right questions when requirements are unclear.

## 1. Common Ambiguity Patterns

Watch for these patterns when reading a requirement — each one is a signal that a clarification question is needed:

| Pattern | Example | Why It's a Problem |
|---|---|---|
| Vague qualifiers | "fast", "user-friendly", "appropriate", "secure" | No measurable threshold — can't be tested pass/fail. |
| Undefined terms or acronyms | "the system validates the KYC status" | Reader must guess what "KYC status" means or where it comes from. |
| Missing negative/edge cases | Only the happy path is described | Error handling, empty states, and limits are left to assumption. |
| Conflicting statements | AC-2 says "optional", AC-5 treats it as required | Behavior depends on which statement is authoritative. |
| Unstated assumptions | "the user" without specifying role/permissions | Behavior may differ by role but the requirement doesn't say. |
| Missing non-functional expectations | No mention of performance, security, or accessibility for a sensitive feature | Non-functional risk is silently unaddressed. |
| Ambiguous scope boundary | "similar to the existing X flow" | Unclear which parts are actually reused vs. new. |
| Implicit business rule | A calculation or limit is implied by an example but never stated as a rule | Can't verify the rule for inputs outside the example. |

## 2. Questions for Each Scenario Type

Use these templates as a starting point, then make each question specific to the actual gap found.

### Completeness
- "What should happen when [precondition/edge case] occurs — is there a defined behavior for it?"
- "Are there other roles/actors who interact with this feature, and do they have different permissions or flows?"
- "What is the expected state after [flow] completes (e.g. session, database, notification)?"

### Testability
- "[Vague term] is used in [AC] — what is the measurable threshold (e.g. time limit, character count, percentage)?"
- "How can [expected outcome] be observed/verified — is there a visible state, a status code, or a log entry?"
- "Does [AC] describe a distinct behavior, or does it overlap with [other AC]? If distinct, what's different?"

### Business Rules
- "Is [rule] always true, or are there exceptions (e.g. by role, by plan tier, by region)?"
- "What is the source of truth for [value] — is it configurable, or is the specific number/limit intentional?"
- "If [condition A] and [condition B] both apply, which takes precedence?"

### Non-Functional
- "Are there performance expectations for this feature (e.g. response time under normal/peak load)?"
- "Does this feature handle sensitive data — are there security or compliance requirements to capture?"
- "Should this feature be tested across specific browsers/devices, or does the standard project matrix apply?"

## 3. Stakeholder Communication Tips

- **Batch questions** — send all clarification questions for a requirement together, grouped by category, rather than trickling them one at a time.
- **Lead with the "why"** — briefly state what decision or test design depends on the answer, so the stakeholder understands the stakes, not just the ask.
- **Offer a default when reasonable** — "If not specified, I'll assume X — confirm or correct?" is often faster to answer than a fully open question.
- **Don't ask what's answerable from context** — check the requirement, linked designs, and related stories before asking; only escalate genuine gaps.
- **Route to the right person** — business rule questions go to the Product Owner/BA; technical feasibility questions go to the Tech Lead; don't default everything to one person.
- **Track resolution** — once answered, update the requirement or record the answer where the next reader (e.g. test case author) will actually see it, not just in a chat thread.

## 4. Question Quality Check

Before sending a clarification question, confirm it:

- [ ] Is phrased so a Product Owner/BA can answer directly (yes/no or a concrete value), not open-ended philosophy.
- [ ] References the specific AC, section, or term it relates to.
- [ ] States why it matters (what test design or implementation decision depends on the answer).
- [ ] Does not assume an answer or lead the respondent toward one option.
- [ ] Is grouped under the correct category: Completeness, Testability, Business Rules, or Non-Functional.
