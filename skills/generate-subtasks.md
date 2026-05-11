# /generate-subtasks

## Purpose

Break down a Jira Story into Subtasks following the project's configured task split pattern. Pattern is project-specific — defined once in `00_quick_standards.md`, applied consistently throughout the project.

## When to invoke

- After Story is in Jira (created via `/push-to-jira`)
- When the team is ready to split work into implementation pieces
- Before sprint planning, when granular tracking is needed

## Prerequisites

- Story exists in Jira
- `00_quick_standards.md` contains a `## Task Split Pattern` section
- Jira MCP connector is active (or fallback mode)

## Input

One of:

- Jira Story ticket key (skill reads via MCP)
- Story title + AC pasted directly + target Jira project

Context (read automatically from Project Knowledge):

- `00_quick_standards.md` → `## Task Split Pattern` section
- `03_tech_context.md` → tech stack for context-aware subtask descriptions

## Task Split Pattern — expected format

In `00_quick_standards.md`, BA defines the split pattern for the project:

**Simple format:**
Task Split Pattern
Story → Subtasks: [FE, BE, Design, QA]

**Detailed format (recommended):**
Task Split Pattern
FE: UI implementation, frontend logic, integration with BE
BE: API endpoints, business logic, data layer
Design: UX/UI mockups, design review
QA: test plan, test execution, regression
DevOps: deployment, infrastructure (when story affects infra)

**Flat format (no split):**
Task Split Pattern
Story → Subtasks: none (single implementation ticket)

If pattern is missing or ambiguous in `00_quick_standards.md` — ask the BA before proceeding.

## Process

1. Read the parent Story from Jira (via MCP) or from input.
2. Read Task Split Pattern from `00_quick_standards.md`.
3. For each subtask category defined in the pattern:
   - Generate a subtask description tailored to the story's scope
   - Reference relevant AC fragments
   - Use tech context for accuracy (stack, conventions)
4. Create subtasks in Jira as children of the parent Story.
5. Return the list of created subtask keys.

## Output

- Subtasks created in Jira, linked to the parent Story
- Each subtask has:
  - **Title:** `[Category] Story title`
  - **Description:** scope of work for that category, referencing the parent Story's AC

- List of created subtask keys returned to the BA

## Non-Cloud Jira fallback

If MCP is unavailable:

- Output the structured subtask content as text — one block per subtask, with title and description
- BA creates subtasks manually in Jira

## Behavior parameters

- Temperature: 0.3
- Anti-hallucination: do not invent technical details — if scope is unclear for a category, write `TBD` and flag it
- Output focus: each subtask is self-contained and ready for assignment

## What this skill does NOT do

- Does not create the parent Story — use `/push-to-jira` first
- Does not invent split patterns — reads from `00_quick_standards.md`
- Does not estimate or assign subtasks — that is the team's responsibility
