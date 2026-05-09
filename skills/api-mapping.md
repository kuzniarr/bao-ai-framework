---
name: api-mapping
description: "Builds an API field mapping table between product UI fields and a third-party API. Input: official API documentation URL + product context. Output: mapping.xlsx with columns: Level / API Object / API Field / Required / Product Field / Notes. One sheet per campaign type if applicable."
---

# API Mapping

## What this skill does

Reads the official API documentation and maps each API field to the corresponding product UI field. Identifies required vs optional fields, documents conditional logic, and flags gaps where the product field is unknown.

## Input

- Official API documentation URL (skill reads it directly via web)
- Product context from Project Knowledge (UI fields, use cases)
- Use case: campaign creation / connection / other

*Always read from the live API docs — do not use cached or assumed field names.*

## Output: mapping.xlsx

One sheet per integration type (e.g. Search, Display, Video) if the API has multiple campaign types.

| Level | API Object | API Field | Required | Product Field | Notes |
|---|---|---|---|---|---|

**Levels:** Business / Budget / Campaign / AdGroup / Ad / Creative

**Required:** yes / no — based strictly on official API docs.

**Product Field:** matching UI field name. If unknown — TBD.

**Notes:** conditional logic, default values, constraints, edge cases.

## Rules

- Read actual API documentation — do not invent fields
- Required = as defined in official docs, not assumed
- If product field is unknown — mark as TBD, do not guess
- If a field has conditional logic — explain the condition in Notes
- If the integration has multiple campaign types — generate a separate sheet per type
- Write in English

## Output

xlsx file delivered via present_files.
