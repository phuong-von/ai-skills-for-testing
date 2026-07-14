---
name: ai-analyze-requirements
description: Analyze user stories and requirements for completeness and testability.
---

# Skill: AI Analyze Requirements

> **Path convention:** this packaged skill bundles its own reference copy of the knowledge base it needs (`knowledge-base/...`, sibling to this file) so it works standalone right after install — no project setup required. If you're instead running it from inside this project's own `skills/ai-analyze-requirements/SKILL.md` (2 folders below the project root, per [Project Structure](knowledge-base/project-knowledge/project-structure.md)), prefer the project's live copy at `../../knowledge-base/...` when it exists — it's likely more current or customized than this bundled snapshot. Output paths like `analysis-reports/` below are relative to whichever project you're running this skill in — create/use them at that project's root, not inside this skill's own installed folder.

---

# 1. Purpose

Analyze user stories and requirements to assess completeness, clarity, and testability before they are used for design, development, or test case generation.

---

# 2. When to Use

Use this skill when you need to:

- Review a User Story or requirement document before it is approved, estimated, or handed off for test case design.
- Check whether newly received requirements are complete and testable, before test design begins.

**Manual execution:**
```
/ai-analyze-requirements Functional requirements/authentication/US-001-login.md
```

---

# 3. Do Not Use When

This skill is NOT responsible for:

- Minor typo fixes or wording/grammar cleanup in a requirement
- Updating or maintaining existing test cases (use `ai-design-test-cases` instead)
- Generating test cases from a requirement (use `ai-design-test-cases` instead)
- Rewriting or authoring a requirement/user story from scratch
- Estimating effort, story points, or sprint planning
- Approving or rejecting a requirement on the team's behalf — this skill only reports findings; a human makes the final call

---

# 4. Inputs

## Required

- One requirement document. (e.g. user story, use case, functional specification, acceptance criteria, business rules, API specification)

  Example: `requirements/authentication/US-001-login.md`

## Optional

- **Analysis Scope** — `completeness`, `testability`, or `both`. If not provided, the AI will ask a clarification question before proceeding (see Decision Rules below).
- Additional context documents (business rules, related user stories, API specs) to disambiguate the requirement under review.

### Analysis Scope — Decision Rules

1. If user says:
   - "completeness" or "missing criteria" → analyze only completeness
   - "testability" or "ambiguity" → analyze only testability
   - "both" or "all" → analyze both completeness + testability
2. If scope is NOT mentioned → ask the user:
   "Do you want a Completeness analysis, a Testability analysis, or both?"
3. Do NOT assume both unless explicitly requested.

---

# 5. Outputs

Generate a single Markdown analysis report document for each input.

The generated file shall:
- Be saved in the `analysis-reports/` folder.

## Output File Naming Convention

Generate one output file per input.

File location
```
analysis-reports/
```

File naming convention
```
RA-<UC ID>-<Use Case Name>.md
```

Rules
- Prefix with `RA-` (Requirement Analysis).
- Use the Use Case ID exactly as provided.
- Use the Use Case Name.
- Replace spaces with hyphens (`-`).
- Remove unsupported filename characters.
- Preserve the original word order.
- Use lowercase or kebab-case consistently.

Examples
```
RA-US001-User-Login.md

RA-US102-Create-User.md

RA-US003-Reset-Password.md
```

If no Use Case ID is provided, use the feature name.
```
RA-Create-User.md
```

---

# 6. Workflow

High-level workflow only:

```
Load Knowledge
        │
        ▼
Read Input (requirement + determine scope)
        │
        ▼
Analyze (Completeness & Testability)
        │
        ▼
Draft Clarification Questions
        │
        ▼
Self-Validate
        │
        ▼
Generate Output (Analysis Report)
```

---

# 7. Knowledge Sources

Before analyzing requirements, load:

## Standards
- [Requirement Quality Standard](knowledge-base/standards/requirement-quality-standard.md)

## Project Knowledge
- [Test Strategy](knowledge-base/project-knowledge/test-strategy.md)

## Templates
- [Requirement Analysis Report Template](knowledge-base/templates/requirement-analysis.template.md)

## Checklists
- [Clarification Questions](knowledge-base/checklists/clarification-questions.md)
- [Requirement Analysis Checklist](knowledge-base/checklists/requirement-analysis-checklist.md)

