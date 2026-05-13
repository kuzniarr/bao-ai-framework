---
name: meeting-to-requirements
description: "Convert a meeting transcript or raw session notes into structured decisions, action items, requirements, and open questions. Use after discovery, refinement, or client meetings to create implementation-agnostic requirement output."
---

# /meeting-to-requirements

## Purpose

Convert a meeting transcript or raw session notes into structured, validated content. Extract decisions, action items, requirements, and open questions — implementation-agnostic.

## When to invoke

- After a client call or stakeholder session
- When raw notes need to be turned into actionable artifacts
- Before adding new scope to a specification

## Input

Meeting transcript or raw notes — any format:

- Bluedot export
- Manually typed notes
- Pasted chat
- Voice memo summary

Context (read automatically from Project Knowledge):

- `01_project_context.md` — scope, glossary, domain terminology
- `02_stakeholders.md` — speaker identification (if applicable)

## Process

1. Read the input.
2. Categorize content into 4 sections:
   - **Decisions** — explicit agreements made in the meeting
   - **Action Items** — tasks with owner and due date if mentioned
   - **Requirements** — new functional or non-functional needs surfaced
   - **Open Questions** — items raised but not resolved
3. For each Requirement:
   - Write as a numbered, plain-text statement
   - **No implementation tag** (no FE/BE/Design/etc split)
   - Use project glossary
   - Mark assumptions explicitly if making them
4. Anti-hallucination:
   - Use only what was said in the source
   - Do not infer scope or decisions not explicitly made
   - Mark unclear items as `TODO` or `Assumption: [details]`

## Output format

### Decisions

1. [decision text]
2. [decision text]

### Action Items

| # | Item | Owner | Due |
|---|------|-------|-----|
| 1 | ... | ... | ... |

### Requirements

1. [requirement statement]
2. [requirement statement]

### Open Questions

1. [question] — flagged for follow-up

## Output destination

- Return as structured markdown by default
- BA decides where to publish (Confluence specification page, Jira comment, etc.)
- Optional: if BA requests + target Confluence page provided → publish via MCP

## Behavior parameters

- Temperature: 0.2 (extraction, not creation)
- Tone: factual, neutral
- Anti-hallucination: strict — never invent decisions or requirements not in the source

## What this skill does NOT do

- Does not decompose epics — use `/decompose-epic`
- Does not write AC — use `/gherkin-ac` or `/ears-ac`
- Does not split requirements by implementation area
