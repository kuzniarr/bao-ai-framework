---
name: ac-validation
description: "Review Acceptance Criteria for one or more user stories against completeness and quality standards. Use when checking happy path, alternative flows, edge cases, error states, and AC quality without rewriting the AC."
---

# /ac-validation

## What it does

Reviews Acceptance Criteria for a user story or a set of stories against completeness and quality standards. Checks coverage of happy path, alternative flows, edge cases, and error states. Does not rewrite AC — returns a structured list of issues only, so the BA decides what to fix.

Complements `/invest-check` (which evaluates story-level quality) — this skill goes deeper into AC content.

## Input

- User story with AC (Confluence page URL, Jira ticket key, or pasted content)
- Optional: scope note — "check only happy path coverage" or "focus on edge cases"

## Output

Structured review report per story:
- **Coverage verdict**: pass / partial / fail
- **Missing scenarios**: list of flows or cases not covered
- **Quality issues**: ambiguous, untestable, or duplicate AC items
- **Suggested additions**: short description of what to add (not full AC — that stays with the BA)

## Process

**Step 1 — Read the story and AC**

If input is a Confluence URL or Jira key — fetch via MCP. If pasted — use directly.

Extract:
- User story statement (who / what / why)
- Full list of AC items

**Step 2 — Check coverage**

For each story, verify AC covers:

| Coverage area | Check |
|---|---|
| Happy path | Main successful flow is described end-to-end |
| Alternative flows | At least one alternative path (if applicable) |
| Negative / error states | Invalid input, system errors, boundary violations |
| Edge cases | Empty states, maximum values, concurrent actions (if applicable) |
| Role / permission variations | If multiple roles exist — each covered (if applicable) |

Mark each area: ✅ covered / ⚠️ partial / ❌ missing

**Step 3 — Check AC quality**

Each AC item must be:

| Quality criterion | Description |
|---|---|
| **Atomic** | One condition per AC item — not bundled |
| **Testable** | Can be verified with a clear pass/fail outcome |
| **Unambiguous** | No "should", "may", "appropriately", "fast" without measurable value |
| **Complete** | Includes enough detail to implement and test without additional questions |
| **Consistent** | Does not contradict other AC items or the story statement |

Flag any AC item that fails one or more criteria.

**Step 4 — Deliver report**

Return a structured report:

```
## AC Validation Report — [Story title]

### Coverage
✅ Happy path — covered
⚠️ Alternative flows — partial: only [X] covered, [Y] missing
❌ Error states — not covered

### Missing scenarios
- [ ] What happens if [field] is left empty
- [ ] What happens when [action] is performed by [role B]

### Quality issues
- AC item 3: ambiguous — "the system responds quickly" has no measurable threshold
- AC item 5: bundled — covers two separate conditions, split into two items

### Suggested additions
- Add AC for empty state: "When no [X] exists, the system displays [message]"
- Add error AC: "When [Y] fails, the user sees [error] and can [action]"
```

**Step 5 — Summary if multiple stories**

If reviewing more than one story — add a summary table:

| Story | Coverage | Quality issues | Status |
|---|---|---|---|
| [name] | ✅ / ⚠️ / ❌ | [N] | Pass / Needs work |

## Rules

- Do not rewrite AC — flag issues only
- Do not add AC items not grounded in the story statement or known project context
- If a coverage area is not applicable (e.g., no roles exist) — mark as N/A, not ❌
- If story statement is missing or vague — flag it before checking AC; AC cannot be validated without a clear story
- Anti-hallucination: if a business rule or flow is unclear — write TBD, do not infer

## MCP Path

If Confluence URL provided:
→ Fetch page via Atlassian MCP → extract story + AC → run validation

If Jira key provided:
→ Fetch issue via Atlassian MCP → extract description + AC field → run validation

## MCP Unavailable Fallback

If MCP is not accessible:
→ Ask BA to paste story and AC directly into chat
→ Run validation from pasted content
→ Do not generate placeholder AC or invented scenarios
