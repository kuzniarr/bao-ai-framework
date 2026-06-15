---
name: pc-from-zero
description: "Build a project's Product Context document (`product_context.md`) from scratch by incrementally synthesizing BA-provided sources (WBS, discovery transcripts, PRD, kickoff notes, client answers) into one traceable product source-of-truth. Use whenever there is no product context file yet and you are creating the initial skeleton and first population. Once the file already exists and a new source must be folded into it, use pc-update instead."
---

# **Product Context — From Zero**

Build `product_context.md` as the single product source-of-truth for a project, synthesized only from sources the BA provides. The document grows source by source; this skill owns the empty skeleton and the first build through to a usable draft.

## **When to use vs pc-update**

* **pc-from-zero (this skill):** no product context file exists yet. You create the §1–§12 skeleton and populate it from the first source(s) until the BA has a coherent first draft.  
* **pc-update (separate skill):** the file already exists and a new source must be folded in as targeted find/replace deltas. Hand off to it once the file is established.

The seam matters: this skill is allowed to create and shape the file; pc-update is not, it only patches an existing one.

## **Inputs**

Any combination, provided gradually and in an order the BA chooses. Do not expect specific documents and do not constrain the source list in advance. Typical sources: WBS, discovery / elicitation transcripts, client PRDs, kickoff notes, client answers to questions, designer mood-board responses.

## **Core principles**

These are the reason the document stays trustworthy — hold them on every source.

1. Know NOTHING about the product upfront. All product knowledge comes only from sources the BA adds or pastes in. Nothing comes from prior projects or assumptions.  
2. Do not invent. If something is not covered by a source, it goes into §9 Open Questions, not into a guess.  
3. Do not hedge with "design assumption", "likely", or "presumably". If a source is silent, write "not defined in sources" and log it as an open question.  
4. If two sources contradict, record both versions with source references and move the conflict into §9 Open Questions. Do not pick a winner.  
5. Cite sources precisely. Every statement carries a traceable inline reference, e.g. `(Source X §3)`, `(Source Y row 88)`, `(Source Z, 00:14:22)`.  
6. Preserve domain terms exactly as a source spells them. Once a glossary term appears in a source, do not translate or rename it.

## **Procedure**

### **1. Create the skeleton**

On the first source, instantiate the full §1–§12 skeleton below as `product_context.md`. Section names are fixed; every section starts empty and is populated only as a source supports it. Do not pre-fill any section with what the product "probably" has.

### **2. Per-source loop**

After each source the BA provides:

1. Incrementally update the file:  
   * Add new information into the right sections.  
   * Refine prior statements where this source clarifies them.  
   * Record contradictions in §9 (both versions, with citations).  
   * Add the source to §11.1 with date and a short description.  
   * Close items in §9 that this source answers — keep them visible with a `Closed by: <source>` note for traceability. (Resolved questions may later be removed during a dedicated cleanup; do not delete them here.)  
2. Deliver the full updated `product_context.md` as a file.  
3. Give a short changelog in chat (5–10 lines): which sections changed, which new capabilities / rules / entities / roles / integrations / constraints were added, which contradictions were recorded, which open questions were opened or closed.  
4. Wait for the next source. Do not rebuild the document from scratch to show progress — updates are targeted and traceable.

### **3. Finalize**

When the BA says "збираємо" / "build product_context" / "finalize": verify the document is internally consistent, every section is populated where sources allow, the permission matrix legend is defined, and §9 Open Questions is current.

## **Document skeleton**

Instantiate exactly this structure. The slots are intentionally empty until a source fills them.

```
# Product Context — <project name from sources>
_Last updated: <date> · Sources processed: <list with dates>_

## 1. Product overview
Two-paragraph description of what the product is, for whom, key value
proposition, and business model(s). All facts must come from sources.

## 2. Users and roles
Create subsections only if sources support them:
- 2.1 Role hierarchy — roles as they appear in sources, each with a brief description.
- 2.2 User attributes — any attribute tracked on the user record.
- 2.3 User states and transition rules — states a user can be in, and what triggers transitions.

## 3. Domain model
Subsections emerge from sources (e.g. organization hierarchy, group / membership
rules, course model, certificate model, assignment model, compliance model,
training records). Only include subsections sources provide content for.

## 4. Functional domains
One subsection per functional area; domain names emerge from sources, not a fixed list.
For each domain: capabilities offered, which roles can perform which capability
(cite a source for each role-capability pair), business rules and constraints.

## 5. System rules and automations
Rules that run without a role triggering them: time-based state transitions,
automated data linking, mandatory validations, data-preservation rules,
ownership-transfer-on-deactivation, etc.

## 6. Integrations
External systems the product integrates with. For each: purpose, direction
(inbound / outbound / bidirectional), constraints. WHAT integrates and WHY, not tech stack.

## 7. Out of scope (MVP)
What is explicitly excluded from the MVP per sources.

## 8. Parallel tracks
Initiatives running alongside the MVP without blocking it. Status + known details per source.

## 9. Open questions
Items where sources are silent, ambiguous, or contradictory. For each:
- the question
- date and source where it first appeared
- status: Open / Closed by <source>
- if closed: the resolution and the source that resolved it

## 10. Glossary
Domain-specific terms as they appear in sources. Each term: definition + source reference.

## 11. Source map
- 11.1 All sources processed (name, date, short description, type).
- 11.2 Traceability table: which decisions/statements in §4 and §5 come from which source. One row per traceable statement.

## 12. Permission matrix
One consolidated table of every permission-bearing capability across all roles.
This is the only place permissions are summarized in tabular form (§4 describes
capabilities textually and does not repeat the table).
Columns: Domain (matches §4) · Capability · one column per role · Source(s) ·
Confidence (Confirmed / Partially Confirmed / Open) · Notes.
Cell values: ✅ / ❌ / ✅ scoped / ❓ / N/A — pick consistently and define the
legend at the top once roles are known.
```

## **What NOT to do**

* Do not write user stories or acceptance criteria — that is downstream work.  
* Do not structure §3 or §4 by WBS epics or by UI flows. Structure emerges from the product's own domain language as sources express it.  
* Do not split sub-artifacts (permission matrix, open questions) into separate files — everything lives inside `product_context.md`. (If the project later adopts a dedicated open-questions tracker, §9 migrates there; until then it stays in the document.)  
* Do not rebuild the document from scratch on update — increment only.  
* Do not pre-fill any section with assumptions about what the product might contain.

## **Style**

Direct, no preamble. Markdown. Bullet points are full sentences, not fragments. Tables where they add clarity (especially §11.2 and §12).

## **Output**

Save as `product_context.md`. Deliver the full file after each source, plus the in-chat changelog.

## **Update trigger**

Once the file exists and is in active use, fold new sources in with the **pc-update** skill, not this one. Return to pc-from-zero only when starting a brand-new project's product context.
