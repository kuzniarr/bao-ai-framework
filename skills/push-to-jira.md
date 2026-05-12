# /push-to-jira

## Purpose

Create a Jira Story ticket from an approved user story with acceptance criteria. One ticket per story. Implementation breakdown happens later via `/generate-subtasks`.

## When to invoke

- After story is approved and AC is written (Gherkin or EARS)
- When moving from specification phase to development phase

## Prerequisites

- Story is approved
- Acceptance Criteria exist (Gherkin or EARS)
- Jira MCP connector is active (or fallback mode if not)

## Input

One of:

- Confluence page URL (story + AC)
- Pasted story content with AC included

Context (read automatically from Project Knowledge):

- `01_project_context.md` — Jira project key, naming conventions
- `00_quick_standards.md` — story format standards

## Process

1. Read the story and AC from the input source (via MCP if URL provided).
2. Validate: story has all required sections — title, context/goal, AC. If missing — flag the gap and stop.
3. Identify target Jira project from `01_project_context.md`. If unclear — ask the BA.
4. Format the story for Jira:
   - **Title:** from story header
   - **Description:** context + user goal + AC (in the format used: Gherkin or EARS)
   - **Link:** to source Confluence page
5. Create Story ticket in Jira via MCP.
6. Return ticket key + URL.

## Output

- Single Jira Story ticket created with full description and AC included
- Linked to the Confluence specification page
- Ticket key and URL returned to the BA

## MCP unavailable fallback

If MCP is unavailable:

- Output the formatted ticket content as structured text:
  - Title
  - Description (with AC inline)
  - Suggested labels (if any pattern in the project)

- BA copies into Jira manually

## Behavior parameters

- Temperature: 0.2 (deterministic transformation, not generation)
- Anti-hallucination: do not invent ticket fields, labels, components, or assignees not present in input or context

## What this skill does NOT do

- Does not create subtasks — use `/generate-subtasks` after the Story is in Jira
- Does not split work by FE/BE/Design/etc — that is `/generate-subtasks` responsibility
- Does not write AC — use `/gherkin-ac` or `/ears-ac` first
- Does not generate stories from scratch — use `/decompose-epic` first
