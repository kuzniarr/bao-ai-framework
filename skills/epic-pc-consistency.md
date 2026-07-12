---
name: epic-pc-consistency
description: "Validate a decomposed epic (stories + AC) against the Product Context and against other already-defined epics — reviews only, never edits. PC-first: derive from the PC what the epic must contain before reading the epic, then check both directions (the epic contradicting or not traceable to the PC, and PC decisions the epic fails to cover), cross-epic duplication/clash against the defined epics the BA provides, and internal contradictions within the epic itself. Every finding cites the PC; anything the PC is silent on is flagged to confirm, never resolved against generic assumptions. Produces categorized findings for the BA to settle — fixes go through pc-update / notion-update in Refine, not here. Use before refinement to check a whole epic's requirements against the source of truth. Trigger: 'звір епік з PC', 'провалідуй вимоги епіку', 'check epic against PC', 'epic consistency check'. For AC completeness of a single story use ac-validation; for designs use design-validation."
---

# /epic-pc-consistency

## Purpose

Check one decomposed epic's requirements (stories + AC) against the source of truth and its neighbours. This is to **requirements** what `design-validation` is to **designs**: a PC-first review that reports findings only — it never edits the PC, the stories, or the AC. Fixes are applied afterwards through `pc-update` / `notion-update` under the Refine mode.

## When to invoke

- After an epic is decomposed and its AC written (`decompose-wbs-epic` → `ears-ac`), before or during refinement, to check the whole epic against the PC and already-defined epics.
- One epic per invocation.

## Inputs

- **The epic under review** — its stories + AC (Notion MD), pasted by the BA. This is what gets validated.
- **Product Context** — `/mnt/project/Product_Context.md`. The source of truth. Read the **current** document in full; a rule relevant to the epic may live in any section, not only the one matching the epic's name.
- **Defined epics** — the stories + AC (Notion MD) of already-defined epics, **provided by the BA**. Needed only for the cross-epic pass, and only for epics that are actually built into requirements — an unbuilt epic cannot clash. The PC alone is **not** enough here: the PC states each rule once and will not reveal that a story duplicates a rule another epic already owns.

## PC-first — derive before you read

Before reading the epic's stories, build the reference from the PC: what this epic **must** contain — the roles, states, rules, messages, and flows the PC assigns to it. Only then read the epic and compare against that reference. Deriving expectations from the PC first is what makes both directions possible and stops the review from rubber-stamping whatever the stories happen to say.

**PC silence is never permission.** Where the PC says nothing on a point the epic asserts, that is a finding to confirm — not a gap to fill with a generic assumption, and not something to wave through.

## Finding categories

Every finding lands in exactly one category and cites the PC (`§n`) or the specific defined-epic story it clashes with.

| # | Category | Meaning |
|---|---|---|
| 1 | **Contradicts PC** | A story or AC decides something differently from what the PC states. The PC is the resolved state → the requirement is wrong (or the PC is stale — flag which). |
| 2 | **Not traceable** | A story or AC asserts something the PC is silent on. Flag to confirm; never invent the backing rule. |
| 3 | **Missing coverage** | A PC decision relevant to this epic that **no** story covers — the epic drops something the source of truth requires. (Reverse direction.) |
| 4 | **Cross-epic clash** | A story duplicates or contradicts a rule/flow owned by a **defined** epic. Includes convention breaches — a rule owned elsewhere must be referenced, not re-stated (`ears-ac` house rule). |
| 5 | **Internal inconsistency** | Two stories (or their AC) within this same epic contradict each other. |

## Procedure

1. **Derive the PC reference** for the epic (PC-first, above).
2. **Direction A — epic against PC.** For every story/AC claim, locate the matching PC point. Classify each into Contradicts (1) or Not traceable (2). Aim for complete coverage — every claim checked.
3. **Direction B — PC against epic.** Walk the PC points relevant to this epic; any decision no story covers → Missing coverage (3).
4. **Cross-epic pass.** Against the defined-epic MDs the BA provided, flag duplication or contradiction → Cross-epic clash (4). Skip if the BA provides no defined epics (note that the pass was skipped).
5. **Internal pass.** Compare stories within the epic against each other → Internal inconsistency (5).
6. **Report (Stage 1 triage).** Group findings by category, each with its PC (or defined-epic) citation and a one-line statement of the real decision at stake, for the BA to settle. Do not rank a winner on a genuine conflict — surface it and stop for the BA's call.

## Output

Categorized findings for the BA to settle — grouped by the five categories, each citing the PC section / defined-epic story, with the decision at stake in one line. No edits, no rewrites of stories or PC. Once the BA settles a finding, the actual change is applied through `pc-update` / `notion-update` under Refine — not by this skill.

## What this skill does NOT do

- Does not edit the PC, the stories, or the AC — reviews only.
- Does not resolve a real conflict silently — surfaces it for the BA.
- Does not treat PC silence as permission — flags it to confirm.
- Does not validate against generic UX or generic BA assumptions where the PC is silent.
- Does not run the cross-epic pass on undefined epics, and does not invent a defined-epic MD it was not given.

## Behavior parameters

- Read the current PC in full; never validate from memory of an earlier version.
- Read-only. Read-only MCP (if used later to pull epics) runs without approval.
- Anti-hallucination: never invent a PC rule, a defined-epic story, a role, or a field. Unknown backing → flag as Not traceable, never fabricate.

## Relation to other skills

- **`ac-validation`** — inside one story (happy path / edges / errors). This skill is the outward axis: the whole epic against the PC and neighbours. Complementary.
- **`design-validation`** — same PC-first philosophy, but for designs, not requirements.
- **`decompose-wbs-epic` / `ears-ac`** — run before this; this checks their combined output against the source of truth.
- Fixes flow to **`pc-update`** / **`notion-update`** under the Refine mode.


flow-build
