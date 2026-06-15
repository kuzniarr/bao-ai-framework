---
name: pc-update
description: "Produce OLD→NEW find/replace edits that bring an existing `product_context.md` to its current decided state from one or more new sources. Trigger only when the BA explicitly asks for find/replace or Product Context edits. Purely updates the Product Context and does not touch changelog, trackers, or stories."
---

# **Product Context — Update (find/replace)**

Produce targeted OLD→NEW edits that bring an existing `product_context.md` to its current decided state. The BA applies them by find/replace in VS Code and re-uploads the updated file. The PC holds the *fixed decision* — history goes elsewhere, never into the document.

## **Trigger gate — read first**

Only produce edits when the BA explicitly requests find/replace (or an equivalent commit instruction: "зроби правки в PC", "actualize the PC"). If a source merely arrived and the BA has not asked for edits, do not output OLD→NEW blocks — at that point the source is for analysis and discussion, not editing.

This gate exists because editing is a deliberate commit step, separate from thinking through a source. The BA wants to analyze and settle a decision first, then explicitly ask for the edits once it is final. Jumping to edits on arrival mixes analysis with committing and produces edits before the decision is sound. When unsure whether the BA wants edits yet, ask — do not assume.

## **When to use vs pc-from-zero**

* **pc-update (this skill):** the file exists; the BA asks to reflect one or more sources as edits.  
* **pc-from-zero:** no file yet; build the §1–§12 skeleton and first draft.

## **Inputs**

* The current `product_context.md` — read it in full first. It is large; never edit from memory.  
* One or more new sources, any type: transcript, call decisions, client answers, PRD, approved-feature list. Flexible by design — multiple sources can be processed in one pass. What matters is that the effect of every source on the PC is fully analyzed, not the number of sources.

## **Procedure**

### **1. Read both, then cross-check**

Read every new source and the current PC in full. For each claim a source makes, locate what the PC currently says on that exact point. Aim for complete coverage — every source claim is checked against the PC, nothing skipped. Incomplete cross-check is how reversals slip through.

### **2. Triage every change into one category**

This is the quality core — nothing slips when every item is classified.

| Category | Meaning | Action |
| ----- | ----- | ----- |
| **Reversal** | Source decides something the PC already states *differently* | Replace the old statement. Never leave both versions. Most dangerous — check for these first. |
| **New** | Source adds something the PC is silent on | Add into the right section, with citation. |
| **Conflict** | Source contradicts the PC and no authority resolves it | Do not pick a winner and do not invent an edit. Stop and resolve with the BA in-pass (see step 3). |
| **Already covered** | Source restates what the PC already has | No edit; treat as confirmed. |

### **3. Clarify the real decision, and resolve conflicts in-pass**

For Reversals and Conflicts, state the actual decision at stake in one line — what changed and why it matters — before the edit, so it is reviewed rather than rubber-stamped.

**Conflicts stop the pass at the point they arise.** When a Conflict surfaces, do not defer it to the end and do not continue emitting edits past it. Clarify the conflict, ask the BA which way to go, and wait for the decision before proceeding. Once resolved, continue the pass from where it stopped.

Precedence when claims clash: an explicit client decision in a new source supersedes an older PC statement (→ Reversal, replace). The WBS baseline takes priority over conflicting prose. Two equal-authority sources that disagree with no resolution → Conflict, resolve in-pass.

### **4. Produce the edits**

For each Reversal and New item, output an OLD→NEW block:

* **OLD:** the exact current PC text to find (verbatim, enough surrounding text to be unique for a single find/replace).  
* **NEW:** the exact replacement.

Point-edits by default; full-section replacement only when more than roughly half a section changes. Write the *final decided state* — never "previously X, now Y" inside the PC. Keep inline citations on every new or changed statement (`(Sx HH:MM:SS)` / `(Sx §n)` / `(WBS Rn)`). Tag each block with its category so the BA sees what they are applying.

## **What NOT to do**

* Do not produce edits before the BA explicitly asks for find/replace.  
* Do not rewrite or re-emit the whole file — only targeted OLD→NEW blocks.  
* Do not silently resolve a real conflict — stop and ask in-pass.  
* Do not write history or chronology into the PC — the PC holds the fixed decision only.  
* Do not touch the changelog, any tracker, or write stories/AC. This skill only updates the Product Context.

## **Output**

Category-tagged OLD→NEW find/replace blocks, ready to apply in VS Code. No file regeneration. After the BA applies them and re-uploads the updated PC, verify the edits landed if asked.

---
