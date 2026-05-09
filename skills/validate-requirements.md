---
name: validate-requirements
description: "Validates a requirements document or user story set against structure, scope, clarity, FE/BE split, and API mapping. Input: requirements document or story set. Output: grouped issues list (Structure / Scope / Clarity / FE / BE / Mapping) + Questions table split into Client and Dev Team."
---

# Validate Requirements

## What this skill does

Reviews a requirements document or user story set and returns a grouped list of issues — without rewriting the document. Ends with a structured questions table split into two tracks: client and dev team.

## Input

Paste the requirements document or user story set directly into the chat after invoking the skill.

## Output format

Return issues grouped under these headers:

### Structure
Is everything logically grouped and easy to follow?

### Scope
Any missing flows? Check: disconnect flow, reconnect flow, error states, empty states, edge cases, permission-based restrictions.
Always check disconnect flow first — it is missing in most integrations.

### Clarity
Anything ambiguous or underspecified for a developer?

### FE
Is it clear what the frontend is responsible for? Cover: what FE initiates, displays, sends, receives, and must NOT do.

### BE
Is it clear what the backend is responsible for? Cover: endpoints, validation, storage, error responses, status management.

### Mapping
Are all required API fields covered with a source?
Cross-reference against the channel's official API documentation.

### Questions

Split into two tables:

**Questions for Client** — business decisions, scope confirmations, approval needed

| # | Question | Priority |
|---|---|---|

**Questions for Dev Team** — architecture, token lifecycle, current implementation details

| # | Question | Priority |
|---|---|---|

## Rules

- Check actual API documentation of the channel on the web
- Apply best architecture and software development practices
- Do not rewrite the document — only list what needs to be fixed
- Be specific — point to exact section or field, not general comments
- Token lifecycle (type, expiry, refresh, reconnect flow) must be explicitly documented for every integration — flag if missing
- Do not invent issues not supported by the document content
- If something is unclear, mark it as TBD in the Questions section

## After output

Ask: "Prepare an updated document with highlights + questions list?"
