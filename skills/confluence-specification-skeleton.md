---
name: confluence-specification-skeleton
trigger: /confluence-specification-skeleton
phase: 3 · Requirements & Modelling
tags: [SKILL, MCP]
output: Confluence page structure — parent page + child sections created via Atlassian MCP
---

# Confluence Specification Skeleton

Creates the Confluence page structure for a feature or epic before writing requirements. Sets up the specification skeleton, all sections are created as empty pages ready to be filled. All subsequent requirements and stories are written as child pages under this structure.

This skill acts directly in Confluence via Atlassian MCP, no copy-paste required.

---

## When to Use

- Before writing requirements for any new epic or feature
- When starting a new stream on an existing project
- When setting up the initial Confluence space structure for a project

**Do not use** for individual user stories, those are child pages under the Feature section, created via `/decompose-epic` or manually.

---

## Prerequisites

- Atlassian MCP connector active in this chat
- Confluence space key known (from `01_project_context.md` or `03_tech_context.md`)
- Epic or feature name and brief description ready

---

## Input

```
/confluence-specification-skeleton
Epic: [Epic name]
Space: [Confluence space key]
Parent page: [Parent page name or ID — optional, defaults to space root]
Description: [1–2 sentences about what this epic covers]
```

If space key or parent page is missing, ask before creating anything:

> "Which Confluence space should I create this in? And should it sit under a specific parent page, or at the space root?"

---

## Steps

**Step 1, Confirm structure before creating**

Show the planned page tree to the BA before executing:

```
[Epic Name] — Specification ← parent page
├── 1. Overview & Goals
├── 2. Scope
├── 3. Non-Functional Requirements
├── 4. Architecture Overview
├── 5. User Stories
│ └── [placeholder — stories added here]
└── 6. Open Questions
```

Ask: *"Does this structure look right? Any sections to add or remove before I create it?"*

Wait for confirmation. Do not create pages before confirmation.

**Step 2, Create parent page**

Create the parent page in the specified Confluence space:

- Title: `[Epic Name] — Specification`
- Body: intro paragraph using the description provided + table of contents macro

```markdown
This page is the specification for the **[Epic Name]** epic.

All requirements, user stories, and technical notes for this feature are documented here.

| Section | Description |
|---|---|
| Overview & Goals | What this feature is and why we build it |
| Scope | What is in and out of scope |
| Non-Functional Requirements | Performance, security, compliance constraints |
| Architecture Overview | High-level technical approach |
| User Stories | Decomposed stories with AC |
| Open Questions | Unresolved questions pending stakeholder input |
```

**Step 3, Create child pages**

Create each section as a child page under the parent. For each page, add a placeholder body so the page is not blank:

| Page title | Placeholder content |
|---|---|
| `1. Overview & Goals` | `## Overview` + `## Business Goals` + `## Target Users` headers |
| `2. Scope` | `**In scope:**` / `**Out of scope:**` / `**TBD:**` sections |
| `3. Non-Functional Requirements` | NFR table with columns: Category / Requirement / Priority / Notes |
| `4. Architecture Overview` | `_To be completed with Tech Lead input._` placeholder |
| `5. User Stories` | `_Stories will be added as child pages under this section._` |
| `6. Open Questions` | Questions table: # / Question / Owner / Status / Resolution |

**Step 4, Report result**

After all pages are created, return:

```
✅ Specification structure created in Confluence.

Parent page: [link]
Space: [space key]

Pages created:
- [Epic Name] — Specification
 - 1. Overview & Goals
 - 2. Scope
 - 3. Non-Functional Requirements
 - 4. Architecture Overview
 - 5. User Stories
 - 6. Open Questions

Next step: fill in Overview & Goals, then run /decompose-epic to populate User Stories.
```

---

## MCP Fallback

If Atlassian MCP is not available or returns an error:

1. Do not generate fake Confluence output
2. Tell the BA: *"Confluence MCP is not accessible. I can't create pages directly. Instead, I'll give you the full page structure as markdown you can paste manually."*
3. Generate the full markdown for each section as a paste-ready document

---

## Anti-hallucination Rules

- Do not create pages without confirmation of structure
- Do not infer the Confluence space, always ask if not provided
- Do not fill in requirements content, pages are created empty with placeholders only
- If MCP fails silently (returns success but pages don't appear), instruct BA to verify manually before proceeding