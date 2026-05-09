---
name: ears-ac
description: "Writes Acceptance Criteria in EARS syntax (Easy Approach to Requirements Syntax). Input: user story or feature description. Output: structured EARS statements covering happy path, alternative flows, and error cases. Use when formal, unambiguous requirement notation is needed."
---

# EARS AC

## What this skill does

Writes Acceptance Criteria using EARS — a structured notation for unambiguous, verifiable requirements. One system action per statement.

## When to use

- Formal requirement notation is needed
- Requirements must be independently verifiable
- Alternative to Gherkin when BDD tooling is not in use

## EARS patterns

| Pattern | Structure |
|---|---|
| Ubiquitous | The system shall [response]. |
| Event-driven | When [trigger], the system shall [response]. |
| State-driven | While [state], the system shall [response]. |
| Optional feature | Where [feature is present], the system shall [response]. |
| Unwanted behavior | If [condition], then the system shall [mitigation]. |
| Complex | While [state], when [trigger], the system shall [response]. |

## Output format

### Feature: [name]

#### Happy Path
[EARS statements]

#### Alternative Flows
[EARS statements]

#### Error / Edge Cases
[EARS statements]

## Rules

- One requirement = one system action
- Clause order: While → When → the system shall
- Each statement must be independently verifiable
- Negative and exceptional cases use If / Then
- Do not describe UI implementation or internal architecture
- Do not invent behavior not present in the input
- Mark unclear items as TODO
- Write in English
