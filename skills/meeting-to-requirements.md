---
name: meeting-to-requirements
description: "Converts a meeting transcript into structured requirements. Input: transcript.md — paste or upload. Output: requirements.md in Confluence — decisions captured, action items assigned, requirements extracted and numbered by FE/BE."
---

# Meeting to Requirements

## What this skill does

Takes a raw meeting transcript and extracts structured requirements from it. Separates decisions, action items, and open questions. Outputs numbered requirements ready for a Confluence page.

## Input

Paste the meeting transcript directly into the chat after invoking the skill.
*Works with raw transcripts, auto-generated captions, or cleaned-up notes. The messier the input, the more the skill helps.*

## Output structure

### Decisions
What was agreed during the meeting. Format: decision + who decided + date.

### Action Items
| # | Action | Owner | Due |
|---|---|---|---|

### Requirements
Numbered list split by side:

**FE Requirements**
1. [requirement]

**BE Requirements**
1. [requirement]

### Open Questions
Items that came up but were not resolved. Format: question + context + who needs to answer.

## Rules

- Extract only what was explicitly discussed — do not infer or expand
- Mark ambiguous items as TBD in Open Questions
- One requirement = one specific, actionable item
- Reference specific fields, endpoints, or flows where mentioned
- Write in English

## MCP path

After BA review:
> Create a requirements page in Confluence.
> Space: [space key]. Parent: [specification page].
> Title: Requirements — [feature name].
> Content: [paste approved output].

## Fallback

Output as markdown. BA copies into Confluence or Google Doc manually.
