---
name: user-stories
description: "Generates User Stories and Acceptance Criteria in the project-standard format. Input: requirements.md or feature description. Output: FE | and BE | stories with Input / Process / Alt Flows / Output / AC / Business Rules sections. Publishes to Confluence as child pages via MCP."
---

# User Stories

## What this skill does

Generates FE and BE user stories in the mandatory project format from a feature description or requirements document. Marks unclear fields as TBD rather than inventing details.

## Mandatory story format

Every story is written as two separate stories with the same title:
`FE | [title]` and `BE | [title]`

Each story includes these sections in order:

- **Input** — what triggers this story / what the user or system provides
- **Process** — what happens step by step
- **Alternate Flows & Error Handling** — what happens when things go wrong
- **Output** — what the user or system receives as a result
- **Acceptance Criteria** — verifiable conditions for story completion
- **Business Rules** — constraints, permissions, invariants that apply

## Input

Provide one of:
- Feature name + description
- Requirements document (paste or Confluence URL)
- Use case from the User Story Map

## Output

Two stories (FE | and BE |) in the format above, ready for review.

After BA approval: published to Confluence as child pages of the specification page via Atlassian MCP.

## Rules

- Follow the mandatory story format strictly — do not omit sections
- Do not invent technical details not present in the input
- Mark unclear fields as TBD
- One story = one user goal
- Use business language in AC, not implementation language
- Write in English

## MCP path

After approval:
> For each approved story below — create a child page in Confluence.
> Parent page: [specification page URL]
> Page title = story title. Page content = full story text.

## Fallback (non-Cloud / no MCP)

Output stories as markdown. BA copies into Confluence manually.

## INVEST check

After generating, optionally run `/invest-check` on the output to verify story quality before publishing.
