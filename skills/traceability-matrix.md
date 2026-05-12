---
name: traceability-matrix
description: "Builds a traceability and progress matrix in xlsx format. Input: Confluence specification pages + Jira project. Output: traceability.xlsx with Team / Epic / Feature / US / Task /
---

# Traceability Matrix

## What this skill does

Reads Confluence specifications and Jira tickets and builds a traceability matrix in xlsx format. Shows the full picture: what is covered, what is in progress, what is done — per feature, per team.

## Input

- Confluence: space key + parent specification page
- Jira: project key
- Project Charter from Project Knowledge (for team and epic structure)

*MCP reads both systems directly — no export needed.*

## Output: traceability.xlsx

### Sheet 1: Traceability and Progress

One row per task (FE and BE are separate rows).

| Team | Epic | Feature / Integration | User Story | Task | Requirements Status | Development Status |
|---|---|---|---|---|---|---|---|---|---|---|

*Weights can be adjusted per project.*

## Rules

- Read from live Jira and Confluence — do not use cached data
- One row per task, not per story
- Progress % calculated strictly from weights — do not estimate
- Flag rows where Requirements Status is missing — these are gaps
- Write in English

## Output

xlsx file delivered via present_files.
