---
name: quick-standards
description: "Generate `00_quick_standards.md` as the project reference for story format, AC format, DoR, DoD, Task Split Pattern, naming conventions, and glossary anchors. Use during setup or when standards change mid-project."
---

# Skill: /quick-standards

## Purpose

Generate `00_quick_standards.md` — the operational reference for daily BA work on the project. Documents story format, AC format, DoR/DoD, Task Split Pattern, naming conventions, and glossary anchors. Every skill that produces stories, AC, or tickets references this file.

## When to use

- Phase 1 · Project Setup, after governance and charter are generated.
- When standards need to be updated mid-project (e.g. team agrees to switch from Gherkin to EARS).

## Inputs

- WBS file or extract (to detect existing story format and AC patterns already used by client or in the Geniusee template)
- `governance.md` from Project Knowledge
- `01_project_context.md` from Project Knowledge
- Optional: client's existing standards doc, internal Geniusee BA standards

If WBS is missing — generate from Geniusee defaults and flag every assumed item for BA review.

## Output

Single file: `00_quick_standards.md` in Project Knowledge.

Structure:

```markdown
# Project Quick Standards

## Story Format
<role + action + benefit, with one example from this project>

## AC Format
<chosen format: Gherkin or EARS — with project-specific example>

## Definition of Ready (DoR)
<checklist from governance.md, condensed to bullets>

## Definition of Done (DoD)
<checklist from governance.md, condensed to bullets>

## Task Split Pattern
<e.g. Story → Subtasks: [FE, BE, QA, Design] — used by /generate-subtasks>

## Naming Conventions
- Jira epic: <pattern>
- Jira story: <pattern>
- Confluence page: <pattern>
- File / artifact: <pattern>

## Glossary Anchors
<5-10 key terms with one-line definitions, sourced from charter and S&V>

## Open Items (TBD)
<things BA needs to confirm with team or client>
```

## Behavior rules

1. **Detect, don't impose.** Read the WBS for existing format clues. If client's WBS uses `[Module] Story title` — capture that.
2. **One AC format per project.** Ask BA to pick Gherkin or EARS before writing. Do not list both.
3. **TBD over invention.** If Task Split Pattern is not clear from inputs — write `TBD — confirm with PM` in the section. Do not invent FE/BE/QA combos.
4. **Concise.** Whole file should fit on one screen when collapsed in Project Knowledge. No prose explanations — bullets and one-line examples only.
5. **Glossary anchors are not a full glossary.** 5-10 entries max. Full glossary lives in `01_project_context.md` or separate doc.

## Example invocation

```
/quick-standards

Inputs:
- WBS: [attached or linked]
- governance.md and charter are already in Project Knowledge
- AC format: Gherkin (team uses BDD)
- Task split pattern: Story → [FE, BE, QA]
```

## Anti-hallucination

If any of the following is unknown — write `TBD` and flag in `## Open Items`:
- Naming conventions not visible in WBS or governance
- Task Split Pattern not specified by PM
- Specific DoR / DoD items not in governance

Never invent specific field names, role tags, or process steps not present in inputs.
