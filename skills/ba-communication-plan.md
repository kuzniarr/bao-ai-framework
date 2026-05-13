---
name: ba-communication-plan
description: "Generate `comm_plan.md` for BA communication on a delivery project using `01_project_context.md`, kickoff notes, PM messages, stakeholder context, and known meeting cadence. Use during project setup or onboarding to define BA-relevant communication tools, meetings, and stakeholder contact matrix. If no cadence is defined, generate a default Scrum meeting set, clearly flag it as default, and ask one follow-up about adjustments or client-specific communication constraints."
---

# BA Communication Plan

Generate `comm_plan.md` as the BA-specific communication plan for the project. Cover only communication activities where the BA is directly involved.

## Inputs

- `01_project_context.md` (Project Knowledge)
- `02_stakeholders.md` (Project Knowledge)
- Scope & Vision document (team/delivery section, if available)
- Kickoff agreements with PM (cadence, channels, escalation)
- Client communication preferences (if stated)

## Source priority

1. Stakeholders file — for contact matrix
2. S&V team/delivery section — for cadence and ceremony list
3. PM agreements — for escalation path
4. Project Charter — for context

## Procedure

### 1. Extract context

Identify:
- communication tools in use
- team composition and stakeholder roles
- any existing meeting schedule
- time zone or working-hour constraints

### 2. Generate the document

Use the template below.
- If a schedule is provided, use it
- If a schedule is not provided, generate the default Scrum events and explicitly mark them as defaults

### 3. Ask one follow-up

After generating, ask:

> This uses the standard Scrum cadence as a default. Do you want to add, remove, or adjust any meetings? Also, are there any client-specific communication constraints I should reflect, such as time zone, preferred channels, or response SLA?

## Output template

```markdown
# BA Communication Plan — [Project Name]
_Last updated: [date]_

## Description

A BA communication plan defines the collaboration activities and their structure
for business analysis work on this project. It covers only BA-related communication,
not the full project communication plan.

This document is an input to stakeholder engagement planning and provides
visibility into BA availability and activities.

## Communication Tools

| Tool | Purpose & Usage |
|---|---|
| Slack | Short and frequent communication, files, meeting notes |
| Google Meet | Meetings, audio/video calls |
| Jira | Task tracking, requirements status, comments |
| Confluence | Documentation, specifications, decision log |
| [add or remove as relevant] | |

## Meetings

| Meeting | Mandatory / Optional | Audience | Purpose | Agenda | Tool | Frequency | Schedule | Duration | Responsible |
|---|---|---|---|---|---|---|---|---|---|
| Sprint Planning | Mandatory | PO, PM, Dev Team, BA | Plan the sprint, set goals and priorities | Define sprint scope and tasks | Google Meet | Bi-weekly | Sprint start | 1–2h | PM |
| Daily Scrum | Mandatory | Dev Team, PM, BA (optional) | Sync on progress, blockers, next steps | What was done, what's next, impediments | Google Meet | Daily | Each morning | 15 min | PM |
| Backlog Refinement | Optional | PO, BA, Dev Team, PM | Update, clarify, prioritize backlog items | Review and refine user stories | Google Meet | Weekly | Wednesday | 1h | BA / PO |
| Sprint Review | Mandatory | PO, Dev Team, PM, Stakeholders | Review completed work, demo to stakeholders | Demonstrate increment, collect feedback | Google Meet | Bi-weekly | Sprint end | 1h | PM |
| Sprint Retrospective | Mandatory | PM, Dev Team, BA, PO (optional) | Reflect and improve team processes | What went well, improvements | Google Meet | Bi-weekly | Sprint end | 1h | PM |
| BA–Client Sync | Optional | BA, PO / key client stakeholder | Resolve open BA questions, align on requirements | Open questions, upcoming stories | Google Meet | Weekly | TBD | 30–60 min | BA |

## Stakeholder Contact Matrix

| Name | Role | Slack / Email | Time Zone | Preferred Channel | Response SLA |
|---|---|---|---|---|---|
| [Name] | [Role] | [contact] | [GMT+x] | [Slack / Email / Call] | [e.g. 24h] |

## Notes

[Any project-specific communication constraints, escalation path, or agreements not captured in the tables above]
```

## Rules

- Do not invent meeting schedules, use what was provided or clearly mark defaults
- Do not add tools that were not mentioned in input or project context unless they are part of an explicitly flagged default structure
- Do not populate stakeholder contacts unless they are known
- Keep the plan BA-specific rather than turning it into a full project communication plan

## Output

Save as `comm_plan.md`.
