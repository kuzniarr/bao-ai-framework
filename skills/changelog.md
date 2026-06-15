---
name: changelog
description: "Write a single change-log item as a ready-to-paste row. Trigger only when the BA explicitly asks for it. Produces columns A–G plus Type, with Change Requests reusing the original WBS story and AC verbatim while marking the delta. Does not edit the Product Context."
---

# **Changelog Item**

Write one change-log item from a decided change, as a row matching the project change log. Output is a markdown table the BA pastes into the change log. The skill writes the content columns only and never touches estimation or process-metadata columns.

## **Trigger gate — read first**

Only produce a change-log item when the BA explicitly asks for one ("напиши айтем в чейнджлог", "write a changelog item"). A decision or source arriving is not the trigger — writing the item is a deliberate step the BA invokes. When unsure, ask.

## **Inputs**

* The decided change — from a call summary, client answer, or stated decision.  
* The **WBS** — for Change Requests, to pull the original story and AC verbatim.  
* The existing change log — to match its AC style.

## **Columns written**

Write columns **A–G and I (Type)** only. Leave Responsible (H), Link (J), Role (K), Status (L), Approval Date (M), Jira (N) and all estimation columns (O onward) untouched — those are team/process metadata.

| Col | Name | Content |
| ----- | ----- | ----- |
| A | Date | Date of the decision / call the change came from. TBD if not provided — do not invent. |
| B | Source | Where it came from (e.g. Elicitation #N, WBS comment, Asana task, client call). TBD if unknown. |
| C | Epic Name | The epic from scope. |
| D | Task / US Description | The user story (see Type logic). |
| E | Acceptance Criteria | The AC, numbered to match the existing change log (not EARS). |
| F | Phase | `MVP` or `Future Phase`. |
| G | Comments / Questions | Short description of the change, or `No new`. |
| I | Type | `New Feature Request` or `Change Request` — these two values only. |

## **Type logic — this drives the whole item**

### **New Feature Request**

No matching item existed in the WBS. Write **D (story) and E (AC) from scratch**: a proper user story (`As a <role>, I want <action>, so that <benefit>`) and numbered AC in the change log's style.

### **Change Request**

A WBS item exists and is being modified — a phase move (MVP → Future Phase) and/or an AC change. The story and AC must be the **original WBS text, verbatim** — do not paraphrase or rewrite them. Then show the delta:

* **Phase change:** set F to the new phase; describe the move briefly in G.  
* **AC change:** in column E, keep the original AC; **strike removed requirements** with `~~strikethrough~~`; add new ones under a `New requirements:` heading. In G, briefly state what changed — or write `No new` when nothing is added to the AC.

## **What NOT to do**

* Do not produce an item before the BA explicitly asks.  
* Do not write any column outside A–G and I — never touch metadata or estimation columns.  
* For a Change Request, do not paraphrase the original story or AC — reuse the WBS text verbatim and only mark the delta.  
* Do not reformat AC into EARS — match the change log's numbered style.  
* Do not invent Date or Source — use TBD.  
* Do not edit the Product Context or any other artifact — this skill writes one change-log item only.  
* Use canonical role names; never translate client glossary terms.

## **Output**

A single markdown table in chat with columns A–G and I, ready to paste. Strikethrough via `~~...~~` for removed AC requirements.
