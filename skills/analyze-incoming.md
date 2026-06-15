---
name: analyze-incoming
description: "Explain an incoming item and route it to the next action. Use when the BA pastes a client change-log comment, client message, transcript snippet, PRD section, or newly proposed feature and asks what it means and what to do. Cross-check against the current Product Context and WBS, state the real decision at stake, and route it without editing the Product Context or changelog."
---

# Analyze Incoming — explain & route

Take one incoming item and do two things: explain what it actually is and the real decision behind it, then route it to the next action. This is the front-door reasoning step that *precedes* any commit to the Product Context or changelog — it grounds the item, makes sense of it, and decides where it goes.

## What this skill is and is not

- **Is:** the analysis and routing step. Ground the item against the project, explain the decision, decide the next action.  
- **Is not:** the PC commit step (→ pc-update) or the changelog write step (→ changelog skill). It does not edit the PC or the changelog. It *may* draft a single client question when that is the route, because that is a small inline action — assembling a full prioritized question set for a session is the separate, heavier elicitation-prep skill.

## Inputs

One or more incoming items, any type: client change-log comment, client Slack/board message, transcript snippet, PRD section, newly proposed feature. Plus read access to the current Product Context (and the WBS where row numbers matter).

## Procedure

### 1. Ground the item

Targeted cross-check against the current PC (and WBS if relevant): what does the project already say on this exact point? Is this genuinely new, already covered, a change to something decided, or a contradiction? Do not analyze in a vacuum — a wrong read here sends the item down the wrong route.

### 2. Explain the item and the decision behind it

State what the item actually is and the real problem or decision at stake — clearly and directly, in normal working style. Do not just restate the item back verbatim; surface the meaning the BA needs to act on. Keep it as long as the item requires and no longer.

### 3. Route to the next action

Classify the item and say it crisply as "this is X → do Y". The routes below are the common ones; they are not a closed set — if a different action fits the item better, use it and name it.

| Route | When | Hand-off |
| :---- | :---- | :---- |
| **Apply directly** | A decision already exists, or it is a pure narrowing / cleanup — no client input needed | Name the commit target: PC update · changelog · board comment · requirement/AC edit. Also say whether it can be applied as-is or should still be confirmed with the client (the recurring "apply now vs bring to call" call). The commit itself happens via the matching skill on the BA's command. |
| **Ask the client** | The item raises something undecided or ambiguous; the client must rule | Draft the question itself — short, in English, addressed to the right person (Danielle for product, Maddie for technical). Flag it if it carries scope risk. |
| **Bring to call** | Needs live discussion, multiple parties, or trade-offs | Note what to put on the agenda and why. |
| **No action** | Already covered, out of scope, or a note that only needs acknowledging | Say which, and that nothing changes. |

An item can carry more than one route (e.g. apply part directly, ask the client about the rest) — split it and route each part.

### 4. Stop at the route

End at the routing decision (plus the drafted question where the route is "ask the client"). Do not emit find/replace blocks and do not write changelog entries. If the BA then says "дай find/replace" or "запиши в changelog", that hands off to pc-update or the changelog skill.

## What NOT to do

- Do not edit the Product Context or write the changelog from this skill — analyze and route only. (Drafting a single client question is allowed; producing PC edits or changelog entries is not.)  
- Do not pick a winner on a real conflict — route it to the client and draft that question.  
- Do not restate the item verbatim instead of explaining it; lead with the decision at stake.  
- Do not invent project facts — cross-check against the PC/WBS, or mark "not defined, needs confirming".

## Output

In-chat only: a clear explanation of the item and the decision behind it, plus the route per item ("this is X → do Y", with apply-now vs confirm-with-client noted). Where the route is "ask the client", include the drafted question. No files, no PC/changelog commits.
