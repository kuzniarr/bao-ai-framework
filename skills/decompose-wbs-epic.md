---
name: decompose-wbs-epic
description: "Decompose a WBS epic into smaller, independent, vertical user stories (INVEST), each written as a full 'As a role, I want, so that' statement and grouped under its parent WBS story. Reads the WBS spreadsheet and Product Context, names the splitting pattern used, surfaces split decisions and WBS-vs-PC gaps for BA review. Use when breaking a WBS epic into refinement-ready stories, before acceptance criteria."
---

# /decompose-wbs-epic

## Purpose

Turn one WBS epic into smaller, valuable **vertical** user stories — INVEST-compliant — each traceable to the parent WBS story it came from. Story statements only; acceptance criteria are added afterwards by `ears-ac`, and the story-map xlsx is produced by a separate skill.

## When to invoke

When the BA wants to break a WBS epic into implementation-ready stories, before writing AC. Works on any epic in the WBS.

## Inputs

The BA names the epic. Read these automatically.

1. **WBS** — `/mnt/project/WBS_Scope_only_1.xlsx`, sheet **`WBS & Development Efforts`**.
   - Header is on **row 3**. Relevant columns: `Topic` (A), `User Story` (B), `Acceptance Criteria` (C), `Phase` (J).
   - An **epic** = a `Topic` header row (the UPPERCASE name, e.g. `USER REGISTRATION`) followed by its **parent-story rows** (each with a `User Story` value) until the next Topic header.
   - From each parent-story row take: the parent **story title** (the short `Topic`/`User Story` name), its **raw AC** (`Acceptance Criteria`), and `Phase`.
   - Extract with code, e.g.:
     ```python
     import openpyxl
     wb = openpyxl.load_workbook('/mnt/project/WBS_Scope_only_1.xlsx', data_only=True)
     ws = wb['WBS & Development Efforts']
     # from row 4: an UPPERCASE Topic with empty User Story = epic header;
     # the rows after it (Topic = short story name, User Story populated) are its parent stories,
     # up to the next header.
     ```
2. **Product Context** — `/mnt/project/Product_Context_actual.md`. The authoritative source for content. Read it as a whole: for every story search the entire document — a rule relevant to the story may live in any section, not only the one matching the epic's name. Where the Product Context and the WBS raw AC conflict, the Product Context wins.
3. **Standards** — `/mnt/project/04_standards_file.md` for the story format and DoR expectations.

## Process

For the named epic, take each parent WBS story in turn:

- If it is already an atomic, INVEST-compliant vertical slice, keep it as **one story** and say so in a line.
- Otherwise decompose it into child stories, each a full statement: `As a <role>, I want <action>, so that <benefit>.`

Group every resulting story under its parent WBS story title (this grouping is the traceability link to the parent). Then add **Split flags** and **Gaps & conflicts**, and return the whole thing as markdown.

## Decomposition guidelines

- Split **vertically**: each story delivers one complete, user-visible piece of value end-to-end — the UI, the logic behind it, and the data it needs, together — rather than a single technical layer on its own.
- Apply **INVEST** (Independent, Negotiable, Valuable, Estimable, Small, Testable).
- Name the splitting pattern used. Common ones:
  1. **Workflow Steps** — sequential stages of one flow (e.g. provide details → verify → agree to terms).
  2. **CRUD Operations**.
  3. **Business Rule Variations** — different rules or branches (often a role-based split, see Role logic).
  4. **Data Variations**.
  5. **Data Entry Methods (UI)**.
- When a candidate child is trivial or naturally belongs inside another, raise it as a **Split flag** for the BA to decide (e.g. "auto-redirect to welcome page could fold into the first story").
- **Shared steps → one canonical story.** When the same behaviour recurs across several flows in the epic (e.g. consent to Terms, email verification), create **one** canonical story for it and have the other flows reference it as an AC step — never duplicate it as a child in each flow. (This catches duplicates born *inside* the decomposition, not only duplicates of another WBS parent story.)

## Story statement rules

The `want` must be clean — detail lives in the AC, not in the statement.

- **One `want` = one user intention.** No compound actions ("open the link **and** reach the page", "activate **and** set a password"). Split them or pick the single real intention.
- **State the genuine goal, in active voice.** Name what the user wants to achieve (`create an account`), not the mechanical sub-action (`provide details`), and never passive/system-driven phrasing (`be taken to`, `be routed to`).
- **Keep conditions, optionality, options, field lists, and UI details out of the statement** — they are Acceptance Criteria. So: not `(optionally)`, not `(with a "Remember me" option)`, not `when the org has subgroups`, not `with a code`.
- **Automatic system outcomes are not stories.** Auto-redirects, default-role assignment, and similar happen without the user acting — they are AC of the triggering story, not stories of their own.

## Role logic

WBS stories are written generically as `As a User`. Resolve the real role from the Product Context role list (`System Admin · Admin · Group Owner · Group Leader · Learner`):

- If behaviour differs by role, split into separate stories per role (Business Rule Variation).
- If behaviour is the same across roles, write **one** story naming the roles together: `As a System Admin and Admin, I want …`.
- One role-context per story otherwise.
- If the correct role cannot be determined from the WBS and Product Context, write the role as `TBD` and list it under Gaps.

## Gaps & conflicts

Surface these for BA review rather than resolving them silently:

- **WBS↔PC divergence** — where a parent's raw WBS AC differs from the Product Context (field optionality, defaults, added or removed steps, scope), note it; the Product Context is the resolved state. Flag a divergence **only when it changes a field, rule, or scope decision**. Treat pure **renames** (e.g. a role label changed) as silent mapping — map to the current term and move on, no Gaps line.
- **Dependency / duplicate** — where a child slice duplicates or depends on another parent WBS story in the same epic (e.g. a verification step that also exists as its own WBS story), flag it instead of creating a duplicate story.
- **Role TBD** — any unresolved role.
- **Missing** — anything the Product Context is silent on; write `TBD`.

## Anti-hallucination

- Use only the epic's WBS rows, the Product Context, and the standards. Introduce no roles, entities, flows, or fields that are not there.
- Anything unknown or unclear → `TBD`, listed under Gaps.
- Use only Product Context glossary terminology.

## Output format

```
# Decomposition — <Epic name> (WBS Topic: <TOPIC>)

## <Parent WBS story title>
Pattern: <pattern(s)>
1. As a <role>, I want <action>, so that <benefit>.
2. As a <role>, I want <action>, so that <benefit>.

## <Parent WBS story title — kept whole>
Already an atomic slice; kept as one story.
As a <role>, I want <action>, so that <benefit>.

## Split flags
- <merge / boundary decisions for the BA>

## Gaps & conflicts
- WBS↔PC: <divergence; PC is the resolved state>
- Dependency/duplicate: <…>
- Role TBD: <…>
- Missing: <… TBD>
```

Return as markdown.
