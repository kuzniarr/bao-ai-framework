---
name: story-map-per-epic
description: "Build a Miro-importable xlsx story map for one epic from the approved output of decompose-wbs-epic. Lay the WBS parent stories left-to-right as the backbone and stack their decomposed child stories beneath each. Arrange only the approved stories and do not invent new ones."
---

# /story-map-per-epic

## Purpose

Turn the approved decomposition of **one epic** into an xlsx that Miro imports as sticky notes, so the BA can arrange and plan the stories on a board. Parent → child traceability is preserved by the layout: each column is a parent WBS story, the stickies under it are its child stories.

## When to invoke

After `decompose-wbs-epic` output is approved, when the BA wants the stories as a Miro board for planning.

## Input

The approved `decompose-wbs-epic` output for one epic. From it, read:

- the **epic** name and WBS Topic (from the `# Decomposition — \<Epic\> (WBS Topic: \<TOPIC\>)` line);  
- each **parent WBS story** (each `## \<parent title\>`);  
- the **child stories** under each parent (the numbered `As a \<role\>, I want \<action\>, so that \<benefit\>.` items).

## Layout

One epic per file. Build a grid:

```  
Row 1:  \<EPIC\>          (col A only)  
Row 2:  \<parent 1\>   | \<parent 2\>   | \<parent 3\>   | ...   ← backbone, one parent per column  
Row 3:  \<child 1.1\>  | \<child 2.1\>  | \<child 3.1\>  | ...   ← child stories stacked under their parent  
Row 4:  \<child 1.2\>  | \<child 2.2\>  | ...  
...  
```

- **Column = a parent WBS story** (the backbone cell in Row 2, parent title verbatim). This is the parent link.  
- **Order the backbone left-to-right as the user's journey over time** — the sequence of the flow, not the WBS row order. (e.g. for registration: sign-up flows → verification → activation → consent → login → SSO → password recovery.) A shared/canonical step (consent, verification) gets its own backbone column placed where it occurs in the journey.  
- **Cells below a parent = its child stories**, in decomposition order.  
- A parent **kept 1:1** has one child beneath it (its single story).  
- Empty cells are fine — Miro skips them.

### Sticky text

- Backbone (Row 2): the **parent WBS story title**, verbatim.  
- Child stickies: the story's **action phrase** — the text between `I want` and `so that` — kept concise and verbatim (do not paraphrase). Keep the full statement in the decomposition MD; the board uses the short label.

## Anti-hallucination

Arrange only the stories present in the approved input. Do **not** generate, infer, or top up epics, parents, or stories. If the input is missing or unclear, ask for the approved decomposition rather than inventing content.

## Generate

Use openpyxl. One sheet named `Story Map`. No formulas, no colour, no formatting — sticky colour, size, and position are set in Miro after import.

```python  
import openpyxl  
from openpyxl import Workbook

# grid: list of rows, each a list of cell values (None for empty)  
# row0 = [EPIC]  
# row1 = [parent_1, parent_2, ...]  
# row2+ = children aligned by parent column

wb = Workbook()  
ws = wb.active  
ws.title = "Story Map"  
for r, row in enumerate(grid, start=1):  
    for c, val in enumerate(row, start=1):  
        if val:  
            ws.cell(row=r, column=c, value=val)

wb.save('/mnt/user-data/outputs/story_map_\<epic_short\>.xlsx')  
```

File name: `story_map_\<epic_short\>.xlsx` (e.g. `story_map_user_registration.xlsx`).

## Deliver

`present_files` the xlsx, then one line: "Done — import into Miro via Insert → Upload from apps → Excel."
