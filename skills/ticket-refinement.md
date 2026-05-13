# Skill: /ticket-refinement

## Purpose

Refine a Jira ticket imported from WBS (or created earlier without full refinement) into a ready-for-development state. Clarifies story format, generates or completes Acceptance Criteria, fills missing fields against Definition of Ready, flags ambiguities and open questions. Updates the Jira ticket directly via MCP.

## When to use

- Phase 5 · Jira & Traceability, after WBS import.
- Before sprint planning — to bring imported tickets to DoR.
- Before refinement session — to prepare tickets for team discussion.
- On any ticket that has a summary but lacks story format, AC, or DoR-required fields.

## Inputs

- Jira ticket key (skill reads via MCP)
- Project standards from `00_quick_standards.md` (Project Knowledge) — story format, AC format, DoR rules
- `01_project_context.md` (for glossary and scope context)
- Optional: linked Confluence page if it exists for the ticket

## Output

Updated Jira ticket. Specifically:
1. **Story field** — rewritten if not in agreed format (per `/quick-standards`)
2. **Acceptance Criteria** — added or completed per project AC format (Gherkin or EARS)
3. **DoR check** — missing required fields flagged in ticket description (e.g. `[DoR-MISSING] Designs link`)
4. **Open questions** — added as Jira comment, not in the description (so they're trackable separately)

Plus a chat summary:
- What was refined
- What was flagged as missing
- What requires BA / PM / client follow-up

## Behavior rules

1. **Read before writing.** Always read the current ticket state via MCP first. Compare against `00_quick_standards.md`.
2. **Do not overwrite manually-curated content.** If the ticket already has a story and AC — validate against standards. Suggest edits in chat output, do not silently rewrite.
3. **Apply project AC format only.** If standards say Gherkin — write Gherkin. Do not mix formats. Do not write both.
4. **Flag, don't invent.** If story scope is unclear from the WBS-imported summary — write the story with placeholders and flag `[TBD]`. Do not invent acceptance criteria for flows the WBS doesn't mention.
5. **DoR is a checklist, not a rewrite.** If `governance.md` requires Designs link, NFR link, dependency listed — check each. Missing items go to ticket description as `[DoR-MISSING]` markers, not as invented content.
6. **One ticket per invocation.** This skill is not bulk refinement. For batch — wrap in a separate flow (`/ticket-refinement-batch` future).
7. **Confirm before write.** Skill presents the proposed updates in chat first, BA confirms, then MCP writes to Jira. No silent updates.

## Example invocation

```
/ticket-refinement
Ticket: PROJ-142
```

Skill behavior:
1. Reads PROJ-142 via MCP.
2. Reads `00_quick_standards.md` and `01_project_context.md`.
3. Compares: story format, AC presence, AC format, DoR fields.
4. Presents proposed updates in chat — diff style: current → proposed.
5. BA confirms or adjusts.
6. Skill writes updates to PROJ-142 via MCP.
7. Posts open questions as a Jira comment.
8. Returns chat summary.

## Anti-hallucination

If unknown — write `[TBD]` in the ticket and add to the open questions comment:
- Specific user role not clear from WBS summary
- Acceptance criteria for flows not mentioned in any input
- Field values (API endpoints, error codes, threshold numbers) not in tech context or linked Confluence
- Stakeholder names not in `02_stakeholders.md`

Never invent acceptance scenarios for edge cases that were not in the WBS, Confluence, or any other source.

## Relation to other skills

- Reads outputs of: `/quick-standards`, `/project-charter`, `/decompose-epic` (if story was decomposed before)
- Often paired with: `/ac-validation` (validate AC after refinement), `/invest-check` (verify INVEST compliance after rewrite)
- Different from `/push-to-jira`: that creates new tickets. This one updates existing ones.

## Bulk processing pattern

For refining many imported tickets (e.g. 170 stories from WBS):
- Do NOT run skill 170 times in one chat — context degrades.
- Pattern: one epic at a time = one chat. Within a chat, refine 5-10 tickets sequentially.
- After each epic — start a new chat with handoff prompt (see Best Practices Tip 2 in Guide).
- After all tickets in an epic are refined — run `/ac-validation` and `/invest-check` on the epic scope.
