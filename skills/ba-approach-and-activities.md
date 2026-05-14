---
name: ba-approach-and-activities
description: "Generate `ba_approach_and_activities.md` from `01_project_context.md`, `02_stakeholders.md`, `governance.md`, `comm_plan.md`, and team composition. Use during project setup or kickoff to formalize BA role, activities, deliverables, allocation, and mutual commitments with team and client. Includes explicit Service Menu (what BA does / does not do) and Expectations section for kickoff sign-off. Tool set: Asana (execution) + Notion (documentation)."
---

# /ba-approach-and-activities

## What it does

Generates the `BA Approach and Activities` document — the artifact that explicitly states what the BA will do on this project, what is out of BA scope, what BA needs from the team and client, and how BA work will be measured.

This is the document the BA brings to kickoff to align expectations and get sign-off from PM, dev team, and client.

Use when: project setup, kickoff preparation, or when BA role ambiguity is causing scope creep ("BA treated as a scribe instead of a product partner").

## Why this skill exists

Wiki gap analysis shows: governance, communication, and stakeholder skills cover the "how" of BA work but not the "what" and "what not". Without an explicit Service Menu, BA defaults to whatever the loudest stakeholder requests, leading to:

- Scope creep into PM / PO territory
- BA seen as documentation clerk instead of analysis partner
- No baseline for performance assessment

This skill closes that gap.

## Input

Primary (read from Project Knowledge via MCP or pasted):
- `01_project_context.md` — project name, methodology, phases, team
- `02_stakeholders.md` — RACI, decision authority
- `governance.md` — prioritization, approvals, change control
- `comm_plan.md` — meetings, tools, cadence

Secondary (ask BA if not found):
- Methodology (Scrum / Kanban / hybrid)
- BA allocation % on the project
- Whether there are other BAs / Team Lead
- Project phase (Initiation / Discovery / Delivery / Support)
- Known client expectations (Kano wants if collected)

## Source priority

1. Explicit BA input for the current session — overrides everything
2. Project Knowledge documents (charter, stakeholders, governance, comm plan)
3. Geniusee BA defaults — fallback for unspecified items, flag as default

## Process

### Step 1 — Gather inputs

Read primary inputs from Project Knowledge.
Extract:
- Project name, methodology, phases
- Team composition (PM, PO, BA, dev count, QA)
- Client stakeholders and decision authority
- Known meetings and cadence
- Known approval and change control rules

Present summary to BA before writing:

```
Inputs found:
- Project: [name]
- Methodology: [Scrum / Kanban / TBD]
- BA on project: [name + allocation %]
- Other BAs: [list / none]
- Phase: [Initiation / Delivery / etc.]
- Tools: Asana + Notion

Gaps — need BA input:
- [list of missing values, especially Service Menu items]

Proceed? (yes / provide context)
```

### Step 2 — Generate document section by section

Generate in this order, present each section for BA review:
1. BA Role
2. BA Activities (general + special)
3. Collaborative vs Non-collaborative activities
4. Deliverables
5. BA Allocation
6. **Mutual Commitments** (critical — this is the kickoff alignment artifact)
7. Performance Expectations

### Step 3 — Publish to Notion

Create Notion page via MCP:
- Title: `BA Approach and Activities — [Project Name]`
- Parent: provided parent page, or ask BA
- Status: Draft (for kickoff review)

### Step 4 — Generate kickoff agenda block

After publishing, generate a short kickoff agenda block the BA can paste into the kickoff invite or Notion meeting notes:

```
### BA Approach & Mutual Commitments — alignment block (15 min)

1. Walk through Service Menu (what BA does / does not do) — 5 min
2. Confirm what BA needs from client — 3 min
3. Confirm what BA needs from team — 3 min
4. Open questions, adjustments — 4 min

Decision needed: sign-off on BA Approach v1.0
Owner: [BA name]
```

### Step 5 — Summary

```
✅ BA Approach and Activities published to Notion: [link]
✅ Kickoff agenda block generated (above)

Sections with TBD content (requires BA follow-up):
- [list]

Suggested next step: schedule 15-min alignment block during kickoff for sign-off.
```

---

## Output template