## Examples
- [Login Requirement Analysis Example](knowledge-base/examples/requirement-analysis-login-example.md)

---

# 8. Execution Rules

Execution sequence:

1. Read the full requirement input.
2. Load all Knowledge Sources listed above (standards, rubric, checklists, templates, examples).
3. Determine analysis scope (completeness, testability, or both) using the Decision Rules in Inputs. If the requirement is too incomplete to analyze at all (e.g. missing title, actor, or goal), stop and ask the user for the missing document or context instead of guessing.
4. Apply the [Requirement Quality Standard](knowledge-base/standards/requirement-quality-standard.md) (SMART criteria, Testability Requirements, Given/When/Then format) to evaluate each acceptance criterion individually — do not evaluate the requirement only as a whole.
5. Apply the Coverage Expectations below and flag ambiguous terms (e.g. "fast", "user-friendly", "appropriate", "several") rather than silently interpreting them.
6. Draft one clarification question per identified gap or ambiguity, using the Clarification Questions checklist to guide phrasing.
7. Assess overall requirement readiness against the Requirement Quality Standard's Definition of Ready (DoR) checklist — mark **Ready** or **Not Ready**, and list which DoR items are unmet.
8. Execute Self-Validation (Section 9) before producing the final report.

General rules:
- The AI must behave as a deterministic requirement analyzer, not a rewriter — do NOT rewrite the requirement unless explicitly asked.
- Avoid duplicate findings that flag the same gap more than once.
- Every finding must reference the specific line, section, or acceptance criterion it relates to.
- Every "missing acceptance criteria" finding must state what the missing criterion should cover (e.g. error handling, validation, permissions), not just that something is missing.

### Coverage Expectations

**Completeness**
- Acceptance criteria present for every stated behavior
- Actors/roles and permissions defined
- Preconditions and postconditions defined
- Error and exception handling addressed
- Data/field-level rules (required fields, formats, limits) defined
- Non-functional expectations addressed when relevant (performance, security, accessibility)

**Testability**
- Each acceptance criterion is specific, measurable, and verifiable
- No vague or subjective language without a defined threshold
- Expected outcomes are observable (not just intent)
- Dependencies and external systems are identified
- Business rules are stated as explicit, checkable conditions

**Ambiguity Detection**
- Vague qualifiers (e.g. "fast", "easy", "secure", "appropriate")
- Undefined terms or acronyms
- Conflicting statements between acceptance criteria
- Missing "what happens if..." (negative/edge case) scenarios

**Clarification Questions**
- Generate one clarification question per identified gap or ambiguity
- Group questions by category (Completeness, Testability, Business Rules, Non-Functional)
- Phrase questions so a Product Owner/BA can answer them directly

### Anti-Fixed-Quota Rule

The AI MUST NOT:
- Use a fixed number of findings or questions per requirement
- Force equal findings across all acceptance criteria
- Inflate or reduce findings artificially to look thorough

All analysis MUST be based on: actual gaps found in the requirement, risk and impact of the gap, requirement complexity, and requirement intent.

---

# 9. Self-Validation

Mandatory validation before output. Before generating the final report, execute the [Requirement Analysis Checklist](knowledge-base/checklists/requirement-analysis-checklist.md) and confirm:
- Standard applied (Requirement Quality Standard — SMART criteria, Testability Requirements, DoR checklist)
- Checklists executed (Clarification Questions, Requirement Analysis Checklist)
- Template followed (Requirement Analysis Report Template)
- Output complete (every AC evaluated, every gap has a clarification question, Ready/Not-Ready status assigned)

If the analysis fails self-check:
- AI MUST revise internally
- AI MUST NOT output partial results
- AI MUST NOT expose reasoning or checklist
- Fix any failures before saving. Do not generate a review-of-the-review report. Only use the checklist internally to verify quality.

---

# 10. Success Criteria

- All requirements in scope have been analyzed.
- All items in the [Requirement Analysis Checklist](knowledge-base/checklists/requirement-analysis-checklist.md) are satisfied.
- Every acceptance criterion in the source requirement has been evaluated for completeness and testability.
- Every identified gap or ambiguity has a corresponding clarification question, and clarifications are documented in the output report.
- The r