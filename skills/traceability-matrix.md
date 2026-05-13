---
name: traceability-matrix
description: "Build a traceability and progress matrix from Jira data in xlsx format to show epic, story, subtask, status, and progress coverage. Use for project progress snapshots, coverage reviews, and sprint reporting."
---

# Traceability Matrix

Builds a traceability and progress matrix in xlsx format. Gives full coverage visibility across the project, what is defined, what is in Jira, and what the current delivery status is.

Reads Jira directly via Atlassian MCP to pull ticket data, statuses, and progress. Generates an xlsx file ready for sharing with PM, tech lead, or client.

---

## When to Use

- At any point when you need a cross-cutting view of project progress
- Before a status call or sprint review to prepare a progress snapshot
- When the client asks for coverage visibility
- At the end of a sprint to track what's done vs in progress vs not started

---

## Prerequisites

- Atlassian MCP connector active in this chat
- Jira project key known (from `01_project_context.md`)
- Scope of epics to include, either full project or specific epics

---

## Input

```
/traceability-matrix
Jira project: [project key, e.g. EWA]
Scope: [all epics / specific epics list]
```

If project key is missing, ask before pulling data:

> "Which Jira project key should I use? And should I include all epics or specific ones?"

---

## Steps

**Step 1, Pull data from Jira via MCP**

Query Jira for the following, scoped to the provided project key:

1. All Epics in the project (or the specified subset)
2. For each Epic, all Stories linked to it
3. For each Story, all Subtasks
4. For each ticket, current Status and Assignee

If MCP returns an error or no data:

> "Jira MCP returned no data for project [key]. Check that the connector is active and the project key is correct. Do not proceed with generated placeholder data."

**Step 2, Calculate progress per level**

For each Story:
- Count total subtasks
- Count subtasks with status = Done (or equivalent closed status)
- Progress % = Done subtasks / Total subtasks × 100
- If no subtasks, derive progress from Story status:
 - To Do → 0%
 - In Progress → 50%
 - Done → 100%

For each Epic:
- Progress % = average of all child Story progress values

**Step 3, Confirm scope before generating**

Show a summary before generating the file:

```
Ready to build traceability matrix for project [key].

Epics found: [N]
Stories found: [N]
Subtasks found: [N]

Continue?
```

Wait for confirmation.

**Step 4, Generate xlsx**

Use openpyxl. One sheet named `Traceability Matrix`.

```python
import openpyxl
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment
from openpyxl.utils import get_column_letter

wb = Workbook()
ws = wb.active
ws.title = "Traceability Matrix"

# Header row
headers = [
 "Epic", "Epic Key", "Feature / Story", "Story Key",
 "Subtask", "Subtask Key", "Assignee", "Status", "Progress %"
]

# Style header row: bold, light blue background
header_fill = PatternFill("solid", fgColor="BDD7EE")
for col, header in enumerate(headers, 1):
 cell = ws.cell(row=1, column=col, value=header)
 cell.font = Font(bold=True)
 cell.fill = header_fill
 cell.alignment = Alignment(horizontal="center")

# Fill data rows
# One row per subtask. If story has no subtasks, one row for the story.
# Epic and Story cells are merged vertically across their child rows.
# Status color coding:
# Done → light green (C6EFCE)
# In Progress → light yellow (FFEB9C)
# To Do / Blocked → light red (FFC7CE)

row = 2
for epic in epics:
 for story in epic.stories:
 items = story.subtasks if story.subtasks else [None]
 for subtask in items:
 ws.cell(row=row, column=1, value=epic.name)
 ws.cell(row=row, column=2, value=epic.key)
 ws.cell(row=row, column=3, value=story.name)
 ws.cell(row=row, column=4, value=story.key)
 ws.cell(row=row, column=5, value=subtask.name if subtask else "—")
 ws.cell(row=row, column=6, value=subtask.key if subtask else "—")
 ws.cell(row=row, column=7, value=subtask.assignee if subtask else story.assignee)
 status = subtask.status if subtask else story.status
 ws.cell(row=row, column=8, value=status)
 ws.cell(row=row, column=9, value=story.progress_pct)
 row += 1

# Auto-fit column widths
for col in range(1, len(headers) + 1):
 ws.column_dimensions[get_column_letter(col)].width = 25

wb.save('/mnt/user-data/outputs/traceability-matrix.xlsx')
```

**Color coding for Status column:**

| Status | Fill color |
|---|---|
| Done / Closed / Released | Light green, `C6EFCE` |
| In Progress / In Review | Light yellow, `FFEB9C` |
| To Do / Open / Blocked | Light red, `FFC7CE` |

Map the actual Jira status names from the project to one of these three buckets. If a status is unknown, use no fill and flag it in the summary.

**Step 5, Deliver file and summary**

After generating, call `present_files` with the xlsx path.

Add a short summary:

```
✅ Traceability matrix generated.

Project: [key]
Epics: [N]
Stories: [N] ([X] Done, [Y] In Progress, [Z] To Do)
Overall progress: [avg %]

⚠ Unknown statuses (not mapped): [list if any]
```

---

## MCP Fallback

If Atlassian MCP is not available:

1. Do not generate a matrix with invented ticket data
2. Tell the BA:

> "Jira MCP is not accessible. I can't pull live ticket data. You can either:
> a) Paste the ticket list here (Epic / Story / Subtask / Status) and I'll build the matrix from that
> b) Check the MCP connector and re-run"

If the BA pastes data manually, proceed from Step 2 using that data.

---

## Anti-hallucination Rules

- Do not generate ticket keys, story names, or statuses that were not returned by Jira MCP
- Do not infer progress from ticket names or descriptions, use only status fields
- If MCP returns partial data (some epics empty), flag it explicitly, do not fill gaps
- Never map an unknown status to Done, default to To Do bucket and flag it
