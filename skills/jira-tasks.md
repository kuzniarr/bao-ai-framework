---
name: jira-tasks
description: "Creates a Story + FE Task + BE Task in Jira from an approved user story. Input: Confluence page URL or approved story content. Output: Story + 2 child Tasks in Jira with numbered requirements in descriptions. Uses Atlassian MCP."
---

# Jira Tasks

## What this skill does

Takes an approved user story from Confluence and creates three linked Jira tickets: a Story + FE Task + BE Task. Descriptions are generated from the story content — no copy-paste required.

## Input

Provide one of:
- Confluence page URL (MCP reads it directly)
- Approved story content pasted into chat

*Only run this on approved stories. Do not create Jira tasks from draft requirements.*

## What gets created

**Story**
- Issue type: Story
- Summary: [story title]
- Description: link to Confluence page + one-line summary
- Epic Link: [epic key]

**FE Task** (child of Story)
- Summary: `FE | [story title]`
- Description: numbered FE requirements covering:
  - What FE initiates
  - What FE displays (states: loading, success, error, empty, pending)
  - What FE sends to BE and receives back
  - What FE must NOT do

**BE Task** (child of Story)
- Summary: `BE | [story title]`
- Description: numbered BE requirements covering:
  - Endpoints required
  - Validation rules
  - Storage / DB changes
  - Error responses and status management
  - Edge cases

## Rules

- Do not invent requirements not present in the source story
- Mark unclear items as TBD
- Write all descriptions in English
- After creating all three — return all Jira links

## Output

Story + FE Task + BE Task in Jira.
✅ Return: three Jira ticket links.

## Fallback (non-Cloud Jira)

Output ticket content as structured text. BA creates tickets manually using the generated descriptions.
