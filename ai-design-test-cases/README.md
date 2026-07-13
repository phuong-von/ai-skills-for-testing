# ai-design-test-cases

A Cowork skill that generates test cases from an analyzed requirement, applying approved test design techniques (Equivalence Partitioning, Boundary Value Analysis, Decision Tables, State Transition Testing) for complete positive, negative, and boundary coverage.

Self-contained — bundles its own reference knowledge base (`knowledge-base/`), so it installs and works standalone without any other project setup.

## Install

Download [`ai-design-test-cases.skill`](./ai-design-test-cases.skill) and install it in Cowork via **Settings > Capabilities > Skills > Install from file**.

## What it does

Takes a requirement analysis report (output of `ai-analyze-requirements`) and produces a Markdown test case document with:
- One test case per meaningful equivalence partition/scenario (not a fixed quota)
- Positive, negative, and boundary coverage for every acceptance criterion
- Full traceability back to the source requirement's acceptance criteria

See `SKILL.md` for the full specification (inputs, outputs, execution rules, self-validation).
