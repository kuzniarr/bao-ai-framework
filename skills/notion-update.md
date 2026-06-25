---
name: notion-update
description: "Apply one targeted find/replace edit to an existing requirement on a Notion page, given the page link and the change. Produce a precise OLD→NEW edit, then either apply it through the Notion connector behind an approval gate or hand the block to the BA for manual apply. Use when the BA gives a Notion page link and asks to change or update a requirement there."
---

# Notion — Update requirement (find/replace)
Apply one targeted change to an existing requirement (story + EARS AC) living on a Notion page. The skill's job is to produce one precise OLD→NEW edit and get it into the page — by connector when available, by hand when not. Notion holds the refinement-facing requirement; product_context.md remains the local source of truth and is never touched here.

## Trigger gate — read first
Only produce an edit when the BA explicitly asks to change a requirement on a named Notion page (or pastes the change and says "put this on the page"). A change merely being discussed in chat is not the trigger — discussion and deciding the change come first, applying it to Notion is a separate, explicitly-requested commit step. When unsure whether the BA wants the edit committed yet, ask; do not pre-emptively emit OLD→NEW.

## When to invoke
A requirement already on a Notion page changed (decided in chat, or the BA pasted the new wording) and the BA wants Notion brought in line.
One page, one change per invocation. Several edits to the same page are fine in one pass, but each is its own OLD→NEW block.

## Prerequisites
The page link is provided by the BA. The Notion page ID is never constructed or guessed (anti-hallucination); it comes from the BA's link, or from an MCP search that the BA confirms is the right page.
The change is final enough to commit (the decision is settled, not still under discussion).
For the apply step: Notion MCP connector active with edit access → live mode. Otherwise → manual mode (the default today, while connector access is pending).

## Inputs
Page link — from the BA.
The change — usually born in chat from our discussion; sometimes pasted by the BA as finished markdown. Either way the skill produces the OLD→NEW from it.
The exact current text on the page — needed to build a valid OLD. In live mode it is read via MCP. In manual mode the page cannot be seen, so the OLD must come verbatim from shared context; if the exact current string is not known, ask the BA to paste the current fragment rather than approximating it. An OLD that does not match the page byte-for-byte will silently fail to find/replace.

## Procedure
1. Get the current text (read-only — no approval)
Live mode: fetch the page via MCP and read the current wording around the change.
Manual mode: use the exact current text from context; if it is not available verbatim, ask the BA to paste the current fragment. Never reconstruct the page text from memory.
2. Locate the fragment and classify the change
Find the precise span to change — point-edit by default, never the whole page. State the real change in one line so it is reviewed, not rubber-stamped — what it was, what it becomes, why. If the change contradicts what the page currently says with no clear authority, stop and resolve with the BA before producing the edit.
3. Produce the OLD→NEW
OLD: the exact current page text to find — verbatim, with just enough surrounding text to be unique on the page for a single find/replace.
NEW: the exact replacement, in the page's existing format (Notion-flavored markdown — heading/list/table as the surrounding content uses).
Write the final decided state only — never "previously X, now Y" inside the requirement.
4. Approval gate, then apply
Show the BA: the page link, the OLD, the NEW, and the one-line change description. Wait for explicit "ok" / "go".
Live mode: apply the point replacement via MCP — replace the located fragment, not the whole page. Read-before-write already done in step 1.
Manual mode: the approved OLD→NEW block is the deliverable; the BA applies it via Notion's find/replace.
5. Report
What changed (one line) → page link.

## Notion mechanics — caveats
Hosted Notion MCP operates at page level: it fetches and rewrites page content, with no block-level endpoints. A small point-edit is safe; the risk of an unintended whole-page rewrite appears only on large structural changes (re-ordering blocks, rewriting a whole section) — flag those before applying.
Keep requirement pages structurally simple (prose, headings, ordinary tables). Notion-specific structures — nested/linked databases, toggles, columns, synced blocks — can be mangled by a markdown round-trip; do not edit through those, and warn the BA if the target fragment sits inside one.

## MCP unavailable fallback
This is the default today. Produce the OLD→NEW block and hand it to the BA, who applies it in Notion by find/replace. The skill is fully useful in this mode — the connector only removes the manual apply step, it does not change what the edit is.

## Behavior parameters
Temperature low — deterministic edit production, not generation.
Read before write; approval before write. Read-only MCP (fetch/search) runs without approval.
Anti-hallucination: never invent the page's current text, never construct a page ID, never add AC/fields/structure the change does not contain. If the exact OLD is unknown, ask — do not approximate.

## What this skill does NOT do
Does not first-write or create a page — edits only. (First-write of a Notion requirement page is a separate skill.)
Does not touch product_context.md — that is pc-update. Notion and the PC are different artifacts; this skill stays inside Notion.
Does not decide which page — the BA provides the link.
Does not write or re-derive the requirement from scratch — it commits a settled change.
Does not run a DoR check, validate fields, or add flags.
Does not rewrite the whole page — point-edits only, save for explicitly-requested structural changes.

## Relation to other skills
Notion-side sibling of pc-update (which does the same OLD→NEW discipline on the local product_context.md). Same find/replace philosophy, different artifact.
Consumes a settled story + AC (post ears-ac / gherkin-ac and refinement). It commits changes, it does not author requirements.
A future notion-fill would cover first-write of a page; this skill is strictly update.
