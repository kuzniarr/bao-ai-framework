---
name: jira-split
description: "Take one epic from WBS split to Jira ticket structure. Two phases in one skill. Phase A (mapping): when the WBS Jira column is empty for the epic, read the epic's MVP rows (Future excluded) and the decomposed stories from the Notion MD, group children under each WBS parent (1→N, dash format), and hand the BA a paste-ready mapping plus gap/orphan flags — then stop for the BA to paste into the WBS. Phase B (execution): when the Jira column is filled, read it via Google Drive MCP, pull the epic's existing tickets from Jira (Epic Link), and go parent-by-parent — 1:1 renames the existing ticket, an N-split keeps the first child on the parent ticket and creates the rest with inherited fields, and every ticket gets the fixed 4-subtask set with role-based assignees. Returns a link to the main ticket(s) only after each parent, waits for the BA's 'ok, next'. Does NOT write story/AC into descriptions (that is jira-fill) and does NOT write back to the WBS. Trigger: 'split [epic] to jira', 'засплітай епік в джиру', 'map and split [epic]'."
---

# /jira-split

## Purpose

Turn one epic's WBS split into the matching Jira ticket structure. One skill, two phases:
Phase A — Mapping. When the epic's WBS Jira column is empty, propose the WBS→stories mapping (titles only, MVP-only) for the BA to review and paste into the WBS.
Phase B — Execution. When the Jira column is filled, read it and build the ticket structure in Jira: rename/create Story tickets per the split, and create the fixed subtask set (with role-based assignees) under each.
The skill auto-detects which phase to run from the state of the WBS Jira column. The BA always pastes the mapping into the WBS manually between phases — Drive MCP reads the sheet but cannot write cells.
This skill is structure only. It never writes the requirement (story + AC) into a ticket — that is jira-fill.

## When to invoke

After decompose-wbs-epic and ears-ac, when moving an epic's stories into Jira.
One epic per invocation.

## Inputs

Epic name — which epic to process (matches an ALL-CAPS topic header in the WBS).
WBS — Google Sheets link. Read live via Google Drive MCP (read_file_content on the file ID from the link). The relevant sheet is WBS & Development Efforts; columns: Topic (A), User Story (B), Acceptance Criteria (C), Jira (D), Phase (I).
Notion MD — the epic's decomposed stories (story + AC), pasted by the BA. Used in Phase A to name the children; the same MD later feeds jira-fill.
Context read automatically: 01_project_context.md (Jira project key, conventions).

## Epic boundary

An epic = the rows from its ALL-CAPS topic header (e.g. GROUP MANAGEMENT) down to the next ALL-CAPS header (e.g. USER MANAGEMENT). The skill processes only those rows and never reaches into neighbouring epics. Default to header-to-header unless the BA specifies otherwise.

## Phase detection

Read column D for the epic's rows via MCP:
D empty for the epic → run Phase A.
D filled (as in Group Management) → run Phase B directly.

## Phase A — Mapping (D empty)

Read the epic's WBS section. Keep MVP rows only — any row flagged Future in the Phase column is never mapped.
Read the decomposed stories from the Notion MD.
For each MVP WBS parent row, group the child stories that belong to it (1→N). Children are titles only, dash-prefixed, one per line — the format the BA uses in column D.
Surface for BA review, do not silently resolve:
gap — an MVP WBS row with no child story.
orphan — a child story in the MD with no WBS parent.
Output the mapping as paste-ready text for column D, plus the flag list. Stop. The BA reviews, pastes into the WBS, then re-invokes (or says "go") to run Phase B.
Phase A reads the WBS and the MD only. It touches neither Jira nor the WBS file.

## Phase B — Execution (D filled)