```markdown
# BA Approach and Activities — [Project Name]

_Version: 1.0 | Author: [BA name] | Date: [date] | Status: Draft (for kickoff review)_

---

## 1. Project Context

**Project:** [name]
**Methodology:** [Scrum / Kanban / Hybrid]
**Phase:** [Initiation / Discovery / Delivery / Support]
**BA on project:** [name], [allocation %]
**Other BAs:** [list or "none"]

Full project context: [link to `01_project_context.md` in Notion]

---

## 2. BA Role on Project

The Business Analyst on [Project Name] is responsible for:

- Eliciting, analyzing, and specifying business and functional requirements
- Maintaining requirements traceability across Asana and Notion
- Facilitating refinement sessions and removing scope ambiguity
- Owning the requirements management process (per `governance.md`)
- Acting as a thought partner to PO and PM on scope and prioritization decisions

The BA is **accountable** for requirements quality and **responsible** for analysis deliverables.

---

## 3. BA Activities

### 3.1 General BA Activities

| Activity | Description | Input | When | Involved Parties | Risks of Absence |
|---|---|---|---|---|---|
| Requirements Elicitation | Discovery and clarification through interviews, workshops, document analysis | Project context, client input | Beginning of iterations, before refinement | Client, SMEs, BA | Incomplete or incorrect requirements |
| Requirements Specification | Formalization as user stories, AC, models | Elicitation results | During sprint | BA, Dev Team, QA | Implementation errors, ambiguity |
| Requirements Refinement | Backlog grooming, AC validation, INVEST check | Specified requirements | Before sprint planning | BA, Dev Team, PO | Incorrect estimates, false expectations |
| Requirements Estimation Support | Facilitating estimation, clarifying scope | Refined requirements | Mid-sprint or planning | BA, Dev Team, PM | Sprint overload, missed goals |
| Terminology Alignment | Maintaining glossary, concept model | Domain knowledge | Throughout project | BA, Team, Client | Different interpretation, rework |
| Traceability Maintenance | Linking Asana tasks to Notion specs, dependencies | All requirements artifacts | Continuous | BA | Lost context, duplicate work |
| Change Request Processing | Per `governance.md` change control process | Client request | When requested | BA, PM, PO | Uncontrolled scope creep |

### 3.2 Special BA Tasks (project-specific)

| Task | Description | Responsible | Risks of Absence |
|---|---|---|---|
| [e.g. Process Modeling] | [Building BPMN process models for [X] flow] | BA, SMEs | Missed states, logic errors |
| [e.g. User Roles & RBAC] | [Defining roles, access rights, permissions matrix] | BA, Tech Lead | Security gaps, role conflicts |
| [e.g. NFR Specification] | [Collecting and formalizing NFRs per ISO 25010] | BA, Tech Lead | System instability, perf risks |
| [e.g. Data Modeling] | [Conceptual data model, data dictionary] | BA, Tech Lead | Data inconsistency, integration errors |

> ⚠ Customize this table per project. Remove rows that don't apply.

---

## 4. Collaborative vs Non-Collaborative Activities

### Collaborative (require team / client participation)

- Backlog refinement / grooming sessions
- Sprint planning
- Daily standups (passive listening, active when blocked)
- Sprint demo
- Sprint retrospective
- Elicitation sessions (interviews, workshops)
- Story / refinement meetings
- Dev syncs (when scope clarification needed)
- Client status calls
- Kickoff and approval meetings

### Non-Collaborative (BA solo work)

- Requirements specification and modeling
- Acceptance criteria writing (Gherkin / EARS)
- INVEST validation and story decomposition
- Subtask generation
- Meeting notes processing
- Documentation in Notion (BRD, specs, glossary, concept model)
- Requirements traceability maintenance
- Gap analysis and consistency checks
- Data analysis and reverse-engineering
- Prototyping (low-fi mockups, flow diagrams)

---

## 5. Deliverables

| Deliverable | Where | When | Owner |
|---|---|---|---|
| User stories + AC | Asana (tasks) | Per sprint | BA |
| Feature specifications | Notion | Before refinement | BA |
| BRD / SRS | Notion | Per phase or epic | BA |
| Glossary | Notion | Continuous | BA |
| Process models (BPMN, flows) | Miro | As needed | BA |
| Data dictionary | Notion | When data-heavy work starts | BA |
| Meeting notes | Notion | Within 24h of meeting | BA |
| Change requests | Asana (CR label) + Notion | When changes occur | BA |
| Open questions log | Notion | Continuous | BA |
| Risks & Assumptions log | Notion | Continuous | BA |

---

## 6. BA Allocation

| BA Project Role | Name | Position | Allocation |
|---|---|---|---|
| Lead BA | [name] | [position] | [e.g. 1.0 FTE] |
| Supporting BA | [name or N/A] | [position] | [e.g. 0.5 FTE] |

**Total BA allocation:** [sum]
**Recommended for project complexity:** [TBD — based on WBS estimate]

If allocation is below recommended, flag risks in section 7.

---

## 7. Mutual Commitments

This section is the kickoff alignment artifact. Walk through it with team and client. Get explicit sign-off.

### 7.1 What BA WILL do

- Own requirements elicitation, specification, refinement, traceability
- Facilitate refinement and clarification sessions
- Maintain documentation in Notion as single source of truth
- Generate user stories with AC ready for development per DoR
- Process change requests per `governance.md`
- Provide weekly BA progress visibility to PM
- Maintain glossary, open questions log, risks & assumptions log

### 7.2 What BA WILL NOT do

- Project management tasks (timeline, capacity, reporting) — owned by PM
- Product ownership (final prioritization decisions, business value calls) — owned by PO / Client
- QA test execution — owned by QA team
- Technical architecture decisions — owned by Tech Lead
- Note-taking-only role without analysis — BA is not a scribe
- Direct client communication on scope changes without PM alignment

### 7.3 What BA NEEDS from Client

- Single point of contact (PO or sponsor) with decision authority
- Response on open questions within [agreed SLA — e.g. 2 business days]
- Availability for refinement / elicitation sessions per `comm_plan.md`
- Sign-off on requirements before development per `governance.md`
- Access to: domain SMEs, existing documentation, current systems (read-only)

### 7.4 What BA NEEDS from Team

- Tech feasibility input during refinement
- Estimation honesty — flag if a story is too large or unclear
- Direct communication through BA on scope questions (no side channels with client)
- QA collaboration on AC and edge cases
- Tech Lead input on NFRs and constraints

### 7.5 Acceptance of this section

| Stakeholder | Role | Sign-off | Date |
|---|---|---|---|
| [name] | PM | Pending | — |
| [name] | PO / Client | Pending | — |
| [name] | Tech Lead | Pending | — |
| [name] | QA Lead | Pending | — |

---

## 8. Performance Expectations

BA performance on this project will be assessed against:

- **Requirements quality:** % of stories meeting DoR before sprint planning (target: ≥ 90%)
- **Refinement readiness:** % of stories ready for grooming on schedule (target: ≥ 80%)
- **Change responsiveness:** time from CR submission to impact analysis (target: ≤ 2 business days)
- **Documentation freshness:** Notion specs updated within 24h of any decision
- **Client satisfaction:** assessed at end of each release / phase
- **Team feedback:** assessed at sprint retrospectives

Full performance assessment process: TBD — link to `ba_performance_assessment.md` if generated.

---

## 9. Related Documents

- `01_project_context.md` — project context
- `02_stakeholders.md` — stakeholder map and RACI
- `governance.md` — prioritization, approvals, change control
- `comm_plan.md` — communication tools and meetings
- `rma.md` — requirements management approach
- `00_quick_standards.md` — story format, AC, DoR, DoD
```

