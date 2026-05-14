---
name: rma-generation
description: "Generate a Requirements Management Approach (RMA) document from `01_project_context.md`, `governance.md`, methodology, and tool set. Use during project setup or onboarding to formalize requirements lifecycle, traceability approach, attributes, and tooling. Publishes to Notion via MCP. Tool set: Asana (execution) + Notion (documentation)."
---

# /rma-generation

## What it does

Generates a Requirements Management Approach document for a delivery project.
Structures project context, governance rules, and tool configuration into a formal RMA that covers the requirements lifecycle, traceability, attributes, and tool access.

Use when: project setup or onboarding, before the first refinement session.

## Input

Primary (read from Project Knowledge via MCP or pasted):
- `01_project_context.md` — project name, methodology, team, constraints
- `governance.md` — prioritization, approvals, change control, DoR/DoD

Secondary (ask BA if not found in primary):
- Methodology: Scrum / Kanban / hybrid
- Asana workspace name and project link
- Notion workspace link and target page for RMA
- Any additional tools (Miro, Figma, Google Drive, etc.)

## Source priority

1. Explicit BA input for the current session — overrides everything
2. `governance.md` — change control, approvals, prioritization
3. `01_project_context.md` — project name, team, constraints
4. Geniusee BA defaults — fallback for unspecified items, flag as default

## Process

### Step 1 — Gather inputs

Read `01_project_context.md` and `governance.md` from Project Knowledge.
Extract:
- Project name
- Methodology (Scrum / Kanban)
- Team roles (BA, PM, PO, dev, QA)
- Tool set (Asana + Notion + any additional tools)
- Change control approach
- Prioritization and approval approach

Present summary to BA before writing:

```
Inputs found:
- Project: [name]
- Methodology: [Scrum / Kanban / TBD]
- Team roles: [list]
- Tools confirmed: Asana, Notion, [others if any]

Gaps — need BA input:
- [list of missing values]

Proceed? (yes / provide context)
```

### Step 2 — Generate document section by section

Do not output all sections at once. Generate, then continue.

### Step 3 — Publish to Notion

Create Notion page via MCP:
- Title: `Requirements Management Approach — [Project Name]`
- Parent: provided parent page, or ask BA
- Status: Draft

### Step 4 — Link to Asana

Add Notion RMA page link to the Asana project description or relevant setup task if accessible via MCP.

### Step 5 — Summary

```
✅ RMA published to Notion: [link]
Asana project description updated with RMA link.

Sections with TBD content (requires BA follow-up):
- [list]

Suggested next step: share with PM / PO for alignment before first refinement.
```

---

## Output template