Read column D for the epic via MCP → for each WBS parent row, the ordered list of target ticket names (split on blank lines).
Pull the epic's existing tickets from Jira via MCP (read-only, no approval): Epic Link = [epic name], label WBS, type Story. This gives the parent ticket per WBS row and the fields to inherit.
Go parent-by-parent, iteratively. For each WBS parent row:
1 name (1:1) — rename the existing parent ticket's title to that name (only if it differs). When a ticket is renamed, its 4 existing subtasks are renamed too to follow the new title (see "Subtask rename sync" below).
N names (split) — the existing parent ticket becomes name 1 (rename, with subtask sync); create new Story tickets for names 2…N, inheriting Epic Link, Labels, Fix Version, Sprint from the parent ticket. Inherited field unavailable → [TBD], never invented.
Under every resulting ticket (renamed parent and new ones), ensure the fixed 4-subtask set (below): create it on new tickets; on a renamed existing ticket, rename the existing 4 subtasks to match the new title. Set each subtask's assignee per the Assignee mapping (created subtasks always; renamed existing subtasks only if currently Unassigned).
Show the BA a link to the main ticket(s) only for this parent — not the subtasks. The BA opens it, sees the title and its BE/FE/QA children, and says "ok, next".
On "ok, next" → proceed to the next WBS parent. (Approval is per-parent here, by design — the BA verifies each before moving on.)
When all parents are done, give the full list of main tickets as Story name → Jira link.
Phase B does not write or clear ticket descriptions and does not add comments — descriptions stay as they are until jira-fill runs.

## Subtask set (fixed, under every ticket)

Four subtasks per Story ticket, all built from the full ticket title:

| # | Subtask name | Assignee |
|---|---|---|
| 1 | [BE] {title} | BE (see Assignee mapping) |
| 2 | [FE] {title} | FE (see Assignee mapping) |
| 3 | [QA] {title} BE | QA (see Assignee mapping) |
| 4 | [QA] {title} FE | QA (see Assignee mapping) |

The parent Story stays Unassigned — only the four subtasks get assignees, per the Assignee mapping below. No clones links between split siblings. (Existing tickets in Jira may show short BE/FE subtask names — that is a manual inconsistency, not the rule; the skill always uses the full title for all four.)

## Subtask rename sync

The subtask name always derives from its parent ticket's title. So whenever a ticket is renamed (a 1:1 rename, or the first child of a split taking a new name), the skill renames that ticket's existing 4 subtasks to match the new title — [BE] {new title}, [FE] {new title}, [QA] {new title} BE, [QA] {new title} FE. It does not create a second set; it renames the existing one. New tickets created for a split get a freshly created 4-subtask set from their own title. Subtasks never keep a stale title after their parent is renamed.

## Assignee mapping

Each subtask is assigned by role. Default mapping for the CID project:

| Role | Subtask(s) | Assignee (username) |
|---|---|---|
| BE | [BE] {title} | v.slobodian |
| FE | [FE] {title} | v.senko |
| QA | [QA] {title} BE + [QA] {title} FE | v.prokopchuk |

Rules:
Usernames are Jira Server/DC usernames (e.g. v.slobodian), not emails or accountIds — this is what the Jira DC assignee field expects.
This mapping is the CID default. The BA may override it at split time (different team/epic) by stating the role→username set for that run; the skill uses the override for that run only.
Assignee is set on newly created subtasks. On a pre-existing subtask being renamed, set the assignee per the mapping only if it is currently Unassigned — never overwrite an assignee a human already set.
If setting an assignee fails (unknown/inactive username, permissions), report it to the BA and leave that subtask Unassigned — never substitute another username to make it pass.

## Output

Phase A: paste-ready column-D mapping + gap/orphan flags. No Jira or WBS writes.
Phase B: the epic's ticket structure built in Jira (parents renamed, split siblings created, 4 subtasks each with role-based assignees), with main-ticket links reported per parent and a final summary list.

## MCP unavailable fallback

Phase A: still works if the BA pastes the WBS section and MD as text — output the mapping the same way.
Phase B: if Jira MCP is down, print the structure plan (per parent: target ticket names, action, inherited fields, subtask names + intended assignees) for the BA to apply manually.

## Behavior parameters

Temperature low — deterministic structural transformation, not generation.
Read before write; approval before write. Read-only MCP (Drive read, Jira fetch) runs without approval.
Anti-hallucination: never invent ticket names (read from D), child story names (read from MD), field values (inherit or [TBD]), assignees (from the Assignee mapping only — never guess a username), or clones links (none).

## What this skill does NOT do

Does not decide the split — the BA records it in WBS column D; Phase A only proposes, Phase B only reads.
Does not write story/AC into descriptions — that is jira-fill.
Does not add traceability comments — that is jira-fill.
Does not set clones links. (It does set subtask assignees — see Assignee mapping.)
Does not write back to the WBS (Drive MCP reads the sheet; the BA pastes the mapping in by hand).
Does not map or split Future rows.

## Relation to other skills

Consumes the decomposed stories from decompose-wbs-epic (via the Notion MD) and runs after ears-ac.
Runs before jira-fill (Split = structure, Fill = content).
