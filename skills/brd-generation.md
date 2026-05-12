# /brd-generation

## What it does

Generates a Business Requirements Document (BRD) for a feature, epic, or project phase. Structures raw inputs — meeting notes, stakeholder interviews, existing tickets — into a formal BRD covering business context, goals, scope, stakeholder needs, functional requirements, NFRs, and constraints. Output is published to Confluence via MCP.

Use when: starting a new project phase, documenting a large feature set, or when a client requires formal requirements sign-off.

## Input

- Source material: any combination of —
  - Meeting notes or transcript
  - Existing Confluence pages (URLs)
  - Jira epic/stories (project key + epic)
  - Free-text description
- Optional: target Confluence space and parent page for publishing
- Optional: client-specific format requirements (level of formality, sections to include/exclude)

## Output

BRD document published to Confluence with the following structure:

1. Document Info (version, author, date, status)
2. Business Context
3. Business Goals and Success Metrics
4. Scope (in / out)
5. Stakeholders
6. Functional Requirements
7. Non-Functional Requirements
8. Constraints and Assumptions
9. Open Questions
10. Approval

## Process

**Step 1 — Gather and structure inputs**

Read all provided sources via MCP or from pasted content. Extract:
- Business problem being solved
- Goals and measurable success criteria
- Scope boundaries (explicit and implied)
- Stakeholder roles and needs
- Functional requirements (numbered list)
- Known NFRs
- Constraints (technical, regulatory, timeline, budget)
- Open questions (anything not yet decided)

Before writing — present the BA with a summary:

```
I found the following inputs:
- [N] Confluence pages read
- [N] Jira epics/stories read
- [N] requirements extracted

Key gaps I cannot fill without more input:
- [list of TBD items]

Proceed with BRD generation? (yes / provide more context)
```

**Step 2 — Write BRD sections**

Write section by section. Present each section for BA review before moving to the next if the document is large (>5 sections with significant content).

**Section format:**

```markdown
## 1. Business Context

[2–4 sentences describing the business problem or opportunity this document addresses.
Source: [meeting notes / stakeholder interview / existing doc]]

## 2. Business Goals and Success Metrics

| Goal | Success Metric | Target |
|---|---|---|
| [goal] | [how measured] | [threshold] |

## 3. Scope

### In Scope
- [item]

### Out of Scope
- [item]

## 4. Stakeholders

| Role | Name | Interest | Involvement |
|---|---|---|---|
| [role] | [name or TBD] | [what they care about] | [Approver / Consulted / Informed] |

## 5. Functional Requirements

FR-001: [requirement statement]
Source: [meeting / stakeholder / Jira ticket]
Priority: High / Medium / Low

FR-002: ...

## 6. Non-Functional Requirements

NFR-001: [requirement statement with measurable threshold]
Quality attribute: [ISO 25010 category]

## 7. Constraints and Assumptions

### Constraints
- [constraint]

### Assumptions
- [assumption — if proven wrong, this requirement changes]

## 8. Open Questions

| # | Question | Owner | Target date |
|---|---|---|---|
| 1 | [question] | [role] | TBD |

## 9. Approval

| Stakeholder | Role | Decision | Date |
|---|---|---|---|
| [name] | [role] | Pending | — |
```

**Step 3 — Publish to Confluence**

Create Confluence page via MCP:
- Title: `BRD — [Feature / Epic name] — v[version]`
- Parent: provided parent page, or ask BA
- Status: Draft

Add link to Confluence page in relevant Jira epic description.

**Step 4 — Summary**

Return to BA:
```
✅ BRD published to Confluence: [link]
Jira epic updated with Confluence link.

Sections with TBD content (requires BA follow-up):
- [list]

Suggested next step: share with [stakeholder] for review
```

## Rules

- Never generate requirements not grounded in provided source material
- All TBD items must be explicitly listed — do not fill with plausible-sounding content
- Functional requirements must be numbered (FR-001, FR-002...) for traceability
- Do not generate approval signatures or claim anyone has approved — leave Approval table as Pending
- Level of formality follows client preference — if unknown, default to structured but readable (not legalese)
- Anti-hallucination: stakeholder names, dates, technical values — TBD if not provided

## MCP Path

Input: `confluence_get_page` for referenced pages, `jira_get_project_issues` or `jira_get_issue` for epics/stories
Output: `confluence_create_page` in target space + `jira_update_issue` to add Confluence link to epic

## MCP Unavailable Fallback

If MCP is not accessible:
1. Ask BA to paste source content directly
2. Generate BRD as markdown in chat
3. BA publishes manually to Confluence
4. Note: "MCP unavailable — manual publishing required"
