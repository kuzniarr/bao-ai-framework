---
name: open-questions-per-epic
description: "Generate a per-epic open-questions register as an `.xlsx` with one sheet per epic, grounded in the Product Context and WBS. Use when preparing open questions for a specific epic before refinement. Produces the columns User Story, Address, Priority, Questions, Answer, and Status, with one atomic question per row."
---

# **Open Questions per Epic**

Generate a per-epic open-questions register as an `.xlsx`. Each sheet is one epic; each row is one open question hung off a User Story, routed to whoever resolves it, prioritized, and worded so it can be answered directly. This is refinement-stage work on specific epics — grounded in what the Product Context and WBS already decide, so the list contains only genuine gaps.

## **When to use vs elicitation-prep**

* **open-questions-per-epic (this skill):** a specific epic, refinement stage. Output is the per-epic `.xlsx` register, routed and prioritized. This is the current default for working through epics.  
* **elicitation-prep:** broad project-wide or kickoff session prep, markdown output, topic-grouped. Use it for early/broad sessions; it stays as-is.

## **Inputs**

* Target epic(s) — required (e.g. `User Management`). One epic → one sheet; several epics → several sheets in one workbook.  
* Product Context — the product source of truth. Read the epic's area in full.  
* WBS — for the epic's User Stories / rows. The User Story column maps to these.

## **Output**

`open_questions_per_epic.xlsx` — one sheet per epic. Build the file following the **xlsx** skill conventions.

### **Columns (in this order)**

| Column | Meaning |
| ----- | ----- |
| **User Story** | The epic's story / WBS item the question hangs off. Leave blank when it repeats the row above (visual grouping under one story). |
| **Address** | Routing tag, format `<Who> / <Forum>` — who resolves the question and in what forum. Open vocabulary (not a fixed list). Who: `Client` (the client must rule) or `Internal` (we decide), extensible. Forum: `Refinement`, `SA`, etc., extensible. Examples: `Client / Refinement`, `Internal / Refinement`, `Internal / SA`. |
| **Priority** | `High` / `Med` / `Low`. High = blocks writing AC for this story. Med = needed soon but not blocking. Low = deferrable / nice-to-have. |
| **Questions** | One atomic question (see wording rules below). |
| **Answer** | Blank — filled when the question is resolved. |
| **Status** | `Open` by default. |

## **Procedure**

### **1. Ground in the Product Context and WBS**

Read the epic's area of the Product Context and its WBS stories/rows. Establish what is already decided versus what is silent or ambiguous. Questions come only from the genuine gaps — never from generic assumptions about this kind of product.

### **2. Find the gaps per User Story**

For each story in the epic, ask: what must be known to write its AC that the Product Context does not yet answer? Separate decisions the client must make from decisions the team can settle internally — this drives the Address tag.

### **3. Write each question**

One question per row. Wording rules:

* **Context first, only where it's needed.** If the question needs setup, put one or two sentences of context before the ask, then the ask itself. If it's self-evident, just ask it.  
* **Concise.** Clear and short — not a paragraph. The reader should grasp it in one pass.  
* **Propose a default where one exists.** Append `Proposed: <sensible default>`, or `confirm with <person> if <condition>` (e.g. `confirm with Maddie if security constraints apply`). This turns a question into a fast yes/no where possible.  
* **Atomic.** One decision per row, so each can be answered and closed independently.

Set Address (who/forum), Priority (blocking-first logic), the User Story it belongs to, and Status = Open.

### **4. Prioritize and sort**

Assign Priority by blocking impact (High = blocks AC). Within each User Story, sort rows most-blocking-first (High → Med → Low).

### **5. Build the workbook**

One sheet per epic, the six columns above in order. Group rows under their User Story (blank the User Story cell on repeat rows). Follow the xlsx skill for the actual file.

## **Anti-hallucination rules**

* Do not invent questions from the general product type — base them on what the Product Context and WBS actually leave open.  
* Do not ask what the Product Context already answers — check before adding a row.  
* If a constraint is unclear, write a question about it, not an assumption that hides the gap.  
* If the Product Context or WBS for the epic is missing or not loaded, flag it before generating rather than filling from general BA practice.

## **After generation**

The BA reviews, removes anything already resolved, adjusts Address/Priority, and uses the sheet as the live open-questions register for that epic. Client-addressed questions can be pulled into a client message; the actual message draft is a separate step.
