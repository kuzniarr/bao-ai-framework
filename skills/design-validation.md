---
name: design-validation
description: "Validate a designer's flow or screens PDF against the Product Context and report findings only. Use when the BA asks whether a flow is correct, complete, and matches product logic. Works PC-first, validates flow structure before screens, and keeps Stage 1 triage separate from Stage 2 paste-ready designer comments."
---

# **Design Validation**

Validate a designer's flow (PDF) against the Product Context and return a structured list of findings. The Product Context is the single source of truth for what the flow should contain. This skill reviews only — the designer edits the diagram.

## **When to use vs flow-build**

* **design-validation (this skill):** a flow already exists (PDF); check it against the Product Context and report findings.  
* **flow-build:** generate a flow from requirements.

## **Inputs**

* The design as a **PDF of the full flow** (steps + screens).  
* The **Product Context** — the only source of truth for the validation. Read access required.

## **Why the sequence matters**

The order below exists to prevent a specific failure: validating screens against *generic UX expectations* instead of against what the Product Context actually requires (e.g. accepting or inventing a login step the PC never asked for). The fix is to build the expectation from the PC **before** looking at the design, then validate flow structure before screens. Do not skip or reorder these phases.

## **Procedure**

### **Phase 0 — Build the reference from the Product Context (do NOT open the PDF yet)**

Before looking at the design, derive from the Product Context what this flow *should* contain:

* Actors / roles involved (canonical names).  
* Entry points and exit points.  
* States and transitions the flow must reflect.  
* Decision points and business rules that govern it.  
* The steps the PC implies.

This is the yardstick. Building it first is what stops the design from setting the expectations.

### **Phase 1 — Inventory the design (read, do not judge)**

Read the PDF start to finish and list what is actually there: steps, screens, branches, decision diamonds, start/end nodes. No evaluation yet — this prevents reacting to the first screen and missing the whole.

### **Phase 2 — Validate the FLOW (structure / logic), screens not yet**

Compare the design's structure against the Phase 0 reference:

* Are entry and exit points complete?  
* Are all required states / transitions present, and does none contradict the PC?  
* Are all PC-mandated decision points present?  
* Is there any step or branch in the design with **no basis in the PC**? (This is where an unwarranted login step would surface — flag it, do not accept it.)  
* Conversely, does the PC require anything the flow omits?

Resolve the flow level first: screens hang off it, so if the flow is wrong, screen-level nitpicks are noise.

### **Phase 3 — Validate the SCREENS, one at a time, only after the flow is sound**

For each screen, check only what the PC says about it: fields, data, role-gated elements, states (empty / error / blocked), and copy where the PC specifies it. An element with no basis in the PC is flagged "not in PC — confirm". Do not validate screens against generic UX, and do not draw in anything the design is missing.

### **Traceability spine (applies to every phase)**

Every "contradicts" or "missing" finding **must cite the Product Context** statement behind it. If you cannot cite the PC, the item is not an error and not a silent fix — it is "not traceable to PC — confirm". This is the guard that catches a silent gap: PC silence is never read as permission to add or to accept.

## **Findings classification**

Classify each finding on two axes — direction of the discrepancy × the action it implies:

1. **Contradicts PC** — the design shows X, the PC decided not-X (cite it) → fix toward the PC.  
2. **Missing vs PC** — the PC requires a step / state / branch / field the design lacks (cite it) → add.  
3. **Not traceable to PC** — the design adds something the PC is silent on → flag "confirm", without assuming it is right or wrong. (This is the login case.)  
4. **Open question on design — already resolved in PC** — close it, stating the PC's resolution (cite it).  
5. **Genuinely open / needs client** — flag as an open question and route it.  
6. **Correct** — matches the PC; confirm briefly.

Categories 1 and 3 are kept separate on purpose: in 1 the PC has a decision the design violates (→ fix); in 3 the PC is silent and the design added something (→ confirm — could be a legitimate detail or scope creep). Collapsing them is exactly how a silent addition slips through.

Plus a cross-cutting block: **Decision points / business rules** that should be explicit on the diagram (diamonds / callouts) but are buried as plain steps.

## **What NOT to do**

* Do not edit the diagram or produce fixed versions — review only; the designer applies changes.  
* Do not validate against generic UX expectations — only against the Product Context.  
* Do not treat PC silence as permission: anything not traceable to the PC is flagged "confirm", never silently accepted or fixed.  
* Do not invent missing screens/steps to "complete" the flow.  
* Do not jump to Stage-2 paste-ready comments before the BA has settled Stage 1 and asked for them — and never generate comments from findings that Stage 1 removed.  
* Do not use legacy role labels — map to canonical names; never translate client glossary terms.  
* Do not write Stage 2 designer comments or client/open questions in any language other than English — outward-facing text is always English, even though the Stage 1 triage is in the BA's working language.

## **Output**

Two stages. Stage 1 is the default; Stage 2 is produced **only when the BA explicitly asks for it**. They are deliberately separate — the same gate as pc-update / changelog: settle the findings first, then commit them into comments.

### **Stage 1 (default) — Triage findings**

In-chat structured findings, grouped by the categories above, each finding citing the PC statement behind it. No diagram edits, no file. This is the working pass: the BA reviews it and items get **removed or adjusted** before anything reaches the designer — e.g. findings that belong to a different flow (a login screen reached from registration), screens that just got cut/postponed, or items downgraded after discussion. Do not auto-advance to Stage 2; wait for the BA to settle Stage 1 and ask.

### **Stage 2 (on explicit request) — Paste-ready designer comments**

Trigger only on an explicit ask ("випиши коментарі", "give me the Figma comments", "comments I can paste"). Convert the **settled** findings (after every Stage-1 removal) into comments the BA pastes straight onto the mockups:

* **Grouped by frame / screen** — each comment attaches to the screen it concerns; head each group with the frame's own label (e.g. "Frame: Create your account", "Frame: Step 3 of 4 — How did you hear about us?"). This is what lets the BA drop each one onto the right mockup.  
* **Self-contained** — each comment reads on its own, with no reference to the chat, to other comments, or to "the findings above". The designer sees only the comment.  
* **Tagged by category** — lead with the finding type in brackets: `[Contradicts PC]`, `[Missing vs PC]`, `[Not traceable to PC — confirm]`, `[Decision point]`, `[Open — client]`, etc. Carry the same direction × action meaning as the Findings classification.  
* **No source references** — Stage 2 comments are clean product text. Do not put PC citations (`§n` / `WBS Rn` / decision-owner + date) inside a comment. Traceability lives in Stage 1 only; the designer/client sees a direct product statement, not where it traces to.  
* **Confirm / open items phrased as a question** — for "not traceable" and "needs client" items, write the comment as a direct question to the designer or client, not a flat statement.  
* **Written in English** — every Stage 2 comment is in English, since it goes to the designer/client. This includes any "needs client" / open-question item: its question text is written in English too. The in-chat Stage 1 triage stays in the BA's working language; only the outward-facing comments and client questions are English.  
* **Only actionable categories become comments** — "Correct" items are not posted; they stay in Stage 1 as confirmation.

Regenerate from the settled set only: if Stage 1 dropped an item, it must not appear as a comment. Keep PC traceability in Stage 1 only — Stage 2 comments carry no source references.

Example comment:

**[Missing vs PC]** The login screen is missing "Continue with Google / Microsoft". Social login is required on the login page for all users. → add both buttons.

Output is in-chat only — ready-to-paste comment text, no file, no diagram edits.

---

##