```markdown
# Requirements Management Approach — [Project Name]

_Version: 1.0 | Author: [BA name] | Date: [date] | Status: Draft_

---

## 1. Description

Requirements management is an iterative process aimed at defining business,
functional, and non-functional requirements to design, develop, implement,
and maintain a product throughout its lifecycle.

This document formalizes how requirements are managed on [Project Name]:
stages, activities, responsible roles, tools, and traceability approach.

**Principles:**
- Transparency in the current requirements status
- Quick access to requirements and their attributes
- Clear understanding of requirements processing at every stage

---

## 2. Requirements Management Process

**Methodology:** [Scrum / Kanban / Hybrid]

### Stage 1 — Initializing

New requirements are identified, added to the backlog, and get basic attributes.

| Activity | Responsible | Tool |
|---|---|---|
| Identify business needs | BA, PO | Asana (backlog) |
| Add to backlog | BA | Asana |
| Assign priority and source | BA | Asana (fields) |
| Initial conflict resolution | BA, PO, PM | Notion (open questions log) |

### Stage 2 — Middle (iterative)

Requirements are elicited, specified, prioritized, estimated, approved, developed, and tested.

| Activity | Responsible | Tool |
|---|---|---|
| Elicitation (interviews, workshops) | BA | Notion (notes), Miro (if applicable) |
| Specification (user stories, AC) | BA | Notion |
| Prioritization | BA, PO, PM | Asana (ranking / fields) |
| Estimation | Dev team | Asana |
| Approval | PO / Client | Notion (comments) / Slack |
| Development | Dev team | Asana (tasks) |
| Testing | QA | Asana (subtasks) |

### Stage 3 — Finalizing

Requirements are completed, released, demonstrated, and closed or transitioned to support.

| Activity | Responsible | Tool |
|---|---|---|
| Mark requirement as Done | BA | Asana (status → Done) |
| Final UAT validation | QA, PO | Asana |
| Requirement retirement or archiving | BA | Notion (page archive) |
| Retrospective input | BA, PM | TBD |

---

## 3. Requirements Traceability

**Responsible:** Business Analyst

**When:** requirements must be traced before grooming, no later than sprint planning.

### Traceability in Asana

Requirements (type = Story / Task) are linked to:

| Link type | Target |
|---|---|
| Subtasks | Development subtasks (BE / FE / DevOps) |
| Subtasks | QA subtasks |
| Dependencies | Blocking / Blocked-by tasks |
| Custom field | Epic / Feature group |
| Attachments / Description links | Notion specification page |

### Traceability in Notion

Each requirement page in Notion links to:

| Link type | Target |
|---|---|
| Relation | Parent epic page |
| Relation | Design (Figma / Miro link) |
| Relation | Asana task URL |
| Inline reference | Related requirements (Notion page links) |

### Dependency types used

| Type | Meaning |
|---|---|
| Blocks / Blocked by | Cannot be started until predecessor is done |
| Relates to | Informational connection, no sequence constraint |
| Duplicates | Same requirement specified elsewhere — consolidate |

---

## 4. Information Management Approach

The BA is accountable for all business analysis information management.

Requirements and designs are stored and accessed as follows:

| Purpose | Tool | Location |
|---|---|---|
| Atomic requirements (stories, AC) | Asana | [Project name] workspace → [Project link] |
| Descriptive documentation (BRD, RMA, specs) | Notion | [Notion space link] |
| Visual artifacts (process flows, mind maps) | Miro | [TBD — add board link] |
| Design references | Figma | [TBD — add Figma link] |
| File storage | Google Drive | [TBD — add Drive link] |

Access to all tools is granted by the PM at project start.
The BA maintains the Notion documentation space as the single source of truth for requirements.

---

## 5. Requirements Attributes

| Attribute | Description | Where managed |
|---|---|---|
| Absolute reference | Unique identifier. Not reused if requirement moves or changes. | Asana task ID (auto) |
| Author | BA responsible for the requirement. | Asana (Assignee field) |
| Complexity | Difficulty of implementation. | Asana (custom field: Story Points / T-shirt size) |
| Ownership | Stakeholder who owns the business need. | Notion (requirement page — Owner field) |
| Priority | Relative importance or implementation sequence. | Asana (Priority field) |
| Source | Origin of the requirement. | Notion (requirement page — Source field) |
| Stability | Maturity of the requirement. | Notion (Status field: Draft / Stable / Approved) |
| Status | Current state of the requirement. | Asana (task status) |
| Urgency | How soon the requirement is needed. | Asana (Due date) |
| Dependencies | Horizontal and vertical connections. | Asana (Dependencies / Blocking links) |

---

## 6. Requirements Management Tools

| Tool | Purpose | Access |
|---|---|---|
| Asana | Atomic requirements as tasks/stories. Execution tracking, status, dependencies, subtasks. | [Asana workspace link — TBD] |
| Notion | Descriptive documentation: BRD, RMA, specs, meeting notes, open questions log, glossary. | [Notion space link — TBD] |
| Miro | Visual artifacts: process flows, mind maps, story maps, discovery boards. | [Miro board link — TBD] |
| Figma | UI designs and wireframes linked to requirements. | [TBD] |
| Google Drive | File storage, exports, shared assets. | [TBD] |
| Slack | Asynchronous communication, approvals, change notifications. | [TBD — project channel] |

---

## 7. Change Control Process

Changes to requirements are managed per the BA Governance Approach (`governance.md`).

Summary:

| Change timing | Risk | Process |
|---|---|---|
| Before grooming | Low | BA updates spec in Notion, notifies in Slack |
| After grooming, before sprint | Medium | Re-elicitation, re-estimation, team re-confirms DoR |
| During sprint | High | CR label in Asana, no sprint scope change unless critical blocker |
| Post-release (delivered feature) | High | New Asana task, full BA process, linked to original |

---

## 8. Open Questions

| # | Question | Owner | Target date |
|---|---|---|---|
| 1 | Asana workspace link — confirm access for BA | PM | TBD |
| 2 | Notion space structure — confirm parent page for RMA | BA / PM | TBD |
| 3 | Miro board setup — confirm link and password | PM | TBD |
| 4 | Figma access for BA — confirm link | PM | TBD |
| 5 | Asana custom fields — confirm Story Points / T-shirt size field exists | BA / PM | TBD |
```

---

## Rules

- Do not invent tool links, workspace names, or stakeholder names — use TBD
- If methodology is unknown, generate Scrum version and flag it as default
- Do not include BPMN diagrams in text output — note "diagram TBD, to be added in Miro"
- Change control section must reference `governance.md`, not duplicate it
- Attributes table must reflect Asana + Notion reality — not Jira fields
- All TBD items must be listed explicitly in Step 5 summary

## MCP Path

Input: Notion MCP → read `01_project_context.md` page, `governance.md` page
Output: Notion MCP → `create_page` in target Notion space
Secondary: Asana MCP → `update_project` description with Notion RMA link

## MCP Unavailable Fallback

If MCP is not accessible:
1. Ask BA to paste project context and governance content directly
2. Generate RMA as markdown in chat
3. BA publishes manually to Notion
4. Note: "MCP unavailable — manual publishing required"
