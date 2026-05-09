---
name: invest-check
description: "Reviews user stories against INVEST criteria: Independent, Negotiable, Valuable, Estimable, Small, Testable. Input: one or more user stories. Output: pass/fail per criterion + specific improvement suggestions. Does not rewrite stories — gives targeted feedback only."
---

# INVEST Check

## What this skill does

Reviews user stories against INVEST and returns targeted feedback — what passes, what fails, and exactly how to fix it. Does not rewrite stories.

## Input

Paste one or more user stories after invoking the skill.

## INVEST criteria

**Independent** — Can this be implemented and tested without depending on another unfinished story?

**Negotiable** — Is the story flexible enough for the team to discuss implementation options?

**Valuable** — Does it clearly express user or business value?

**Estimable** — Is the scope clear and bounded enough to estimate?

**Small** — Is it focused on one user goal? Can it be completed in a sprint?

**Testable** — Can success or failure be verified with clear, specific acceptance criteria?

## Output format

**Story: [title]**

| Criterion | Result | Issue |
|---|---|---|
| Independent | ✅ / ⚠️ / ❌ | [specific issue if any] |
| Negotiable | ✅ / ⚠️ / ❌ | |
| Valuable | ✅ / ⚠️ / ❌ | |
| Estimable | ✅ / ⚠️ / ❌ | |
| Small | ✅ / ⚠️ / ❌ | |
| Testable | ✅ / ⚠️ / ❌ | |

**Improvement suggestions:**
[specific, actionable points — not a rewrite]

## Rules

- ✅ = fully meets criterion
- ⚠️ = partially meets, specific risk noted
- ❌ = fails, must be fixed before development
- Do not rewrite the story — targeted improvement points only
- Be specific: reference the exact section or AC that fails
- Write in English
