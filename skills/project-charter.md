---
name: project-charter
description: "Generate `01_project_context.md` for Project Knowledge from kickoff notes, PM messages, WBS fragments, onboarding notes, transcripts, or free-text project descriptions. Use at the start of a new project or when onboarding into an existing one to capture project overview, business goal, scope, users, constraints, risks, and project identifiers. Ask up to 3 targeted questions only when business goal or high-level scope is unclear."
---

# Project Charter

Generate `01_project_context.md` as the foundational project context file for Project Knowledge. Use it as the baseline reference for future BA work on the project.

## Inputs

Any combination of:
- Kickoff notes, PM Slack messages, free-text project description
- Scope & Vision document (Google Doc, PDF, Confluence page)
- WBS (Geniusee template or client-provided — epic/scope structure)
- Architectural diagrams (PDF, screenshots — uploaded as images)

On Geniusee delivery projects after discovery — S&V is the primary input, WBS provides scope structure.

## Procedure

### 1. Parse the input

Extract the following if present:

| Field | Look for |
|---|---|
| Client & product | Company name, product, domain |
| Delivery model | T&M, Fixed Price, Pilot, Discovery |
| Business goal | Why the client wants this, expected outcome, success criteria |
| Scope | In-scope items, exclusions, open areas |
| Users & roles | User groups, access levels, internal/external roles |
| Constraints | Deadline, budget model, dependencies, resourcing gaps |
| Risks | Explicit risks or obvious red flags from the source |
| Project identifiers | Jira, Confluence, Figma, Slack, repo, staging URLs |

### 2. Identify critical gaps

Before generating, verify that:
- Business Goal is clear enough to write factually
- High-level Scope exists

If either is missing or too vague, ask a maximum of 3 targeted questions.

Example:

> I have enough to generate the charter. Two things are unclear:
> 1. Is this T&M or Fixed Price?
> 2. Are there any explicit out-of-scope items mentioned?
> If you do not know yet, I will mark them as TBD.

Do not invent missing facts.

### 3. Generate the document

Use the template below. Mark unknown values as `TBD`.
Keep the document concise, factual, and usable in future project work.

```markdown
# Project Charter — [Project Name]
_Last updated: [date]_

## Project Overview
[2–4 sentences: what the product is, who the client is,
what domain, delivery model (T&M / Fixed / Pilot)]

## Business Goal
[WHY — what the client wants to achieve.
What does success look like for them.]

## Scope
**In:** [features, modules, integrations confirmed in scope]
**Out:** [explicitly excluded — design, infra, specific platforms, etc.]
**TBD:** [open scope questions not yet resolved]

## Users & Roles
[Optional — fill if known at kickoff]
- [Role name] — [what they do / access level]
- [Role name] — [what they do / access level]

## Key Constraints
- [Deadline or milestone]
- [Budget type or allocation constraint]
- [Resource gaps, dependencies, external blockers]

## Risks
| Risk | Impact | Notes |
|---|---|---|
| [Risk description] | High / Med / Low | [Mitigation or open question] |

## Project Identifiers
| Tool | Value |
|---|---|
| Jira | [project key] |
| Confluence | [space key or URL] |
| Figma | [link] |
| Slack | [channel name(s)] |
| Other | [e.g. GitHub repo, Staging URL] |
```

### 4. Add BA review note

After the generated document, append:

```markdown
Review before adding to Project Knowledge:
- [ ] TBD fields to resolve
- [ ] Scope Out — confirm with client
- [ ] Project identifiers — fill in if missing
```

## Rules

- If a field is not present in the input, write `TBD`
- Do not invent delivery model, risks, users, or identifiers
- Do not use generic filler text
- Do not document communication cadence or governance here, those belong in separate skills

## Handling large inputs

- Scope & Vision can be 30-100+ pages. Do NOT copy the full S&V text into the charter.
- Extract ONLY what belongs in a charter:
 - Product context (1-2 paragraphs)
 - Business goals (3-7 bullets)
 - Scope highlights (in/out of scope — high level, NOT story-level)
 - Key stakeholders (names, roles)
 - Glossary anchors (5-10 key terms)
 - Constraints and risks
- WBS: extract epic list and phase structure, not stories.
- If S&V section is ambiguous or contradictory — flag with `[TBD — clarify with PM/client]`. Do not paraphrase to hide uncertainty.

## Output

Save as `01_project_context.md`.

## Update trigger

Regenerate or revise this file when:
- scope changes significantly
- a new delivery stream appears
- a major constraint changes
