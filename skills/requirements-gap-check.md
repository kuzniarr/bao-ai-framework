---
name: requirements-gap-check
description: "Run a cross-project scan of Confluence requirements and Jira tickets to find gaps, inconsistencies, duplicates, and orphaned items. Use before refinement, sprint planning, or milestone reviews."
---

# /requirements-gap-check

## What it does

Cross-project quality scan across all requirements sources — Confluence specification pages and Jira tickets. Identifies gaps, inconsistencies, duplicates, and orphaned items. Gives the BA a single health report of the requirements landscape before refinement, sprint planning, or a milestone review.

This is a project-level scan, not a single-story review. Use `/ac-validation` for per-story AC depth checks.

## Input

- Jira project key (required)
- Confluence space key or parent page URL (required)
- Optional: scope filter — epic name, feature, or sprint to narrow the scan

## Output

Requirements health report with four sections:
1. **Gaps** — missing scenarios, uncovered flows, stories without AC
2. **Inconsistencies** — contradictions between Confluence and Jira, or between stories
3. **Duplicates** — same requirement described in multiple places
4. **Orphans** — stories without epic link, requirements without Jira ticket, AC without story

## Process

**Step 1 — Pull data via MCP**

From Jira:
- All stories in scope (summary, description, AC, epic link, status)
- Stories missing: AC, epic link, description

From Confluence:
- Specification pages in scope (requirements list, AC blocks)
- Pages without linked Jira tickets

**Step 2 — Gap analysis**

For each epic or feature in scope:

- Are all major user flows covered by at least one story?
- Do stories cover: happy path + at least one alternative + error state?
- Are there functional areas mentioned in Confluence but no corresponding Jira stories?
- Are there stories in Jira not traceable to any Confluence requirement?

Flag each gap with: location + description + severity (High / Medium / Low)

**Step 3 — Inconsistency check**

Compare Confluence spec vs Jira ticket content:

- Story description in Jira contradicts requirement in Confluence
- AC in Jira differs from AC in Confluence spec page
- Scope described in Confluence not reflected in any Jira story

Flag each inconsistency with: source A vs source B + description

**Step 4 — Duplicate detection**

- Same requirement stated in two or more Confluence pages
- Two Jira stories covering the same functional scope
- AC items repeated across multiple stories

Flag with: list of duplicate items + recommended resolution (merge / remove / keep one as primary)

**Step 5 — Orphan detection**

| Orphan type | Check |
|---|---|
| Story without epic | Story exists in Jira, no Epic Link |
| Story without AC | Story is In Progress or higher, AC field empty |
| Story without Confluence link | Story has no linked spec page |
| Confluence page without Jira link | Spec page exists, no linked story |
| Requirement without owner | Requirement in Confluence has no assignee or BA |

**Step 6 — Deliver report**

```
## Requirements Health Report — [Project / Epic / Sprint]
Scan date: [date]
Sources: Jira [key] + Confluence [space/page]

### Summary
| Category | Count | High severity |
|---|---|---|
| Gaps | N | N |
| Inconsistencies | N | N |
| Duplicates | N | N |
| Orphans | N | — |

### Gaps
- [Epic / Feature]: [description of missing coverage] — Severity: High/Medium/Low

### Inconsistencies
- Confluence [page]: says [X] / Jira [ticket]: says [Y] — Action needed: [clarify / update Jira / update Confluence]

### Duplicates
- [Story A] and [Story B] cover the same scope — Recommended: [merge / remove B]

### Orphans
- [Story key]: no Epic Link
- [Story key]: AC missing, status In Progress
- [Confluence page]: no linked Jira story

### Recommended next steps
1. [Priority action]
2. [Priority action]
```

## Rules

- Do not modify Jira or Confluence during the scan — read only
- Do not infer requirements that are not explicitly stated in source documents
- If MCP returns partial data — flag it explicitly, do not fill gaps with assumptions
- Severity assignment: High = blocks development or causes rework risk / Medium = should fix before sprint / Low = cleanup, not urgent
- Anti-hallucination: if a requirement is ambiguous or unclear — flag as TBD, do not interpret

## MCP Path

→ Fetch Jira issues: `jira_get_project_issues` or `jira_search` with JQL scoped to epic/sprint
→ Fetch Confluence pages: search by space key or parent page
→ Cross-reference by: Confluence page links in Jira, Jira links in Confluence

## MCP Unavailable Fallback

If MCP is not accessible:
1. Ask BA to export Jira issue list (CSV or pasted) and paste Confluence requirements
2. Run analysis from provided content
3. Note in report: "Manual input — MCP not used. Verify against live data."
