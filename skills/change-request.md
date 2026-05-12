# /change-request

## What it does

Processes a scope change from identification through client decision. Creates a structured Change Request document, registers it in Jira with correct labels and status, and prepares communication for PM and client. Follows the Geniusee BA Change Management Process (9-phase model).

Use when: a requirement falls outside the agreed scope, changes agreed logic or functionality, or a stakeholder requests something new after baseline is set.

## Input

- Description of the change (free text — what was requested, by whom, in which context)
- Jira project key (for ticket creation)
- Optional: link to the original scope document (WBS, Confluence spec) to reference in the CR

## Output

1. Structured Change Request document (impact analysis, scope delta, communication draft)
2. Jira CR ticket created with: Type: Story, Label: CR, Status: To Do
3. PM communication draft (Slack message)
4. Client communication draft (after estimation)

## Process

**Phase 1 — Change identification**

Extract from the input:
- What exactly is being requested
- How it differs from the agreed scope (reference original scope if provided)
- Preliminary business priority (High / Medium / Low) — ask BA if not clear

If the change is a clarification of existing scope (not a new requirement) — flag it and stop: "This appears to be a clarification, not a change. Confirm before proceeding."

**Phase 2 — Register in Jira**

Create a Jira ticket via MCP:

```
Type: Story
Summary: [CR] [brief description of change]
Label: CR
Status: To Do
Description:
  ## Change Request

  **Requested by:** [stakeholder]
  **Date identified:** [date]
  **Original scope reference:** [link or TBD]

  ## Change Description
  [What is being requested]

  ## Scope Delta
  [How this differs from agreed scope]

  ## Preliminary Business Priority
  High / Medium / Low

  ## Impact areas (to be completed in Phase 3)
  - Functionality: TBD
  - NFRs: TBD
  - Integrations: TBD
  - Risks: TBD
```

**Phase 3 — Impact analysis**

Once BA confirms to proceed, fill impact areas:

- **Functionality impact:** which existing features are affected
- **NFR impact:** performance, security, data — any new or changed NFRs
- **Integration impact:** APIs, third-party services, data flows affected
- **Risk:** delivery risk if implemented vs. if deferred

Update Jira ticket. Transition status: `To Do` → `In Analysis`

**Phase 4 — Prepare for estimation**

Confirm the CR description is sufficient for dev team to estimate:
- Scope is clear
- Acceptance criteria drafted (can be high-level at this stage)
- Dependencies identified

Transition status: `In Analysis` → `Ready for Estimation`

**Phase 5 — PM communication draft**

Generate a Slack message for PM:

```
#change_request
Jira: [ticket key + link]
Requested by: [stakeholder]
Change: [1-2 sentences]
Impact: [brief — what is affected]
Estimated effort: TBD (pending team review)
Next step: submit for team estimation at next refinement
```

**Phase 6 — Client communication draft** (after estimation received)

Generate client-facing message for PM to send:

```
Hi [client name],

We've analyzed the change request regarding [topic].

Summary of the change:
[1-2 sentences]

Impact:
- Timeline: [X days / TBD]
- Budget: [impact / TBD]
- Dependencies: [list or none]

Alternatives considered:
- [Option A: implement now]
- [Option B: defer to Phase 2]
- [Option C: simplify scope]

Please confirm your decision:
□ Approve
□ Reject
□ Defer

Note: implementation begins only after written approval.
```

**Phase 7 — Update Jira based on client decision**

| Decision | Jira action |
|---|---|
| Approved | Add label `approved` → transition to `Ready for Development` |
| Rejected | Transition to `Closed` |
| Deferred | Leave in backlog without `approved` label, or close |

## Rules

- Never create a CR ticket without BA confirmation that it is a real scope change
- The BA is not the decision-maker — do not suggest approving or rejecting on BA's behalf
- Do not modify sprint scope without PM agreement
- CR ticket must always be linked to the original scope item in Jira (use "relates to" link)
- Anti-hallucination: if scope baseline document is not provided — write TBD for scope delta, do not infer

## MCP Path

Phase 2: Create Jira ticket via `jira_create_issue`
Phase 7: Update Jira via `jira_update_issue` + `jira_transition_issue` + `jira_add_comment`

## MCP Unavailable Fallback

If MCP is not accessible:
1. Generate the CR document and Jira ticket content as markdown
2. BA creates the ticket manually
3. Note: "MCP unavailable — ticket content provided for manual entry"
