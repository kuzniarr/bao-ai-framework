---
name: tech-context
description: "Generate `03_tech_context.md` for Project Knowledge from `01_project_context.md`, tech kickoff notes, stack descriptions, API documentation, architecture notes, glossary fragments, or developer input. Use during project setup or onboarding to capture stack, integrations, architecture notes, glossary, technical constraints, and open technical questions. Ask about domain-specific terms only when glossary language is missing, and never invent API fields, endpoints, or integration details."
---

# Tech Context

Generate `03_tech_context.md` as the technical grounding file for the project. Use it to support technically accurate requirements writing, API mapping, integration work, and validation with the tech team.

## Inputs

- Architectural diagrams (PDF, screenshots — uploaded directly to Claude)
- API specs (OpenAPI, Postman collections, Confluence pages)
- Tech stack description (free text from tech lead or PM)
- Existing tech docs from Confluence / Google Drive

## Handling diagrams

- Screenshots: upload directly to chat — Claude reads images natively
- PDF: extract key diagrams first (use /mnt or screenshot the relevant pages). Do NOT pass full multi-page PDFs.
- For each diagram, capture: components, integration points, data flow direction. Reference diagram by name in output.

## Procedure

### 1. Extract technical context

Identify, if present:
- frontend and backend stack
- database and infrastructure
- authentication approach
- PM and design tools when relevant for BA work
- external integrations and APIs
- key technical constraints

### 2. Build glossary

Extract domain-specific terms from the input.
For each term:
- write a concise definition in 1 to 2 sentences
- add synonyms or abbreviations if provided
- flag whether clarification is needed

If no glossary language is provided, ask:

> Are there any domain-specific terms, abbreviations, or concepts the team uses that I should know? This helps me write requirements using the correct language.

### 3. Document integrations

For each integration or external API, capture:
- purpose on this project
- data flow direction: inbound, outbound, or bidirectional
- owner: client-side, third-party, or internal team
- constraints or limitations if known

### 4. Flag gaps

After generating, explicitly flag:
- stack areas marked `TBD`
- integrations where flow or ownership is unclear
- glossary terms that need confirmation

## Output template

```markdown
# Tech Context — [Project Name]
_Last updated: [date]_

## Tech Stack

| Layer | Technology | Notes |
|---|---|---|
| Frontend | [e.g. TypeScript / React] | [Notes] |
| Backend | [e.g. Python] | [Notes] |
| Database | [TBD] | |
| Infrastructure | [e.g. Cloudflare Pages] | |
| Auth | [e.g. Magic link / OAuth] | |
| PM Tool | [e.g. Jira — project key: EWA] | |
| Design | [e.g. Figma] | |

---

## External Integrations & APIs

| Integration | Purpose on This Project | Direction | Owner | Constraints / Notes |
|---|---|---|---|---|
| [Integration] | [Purpose] | Inbound / Outbound / Bidirectional | [Owner] | [Constraints / notes] |

---

## Architecture Notes

[High-level description of system structure, component relationships, and data flow.]

---

## Domain Glossary

| Term | Definition | Synonyms / Abbreviations | Needs Clarification |
|---|---|---|---|
| [Term] | [Clear 1–2 sentence definition] | [Alt names] | Yes / No |

---

## Key Technical Constraints

- [Constraint]
- [Constraint]

---

## Open Technical Questions

- [ ] [Question about API, ownership, data flow, stack, or terminology]
```

## Rules

- Do not invent API field names, endpoint paths, payloads, or data types
- Do not assume integration direction without evidence
- Do not define glossary terms from generic knowledge if the project may use them differently
- If the stack is partially known, fill only confirmed layers and mark the rest as `TBD`

## Output

Save as `03_tech_context.md`.
