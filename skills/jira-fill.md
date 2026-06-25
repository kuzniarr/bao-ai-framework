---
name: jira-fill
description: "Write a finalized requirement (story plus AC) into a Jira ticket. Input is a CID ticket key plus the requirement as markdown. If the ticket description is non-empty, move the old title and description to a comment for traceability first, then write the new requirement; if empty, write it directly. Use when the BA says 'залий вимогу в тікет', 'онови CID-XXX', or 'fill the ticket'."
---

# /jira-fill

## Purpose
Write one finalized requirement (story + Acceptance Criteria) into one Jira ticket. The same flow serves two cases: first-fill of an empty ticket created by jira-split, and update of a ticket that already holds a requirement. Pure transfer of what the BA provides — no validation, no DoR check, no flagging, no rewriting.

## When to invoke
After jira-split has created the empty ticket structure and the requirement is finalized — to fill a ticket.
When a requirement already in Jira changed (refined in Notion or decided in chat) — to update the ticket.
One ticket per invocation.

## Prerequisites
The requirement (story + AC) is final.
The CID ticket key is known (the BA provides it — the WBS↔CID link is not recorded anywhere, so it is never guessed).
Jira MCP connector active (or fallback to printed content).

## Inputs
CID ticket key — provided by the BA (e.g. CID-594).
Requirement as markdown — story + AC, pasted into chat by the BA. Notion is exported to MD; this skill does not read Notion directly and does not match by name.

## Process
Read the ticket's current state via MCP (read-only, no approval needed): current title and description.
Traceability rule:
Description non-empty → move the current title + description into a comment on the ticket (traceability), then continue.
Description empty → skip the comment (a freshly split ticket has nothing prior to preserve).
Write the requirement into the ticket: title and description from the provided markdown. Story + AC go into the description as given.
Report in chat: Story name → Jira link.
That is the whole skill. No DoR check, no [DoR-MISSING] flags, no field validation, no added structure — only what the markdown contains.

## Why traceability lives here, not in jira-split
jira-split deliberately does not touch comments or descriptions. If both skills wrote the traceability comment, the previously-existing parent tickets would get it twice. Keeping it solely in Fill means: the comment is written exactly once, at the moment the old content is about to be overwritten, by the skill that overwrites it.

## Output
The ticket's title and description set to the provided requirement.
A traceability comment holding the prior content (only if the description had content before).
Chat report: Story name → Jira link.

## MCP unavailable fallback
Print the formatted ticket content: title, description (story + AC inline), and — if the prior description was non-empty — the prior content to be pasted as a comment first. The BA applies it in Jira manually.

## Behavior parameters
Temperature low — deterministic transfer, not generation.
Read before write; approval before write. Read-only MCP (fetch) runs without approval.
Anti-hallucination: write only what the provided markdown contains. Do not add AC, fields, labels, or assignees not present in the input.

## What this skill does NOT do
Does not check DoR or flag missing fields.
Does not write or rewrite AC — the AC is provided final.
Does not read Notion directly — input is markdown in chat.
Does not decide which ticket — the CID is provided.
Does not touch subtasks — they already exist from jira-split.
Does not create or split tickets — that is jira-split.

## Relation to other skills
Runs after jira-split (Split = structure, Fill = content).
Consumes the finalized story + AC the BA refined (post ears-ac / gherkin-ac and team refinement).
