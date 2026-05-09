---
name: miro-xlsx-generator
description: "Use this skill whenever you need to generate an xlsx file for import into Miro as sticky notes. Trigger: any request to create a story map, miro board, stickies in miro, miro table, flow map, discovery map, or to structure features / flows / questions as a board. Also triggers when input content (feature list, notes, questions) is provided with a request to make xlsx or prepare for miro. Supports two types: Story Map and Flow/Discovery Map."
---

# Miro XLSX Generator

Generates an xlsx file in the format that Miro imports as sticky notes. Color, style, and size of stickies are adjusted manually after import.

---

## Two Template Types

### Type 1: Story Map

Use when: product features, user stories, backlog, epic → activity → story structure.

**xlsx structure:**
Row 1: Epic 1      | Epic 2      | Epic 3      ...   ← epic headers
Row 2: Activity    | Activity    | Activity    ...   ← main activities
Row 3: User story  | User story  | User story  ...   ← details / stories
Row 4: User story  | ...                              ← additional rows

- Column = one epic with its activities/stories below it
- If an epic has multiple activities — they go in separate columns
- Empty cells are fine — Miro simply skips them

### Type 2: Flow / Discovery Map

Use when: step-by-step flows, questions by user type, discovery artifacts.

**xlsx structure:**
Row 1: Step/Epic 1  | Step/Epic 1 | Step/Epic 2 | Step/Epic 2 ...  ← repeated for sub-groups
Row 2: Sub-group A  | Sub-group B | Sub-group A | Sub-group B ...  ← sub-groups
Row 3: Item         | Item        | Item        | Item        ...  ← questions / elements

- Row 1 is logically "merged" — repeat the epic name as many times as there are sub-groups under it
- Row 2 — sub-group names (user types, subtopics, etc.)
- Rows 3+ — the actual items

---

## Workflow

### Step 1: Determine the type

If input contains epics → stories: **Story Map**
If input contains flow steps → questions/elements per step: **Flow Map**
If unclear → ask the user.

### Step 2: Parse or generate content

Accepted input formats: bullet list, numbered list, markdown table, free-form text.

If content is missing — generate it based on context, show structure to user, confirm before creating file.

### Step 3: Show structure if generated or uncertain

Show the proposed structure and ask: "Does this look right? Anything to add or change?"

If content is clear and detailed — generate directly without asking.

### Step 4: Generate xlsx

Use openpyxl. One table, one sheet named `Story Map` or `Flow Map`.

Fill rules:
- One item = one cell
- No formulas, no formatting
- File name: `miro_[type]_[short_name].xlsx`

### Step 5: Deliver

After generating — present_files.
Import into Miro via Insert → Upload from apps → Excel.
