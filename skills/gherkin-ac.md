---
name: gherkin-ac
description: "Writes or converts Acceptance Criteria into Gherkin (BDD) format: Given / When / Then. Input: user story, AC, or feature description. Output: structured Gherkin scenarios covering happy path, alternative flows, and edge cases. Use when the team practices BDD or QA writes automated tests."
---

# Gherkin AC

## What this skill does

Writes Acceptance Criteria in Gherkin syntax from a user story or feature description. Covers happy path, alternative flows, and edge cases as separate scenarios.

## When to use

- Team practices BDD
- QA writes automated tests in Cucumber or similar
- Requirements need formal, testable scenario format

## Input

Provide one of:
- User story (full text)
- AC written in plain language
- Feature description

## Output format

```gherkin
Feature: [short functional description]
  As a [role]
  I want to [goal]
  So that [value]

  Background:
    Given [shared precondition]
    And [shared precondition]

  Scenario: [happy path name]
    When [action]
    And [action]
    Then [observable outcome]
    And [observable outcome]

  Scenario Outline: [negative or edge case name]
    When [action with <variable>]
    Then [outcome with <variable>]

    Examples:
      | field   | message         |
      | [value] | [expected msg]  |
```

## Rules

- Follow Feature / Background / Scenario / Scenario Outline + Examples structure strictly
- Use 2-space indentation
- Business language only — no implementation or API details
- Each Then = observable outcome (what the user sees, not what the system does internally)
- Separate scenarios for: happy path / alternative flows / error cases
- Do not invent roles, fields, or logic not present in the input
- Mark unclear items as TODO instead of assuming
- Write in English