---

## Behavior rules

- Service Menu (sections 7.1 and 7.2) is the most important block — never skip, never make generic. Customize per project context
- If `02_stakeholders.md` exists, populate sign-off table with real names. Otherwise use roles + TBD
- If methodology is unknown, generate Scrum version and flag as default
- Do not invent allocation numbers — use TBD if not provided
- Do not invent SLAs for client response — use [TBD — to be agreed] placeholder
- Special Tasks table (3.2) must be customized per project — flag if BA needs to provide project-specific tasks
- Performance metrics (section 8) are defaults — flag for adjustment per project

## Rules

- Anti-hallucination: stakeholder names, allocation, SLAs, project-specific tasks — TBD if not provided
- Service Menu must reflect actual project reality — if BA is genuinely doing PM work on this project (common in small teams), include it transparently rather than denying it
- "What BA WILL NOT do" section is critical — push back if BA tries to skip it
- Mutual Commitments section must be presented at kickoff — generate the agenda block in Step 4 to support this

## MCP Path

Input: Notion MCP → read `01_project_context.md`, `02_stakeholders.md`, `governance.md`, `comm_plan.md` pages
Output: Notion MCP → `create_page` in target Notion space
Secondary: Asana MCP → create kickoff task with link to Notion page and Mutual Commitments agenda

## MCP Unavailable Fallback

If MCP is not accessible:
1. Ask BA to paste content of related documents directly
2. Generate document as markdown in chat
3. BA publishes manually to Notion
4. Note: "MCP unavailable — manual publishing required"
