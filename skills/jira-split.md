---
name: jira-split
description: "Build the Jira ticket structure for one epic from the split recorded in the WBS Jira column. For each WBS story, a 1:1 entry keeps its existing parent ticket, while an N-way split renames the parent ticket to the first name and creates the remaining tickets with inherited fields. Create the fixed 4-subtask set under every ticket. Use when the BA says 'розбий епік на тікети', 'засплітай в джиру', or 'jira split for the epic' after the split is filled into the WBS Jira column."
---

# /jira-split

## Purpose
Turn the split the BA has already recorded in the WBS Jira column into the matching Jira ticket structure for one epic. Renames/creates Story tickets and creates the fixed subtask set under each. This skill is structure only — it never writes the requirement (story + AC) into a ticket and never touches comments. Filling content is jira-fill.

## When to invoke
After the BA has finished refining the epic's stories and has filled the WBS Jira column (C4) with the split — one ticket name per line, names separated by a blank line (\n\n) when a WBS story splits into several.
Before jira-fill — Split lays down empty tickets, Fill fills them.
One epic per invocation. Not bulk across epics.

## Prerequisites
The WBS Jira column (C4) is filled for the epic's rows.
For new epics: exactly one parent Story ticket exists in Jira per WBS story (the WBS-imported ticket).
Jira MCP connector active (or fallback to a printed plan).

## Inputs
Epic name (the BA names which epic to process).
WBS — WBS_scope_only_copy.xlsx, sheet WBS & Development Efforts. Columns: Topic (C1), User Story (C2), Jira (C4), Phase (C9). The split lives in C4.
Context read automatically: 01_project_context.md (Jira project key, conventions).

## The split is read, never decided
The BA decides the split and records it in C4. This skill reads C4 and never proposes, infers, or tops up a split. If C4 is empty for a row, that row is skipped and flagged — Split does not guess.
Parsing rule for each WBS row's C4 cell:
Split the cell text on blank lines (\n\n) → an ordered list of ticket names.
1 name → 1:1: keep the existing parent ticket, rename its title to that name if it differs.
N names → split: the existing parent ticket becomes the first name (rename); create N − 1 new tickets for names 2…N.

## Process
Read the WBS rows belonging to the named epic. Pull C2 (WBS story), C4 (split), C9 (phase) for each.
Read the existing Jira tickets for the epic via MCP (read-only, no approval needed) — to know the parent ticket per WBS story and the fields to inherit.
Build the structure plan and show it in chat (table): per WBS story → 1:1 or split → for each target ticket, the name, the action (rename existing / create new), and the source ticket whose fields it inherits.
Wait for explicit "ok" before any write.
Execute via MCP:
1:1 — rename the parent ticket title to the C4 name (only if it differs).
Split — rename the parent ticket title to name 1; create new Story tickets for names 2…N. New tickets inherit Epic Link, Labels, Fix Version, Sprint from the parent ticket of that same WBS story. Inherited field unavailable → [TBD], never invented.
Under every target ticket (renamed parent and new ones), create the fixed subtask set (below).
Report in chat: each ticket as Story name → Jira link.
Split does not write or clear ticket descriptions and does not add comments. Descriptions stay as they are (the parent still shows its original WBS story) until jira-fill runs.

## Subtask set (fixed, under every ticket)
Exactly four subtasks per Story ticket. Name = [ROLE] {parent ticket title}:
#
Subtask name
1
[BE] {parent}
2
[FE] {parent}
3
[QA] {parent} BE
4
[QA] {parent} FE
All Unassigned. No clones links between split siblings. The parent title is substituted verbatim.

## Output
The epic's Jira ticket structure: parents renamed, split siblings created, four subtasks under each.
Chat report: Story name → Jira link for every created/renamed ticket.
Ticket descriptions left untouched (awaiting jira-fill).

## MCP unavailable fallback
Print the structure plan as text: per WBS story → target ticket names → action (rename/create) → inherited fields → subtask names. The BA executes in Jira manually.

## Behavior parameters
Temperature low — deterministic structural transformation, not generation.
Read before write; approval before write. Read-only MCP (fetch/search/list) runs without approval.
Anti-hallucination: never invent ticket names (read from C4), field values (inherit or [TBD]), assignees (none), or clones links (none).

## What this skill does NOT do
Does not decide the split — reads it from WBS C4.
Does not write story/AC into descriptions — that is jira-fill.
Does not add traceability comments — that is jira-fill.
Does not set assignees.
Does not create clones links between split siblings.
Does not write back to the WBS.
Does not match tickets to Notion — Split never touches Notion.

## Relation to other skills
Runs before jira-fill (Split = structure, Fill = content).
Reads the split that the BA recorded after decompose-wbs-epic / story-map-per-epic thinking.
