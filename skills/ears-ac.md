---
name: ears-ac
description: "Write Acceptance Criteria in EARS for an approved user story, in the project house style: numbered criteria with a short group Title, a Data Dictionary for forms, screen references, and the story's open questions beneath it. Minimal by default — add optional sections (Data Dictionary, Out of Scope, Permissions, Pre/Post-conditions) only where they add clarity. Behaviour only, no UI description. Source of truth is the Product Context. Use after stories are decomposed and approved, before refinement."
---

# /ears-ac

# Purpose

Write Acceptance Criteria in EARS notation for an approved user story (or set), in the project house style. AC content comes from the Product Context (authoritative); the WBS raw AC is a seed.

# When to invoke

After stories are decomposed and approved (decompose-wbs-epic), to write their AC before refinement.

# Inputs

1. The approved story/stories — each with its statement and parent WBS reference.
2. Product Context — /mnt/project/Product_Context_actual.md. The authoritative source. Read it as a whole: for every story and every criterion, search the entire document — a rule relevant to the story may live in any section, not only the one matching the epic's name. Where the Product Context and the WBS raw AC differ, the Product Context is the resolved state — raise the difference as an open question. Set `TBD` only after a full pass over the Product Context has found nothing, never after checking one section.
3. WBS — /mnt/project/WBS_Scope_only_1.xlsx, sheet WBS & Development Efforts — the parent story's raw AC as a seed.
4. Standards — /mnt/project/04_standards_file.md.

# EARS patterns

| Pattern | Structure |
|---|---|
| Ubiquitous | The system shall [response]. |
| Event-driven | When [trigger], the system shall [response]. |
| State-driven | While [state], the system shall [response]. |
| Optional feature | Where [feature is present], the system shall [response]. |
| Unwanted behaviour | If [condition], then the system shall [mitigation]. |

Clause order: While -> When -> the system shall.

# Minimal by default

A story carries only what it needs. **Always:** the Story statement, the numbered Acceptance Criteria, and Open Questions. **Only when they add clarity:** Data Dictionary, Out of Scope, Permissions, Pre/Post-conditions. Never add an empty or "n/a" section. Readability decides the shape, not the template.

# Output — one block per story

```
Story: As a <role>, I want <action>, so that <benefit>.

Acceptance Criteria

<Group heading>            (group only where it helps)
1. <Title> — <criterion>
2. <Title> — <criterion>

<Group heading>
1. <Title> — <criterion>

Data Dictionary            (only for a form with fields that have rules)
| Field Name | Field Type | Required? | Accepted Information | Comments |
|---|---|---|---|---|

Out of Scope               (only on a real dependency/overlap; numbered)
1. <what a reader expects here but lives elsewhere> -> see <story>

Open Questions             (numbered)
1. <Address> — <one atomic question; propose a default where sensible>
```

# Acceptance Criteria rules

- **Numbered markdown lists.** Use plain `1. 2. 3.` numbering so the rendering stays clean and each criterion is its own line. The counter restarts within each group (e.g. Steps 1-3, Validation 1-3); reference a criterion by its group and number ("Validation 3"). Number Out of Scope and Open Questions the same way.
- **Title first.** Begin each criterion with a short 2-3 word Title, then `—`, then the requirement (`Create account — When the user...`).
- **One shared outcome -> one criterion with listed sub-conditions.** When several criteria share the same outcome, write one criterion and list the conditions:
  `3. Verification error — The system shall show a verification error if: 1) the code is invalid; 2) the code is expired.`
- **Group only where it helps.** Plain-text group headings (Steps, Validation, ...) for a multi-faceted story; a simple story is one flat numbered list.
- **Behaviour / goal, not UI.** State what the system does or the outcome, never the interface — no controls, colours, menus, widget names, and no "the system displays the field/selector/screen". A screen reference is a pointer to where it happens, not a UI description.
- **Phrase from the user where simpler** (`the user can select ...`) rather than always `the system shall ...`.
- **Atomic and explicit.** One requirement per criterion. Enumerate roles/values explicitly; never let "or / and / but" mask several requirements or hide ambiguity.
- **Testable and measurable.** No vague "fast", "user-friendly". Give a concrete value/limit or mark `TBD`.
- **Provisioning** (default role, purchaser_type, default-group membership) -> event-driven criteria marked `(provisioning)`.

# Optional sections

- **Data Dictionary** — only for a form whose fields carry rules (required / accepted values / default / message). Columns exactly: Field Name, Field Type, Required?, Accepted Information, Comments; put the field's validation message and any default-when-empty in Comments. A field captured in the table is not repeated in the criteria — criteria keep only behaviour (submit, save, cross-field rules). A plain list of inputs with no per-field rules is not a table; a single field goes inline.
- **Out of Scope** — only when the story genuinely depends on or overlaps another; name what a reader would expect here that lives elsewhere and reference the owning story. Omit otherwise; never write `n/a`.
- **Permissions** — only when a criterion's behaviour depends on role; state the allowed roles on that criterion (this is a reference to roles, not an authorization matrix).
- **Pre/Post-conditions** — only for a state-driven flow where they aid clarity (e.g. activation: pre = a Pending account exists; post = the account is Inactive (Registered)).

# Conventions

- Write in **English**; use only Product Context glossary terms, one term per concept used identically everywhere.
- **No filler** — no back-references ("as mentioned", "above"), no rationale or commentary, no pronouns (it / he / she).
- Screen reference inline as `[Screen: <name>]`; unknown -> `[Screen: TBD]`. No "Design link" line. No emojis.
- Any unknown or unconfirmed value -> `TBD` in place, plus an Open-Questions line.

# Anti-hallucination

- AC content comes only from the approved story, the Product Context, the WBS seed, and the standards. Introduce no fields, messages, roles, or rules that are not there.
- Anything unknown or unclear -> `TBD`, with an Open-Questions line. Never guess a value, message, or rule.

# Example (abridged)

```
Story: As a Learner, I want to provide my sign-up details, so that I can create an account.

Acceptance Criteria

Steps
1. Create account — When the user submits sign-up details with all required fields valid and a unique email, the system shall create the account. [Screen: Sign-Up]
2. Defaults — When the account is created, the system shall set purchaser_type = B2C and assign the Learner role by default. (provisioning)

Validation
1. Required fields — If a required field is empty on submit, then the system shall keep the user on the Sign-Up screen and show that field's required message. [Screen: Sign-Up]
2. Accepted values — If a field value is outside its accepted information, then the system shall show that field's validation message. [Screen: Sign-Up]
3. Email uniqueness — If the email already belongs to an account, then the system shall block submission. [Screen: Sign-Up] TBD (message)

Data Dictionary
| Field Name | Field Type | Required? | Accepted Information | Comments |
|---|---|---|---|---|
| Email | input | Yes | valid format, contains "@", <= 50 | "Enter a valid email address" |
| Password | input | Yes | TBD security rules | TBD |
| Staff role | multi-select | No | from the Staff Roles list | TBD list |

Out of Scope
1. Email verification -> see Verify email.
2. Consent to Terms of Use & Privacy Policy -> see Agree to Terms of Use & Privacy Policy.

Open Questions
1. Client / Refinement — Confirm the canonical Staff Roles list for the multi-select.
2. Internal / Refinement — Define password security rules (min length, complexity); confirm against the Security epic.
```
