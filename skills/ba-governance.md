---
name: ba-governance
description: "Generate `governance.md` for BA governance on a delivery project using `01_project_context.md`, team structure, tools, approval expectations, and known change-control agreements. Use during project setup or onboarding to define prioritization approach, approvals approach, change control process, and default DoR/DoD. Generate section by section, ask targeted questions only when needed for the current section, and clearly flag all defaults for team alignment."
---

# BA Governance Approach

Generate `governance.md` as the BA governance baseline for how requirements are prioritized, approved, changed, and considered ready or done on the project.

## Input

Accept any of the following:
- `01_project_context.md`
- Team structure and roles
- Known tools such as Jira, Confluence, Slack
- Existing agreements on prioritization, approvals, or change control
- Existing DoR or DoD if already defined

If a governance rule is not known yet, generate a sensible default and clearly flag it for validation.

## Procedure

Generate the document section by section. Do not ask all clarification questions upfront.

### 1. Prioritization Approach

Extract or ask only what is needed for this section:
- which tools are used for prioritization
- who participates in prioritization decisions
- what criteria drive prioritization
- when prioritization happens

### 2. Approvals Approach

Extract or ask only what is needed for this section:
- who approves requirements
- what approved means in practice
- whether there are intermediate checks such as feasibility review or peer review

### 3. Change Control Process

Extract or ask only what is needed for this section:
- how formal the process is
- how changes to already-developed functionality are handled
- what happens when a change appears during a sprint

### 4. Assemble the full document

Use the fixed intro and the section structure below.

### 5. Add default DoR and DoD

Generate defaults appropriate for a Scrum delivery project, then explicitly flag that they require alignment with dev and QA.

## Output template

```markdown
# BA Governance Approach — [Project Name]
_Last updated: [date]_

## Intro

The BA governance approach identifies the stakeholders who have responsibility
and authority to make decisions about business analysis work, including who sets
priorities and who approves changes to business analysis information. It also
defines the process for managing requirements and design changes across the project.

BA governance approach consists of:
- Prioritization approach
- Approvals approach
- Change control process

---

## 1. Prioritization Approach

**Formality:** [e.g. Medium — priorities defined in Jira backlog; changes require stakeholder alignment]

**Stakeholders & roles:**
| Stakeholder | Role in prioritization |
|---|---|
| [Product Owner / Client] | Defines business priorities, final decision |
| [BA] | Facilitates, flags dependencies and risks |
| [PM] | Ensures timeline constraints are reflected |

**Prioritization criteria** (in order of importance):
1. Business value / client impact
2. Dependencies and technical risk
3. Time sensitivity / contractual deadlines
4. Effort and cost

**When prioritization happens:**
- During backlog refinement (weekly / bi-weekly)
- Before sprint planning
- Ad hoc when a change request arrives

**Risks of skipping:**
Requirements may be estimated and developed out of order, reducing solution effectiveness and creating rework.

---

## 2. Approvals Approach

**Approval process:**
1. BA completes requirement specification and AC
2. [Feasibility check with dev team — optional]
3. Requirement shared with [approver] via [channel]
4. Approver confirms — Jira status changes to [status] + Slack message with link
5. If no response within [X days] — treated as approved (include only if explicitly agreed)

**Approved Requirements Checklist:**
- [ ] Requirements meet quality standards (atomic, unambiguous, testable)
- [ ] INVEST criteria passed (for user stories)
- [ ] Acceptance criteria defined
- [ ] Design linked (if applicable)
- [ ] Technical feasibility confirmed by dev team
- [ ] Stakeholder approval received and recorded

**Approvers:**
| Deliverable | Approver | Method |
|---|---|---|
| User stories + AC | [PO / Client stakeholder] | [Jira status / Slack] |
| NFRs | [Tech Lead + PO] | [Confluence comment] |
| Change requests | [PO] | [CR document + Slack] |

---

## 3. Change Control Process

**Formality:** [e.g. Medium — informal for small changes, formal CR for scope changes]

**Changes to already-developed functionality:**
Any change to completed and delivered features is treated as a new backlog item.
Follows the full BA process: elicitation → specification → approval → Jira ticket.
Linked to the original feature for traceability.

**Changes before grooming:**
Low risk. BA updates the specification and notifies the team. No formal CR needed.
Must be clear and estimated before the next grooming session.

**Changes after grooming, before sprint planning:**
Medium risk. Requires re-elicitation and re-estimation. BA notifies via Slack with change summary and ticket link. Team re-confirms readiness.

**Changes after sprint planning (during sprint):**
High risk. Not recommended. If unavoidable, treat as a separate change request with label `CR` in Jira. Sprint scope is not modified unless the change is a critical blocker.

---

## 4. Definition of Ready (DoR)

A story is ready for development when:

- [ ] User story written in agreed format
- [ ] Acceptance criteria defined (Gherkin or EARS)
- [ ] Design linked and finalized (if applicable)
- [ ] Dependencies identified and resolved
- [ ] Business value defined
- [ ] Effort estimated by dev team
- [ ] Story is small enough to complete in one sprint
- [ ] Non-functional requirements noted (if applicable)
- [ ] Client approval obtained

> ⚠ Align this list with dev + QA team before use. Add or remove items as agreed.

---

## 5. Definition of Done (DoD)

A story is done when:

- [ ] All acceptance criteria passed
- [ ] Code reviewed and merged
- [ ] QA tested (frontend + backend)
- [ ] No critical bugs open
- [ ] Deployed to [staging / production]
- [ ] Confluence page updated (if applicable)

> ⚠ Align this list with dev + QA team before use.
```

## Rules

- Do not invent approver names, use roles if names are unknown
- Do not define Jira statuses that were not mentioned, use `TBD` if needed
- Include passive approval only if it is explicitly agreed, otherwise present it as a recommendation or omit it
- Treat DoR and DoD as defaults unless project-specific versions are provided

## Output

Save as `governance.md`.
