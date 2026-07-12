---
name: ears-ac
description: "Write Acceptance Criteria in EARS for an approved user story, in the project house style: numbered criteria grouped by meaning, a Data Dictionary for forms, one design link per story, a Surface note (per Product Context §2.4) per story, and the story's open questions beneath it. Presentation follows the reader's experience; a coverage pass plus a state-reachability filter mean no real case is missed and no unreachable one is invented. Uses must (not shall); bold block and group headers, TBD as a code chip. Complete on behaviour, free of ceremony. Source of truth is the current Product Context. Use after stories are decomposed and approved, before refinement."
---

# /ears-ac

# Purpose

Write Acceptance Criteria in EARS notation for an approved user story (or set), in the project house style. The output should be good enough that team refinement only confirms and fills `TBD`s — not rewrites. Content comes from the Product Context (authoritative), the flowcharts/designs, and the WBS seed.

# When to invoke

After stories are decomposed and approved (decompose-wbs-epic), to write their AC before refinement.

# Inputs

1. The approved story/stories — each with its statement and parent WBS reference.
2. Product Context — /mnt/project/Product_Context.md. The authoritative source. Read the **current** document as a whole and reconcile **every** criterion against it — a rule, value, message, role, or state relevant to the story may live in any section, not only the one matching the epic's name. Do not rely on memory of an earlier version. Where the Product Context and the WBS raw AC differ, the Product Context is the resolved state — raise the difference as an open question. Write `TBD` only after a full pass over the current Product Context finds nothing.
3. Flowcharts / designs — the design-level source for behaviour and screen names; concrete values seen on a mockup (e.g. code length, expiry) are real input, not guesses.
4. WBS — the parent story's raw AC as a seed.

# EARS patterns

| Pattern | Structure |
|---|---|
| Ubiquitous | The system must [response]. |
| Event-driven | When [trigger], the system must [response]. |
| State-driven | While [state], the system must [response]. |
| Optional feature | Where [feature is present], the system must [response]. |
| Unwanted behaviour | If [condition], then the system must [mitigation]. |

Use **must**, never *shall*. Clause order: While -> When -> the system must. Phrase from the user where it reads simpler (`the user can ...`).

# Presentation — write for the reader

How the criteria are arranged matters as much as their content. A reader should be able to move through them the way they move through the feature.

- **Order = the reader's experience.** Arrange criteria in the order the user and system meet them: first the context / entry (where this happens), then the happy-path steps in their natural sequence (selects X, then Y, then the completing action), then validation, then negative and edge cases. Do not scatter them.
- **A completing action never precedes the choice it depends on.** If a step ends with "Continue / Save / Submit", that criterion comes after the criteria for the inputs it acts on — never before them.
- **Group top-down.** For a multi-faceted story, give the reader the map before the detail: a high-level view first (where it happens, for which flows — often a small table), then one whole branch, then the next, without mixing levels. Group names follow the story's meaning (e.g. Sign in / Sign up, Entry / Resolution) — there is no fixed set like "Steps / Validation".
- **Choose the form by readability.** Several forms are available; pick the one that scans fastest for *this* material, and never lose per-item nuance (default state, role scope, cross-reference, exclusion) to make a form fit. These are options, not mandatory templates — uniform stamping kills a story's readability as surely as clutter does.
  - **Numbered criteria** — the default for ordinary behaviour.
  - **Sub-point list under one criterion** — N homogeneous items that share one outcome and need no per-item description (one criterion + a/b/c list). Example shape: "Sortable fields — the user can sort by: a) X; b) Y; c) Z."
  - **Reference table** — N homogeneous items where each has its **own** short outcome (item -> behaviour), two columns; reads faster than N near-identical criteria. Use whenever you find yourself writing several criteria of one template differing only in *which item* and *what it does* (filters and what each surfaces, status values and what each means, group type and its entitlement, and so on).
  - **Scenario matrix** — condition x result across dimensions (a table, but different from a reference table).
  - **Data Dictionary** — form fields with rules (fixed columns).
- **List vs table — the deciding test.** Same outcome shared by every item -> sub-point list under one criterion. A different outcome per item -> reference table. (Sort = one shared "sort by" -> list; Filter = each filter surfaces something different -> table.)
- **Branches stay visible, but may be grouped.** When one trigger splits into outcomes, show every branch — never hide them behind vague wording ("the system handles it"). Each branch is its own criterion, or a sub-point under one shared criterion, whichever reads better. Collapsing is wrong only when it loses a branch (e.g. dropping the Region step of a two-level selection), not when it groups cleanly.

# Coverage pass — write the full picture, not just the happy path

Most gaps found in refinement are missing cases, not bad wording. Before finalising a story, walk these five and write a criterion for every case the flow can actually reach (see the reachability filter next):

1. **Happy path + forward outcome.** The main success flow, ending with what happens next — advance to the next step, route home, save and continue. The terminal/forward outcome is the single most-forgotten criterion.
2. **Alternative branches.** Where one trigger splits by tier, segment, account type, or input state, surface each branch (own criterion or sub-point) — never blur them into one vague rule.
3. **Negative / error states.** Invalid input; expired / invalid / already-used link; provider error or user cancel; a blocked account state — with the client-approved message from the current Product Context; save/network failure (usually a platform-level pattern -> open question, not invented here).
4. **Edge cases.** The optional/empty path; single-use link; resend cap + cooldown; back-navigation and field persistence across steps; empty search result; an upstream change that must reset a downstream choice; an already-completed action (e.g. an already-used activation link routes to login).
5. **Role / permission.** Only where a role actually gates the behaviour.

This is completeness of behaviour, not bulk — every criterion must still earn its place and pass the reachability filter.

# State-reachability filter — run on every conditional criterion

Before writing a criterion about a state or condition, confirm the user can physically reach that state in **this** flow. If they cannot, do not write it.

- Derive reachability from the state's own definition in the Product Context; do not copy a state pair from one flow into another out of habit.
- A criterion that only describes a state the user can never be in on this screen is noise; drop it.

# Decided vs proposed vs open — never invent behaviour as fact

When information is missing, do not make it up.

**Decision gate — run on every criterion before writing it.**

1. **Full pass over the current Product Context first.** If the PC answers the point — in any section, not only the one matching the epic — it is **decided**: write a plain criterion, no tag, no question.
2. **PC silent, but the call is the team's to make** (a standards default, an SA modelling detail, a routine BA judgment that no client need depends on) — it is an **internal decision**: bake it as a plain criterion, no tag and no open question. Never route a decision you own to the client.
3. **PC silent and the point is a genuine product / client unknown** — only now does it become a question: write the criterion with a proposed answer ending `(proposed — confirm)` and add the matching open question; or, where no sound answer exists, an open question alone.

`(proposed — confirm)` marks case 3 only — a proposal awaiting a client confirm. It is never the default for anything reasoned out internally; cases 1-2 carry no tag.

- **Decided** — stated in the Product Context, a flowchart, or a mockup: write a plain criterion.
- **Unknown value or copy** (a duration, a message text) where the behaviour itself is decided: write the criterion with `TBD` in place and add the matching open question.
- **Unknown whether the behaviour is even in scope** (you cannot put a `TBD` value on it, because the rule itself is unknown): write an open question only, no criterion.
- **Proposed** — sound reasoning not stated in any source. If the call is the team's to make, write a plain criterion with no tag and no question (gate case 2). If it pre-answers a genuine product / client point, write the criterion ending `(proposed — confirm)` and add the narrow confirm as an open question (gate case 3); the working answer goes in the AC, the confirm in the questions.
- **Derived technical / security mechanism** the client would never state as a need (e.g. how the platform treats an unverified provider email): this is system design, not a product requirement — surface it as an open question and note it likely belongs to the Security / relevant epic, not as a product AC here.
- **Non-functional requirement** — performance, scalability, load / throughput, response time, availability. A "how fast / how many / under what load" concern is never a functional criterion: do not write it as an AC. It may be parked in Open Questions, flagged as an NFR, so it is not lost (NFRs are handled separately, outside this story's AC). Functional empty/error states stay in the AC; the "how fast / how many / under what load" part leaves.

# Output — one block per story

Block and group headers in **bold**; the design link given once near the top, never per criterion; every `TBD` written as a `` `TBD` `` code chip.

```
**Story:** As a <role>, I want <action>, so that <benefit>.

**Design:** <one Figma link, or several if the story spans screens>

**Surface:** <one-line note per actor, from Product Context §2.4 — e.g. `User front-end (Group Owner, Group Leader)`; use only approved surfaces, never invent>

**Pre-conditions:** <only for a state-driven flow that needs them>

**Acceptance Criteria**

**<Group name>**            (group by meaning, only where it helps; high-level first)
1. <Title> — <criterion>
2. <Title> — <criterion>

**<Group name>**            (numbering continues, does not reset)
3. <Title> — <criterion>

**Data Dictionary**         (only for a form with fields that have rules)
| Field Name | Field Type | Required? | Accepted Information | Comments |
|---|---|---|---|---|

**Related stories**         (parts of the flow owned by sibling stories; numbered)
1. <part of the flow that lives elsewhere> -> see <story>

**Out of Scope**            (only what is deliberately not done; numbered)
1. <what could be expected here but is intentionally excluded / Future Phase / not collected>

**Open Questions**          (numbered, plainly written)
1. <one atomic question; propose a default where sensible>
```

# Surface

- **Surface note — mandatory, one per story.** Every story carries a one-line Surface note directly under Design, naming the surface(s) it renders on, scoped per actor. Use only the approved surfaces defined in Product Context §2.4 (Admin panel / User front-end, plus the External and none qualifiers); never invent a new surface.

# Acceptance Criteria rules

- **Numbered markdown lists, continuous across the story.** Plain `1. 2. 3.`, one criterion per line. The counter runs continuously through all Acceptance Criteria groups (e.g. Sign-up 1-3, then Validation 4-8) so every criterion has a unique number; reference a criterion by that number. Group headings organise the list but do not reset the count. Related stories, Out of Scope, and Open Questions are each their own list, numbered from 1.
- **Title first.** Begin each criterion with a short 2-3 word Title, then `—`, then the requirement (`Create account — When the user...`).
- **One shared outcome -> one criterion with listed sub-conditions.** `Verification error — The system must show a verification error if: 1) the code is invalid; 2) the code is expired.`
- **Behaviour / goal, not UI.** State what the system does or the outcome, never the interface — no controls, colours, menus, or widget names, and no "the system displays the field/selector/screen". A screen is named in prose only where needed to disambiguate, with no inline link.
- **Atomic and explicit.** One requirement per criterion; enumerate roles/values/branches explicitly; never let "or / and / but" mask several requirements or hide ambiguity.
- **No vague verbs.** A criterion states exactly what is and is not allowed — never a blanket "restrict / manage / handle / control". If a user is limited, spell out precisely what they can and cannot do (e.g. not "restrict a banned user from posting" but "a banned user cannot publish articles or comments; existing posts stay visible").
- **Testable and measurable.** No vague "fast", "user-friendly". Give a concrete value/limit or mark `TBD`.
- **No meta criteria.** Do not write a criterion that just points at the Data Dictionary ("validate the fields per the table") — the table plus a single "outside its accepted information" criterion cover it.
- **Behaviour belongs in criteria.** If any line describes what the system does, it is a numbered criterion — never prose in the Story preamble or anywhere else. Every piece of content has one home: behaviour -> a criterion; an excluded/Future-Phase item -> Out of Scope; a part of the flow owned elsewhere -> Related stories; a scope condition -> a clause inside the criterion it qualifies (`Where the flow is B2C, ...`). There is no catch-all "Note" block.
- **No inline cross-references.** A criterion never points to a sibling story inline (no `-> see <story>` beside a criterion). A part of the flow, rule, or behaviour owned by another story or PC section is referenced only in the Related stories block — never duplicated as a criterion here.

# Optional elements — only when they add clarity, never empty

- **Data Dictionary** — only for a form whose fields carry rules (required / accepted values / default / message). Columns exactly: Field Name, Field Type, Required?, Accepted Information, Comments; put the field's validation message, any default-when-empty, and the UI label (when it differs from the canonical name) in Comments. A field captured in the table is not repeated in the criteria — criteria keep only behaviour (submit, save, cross-field rules). A plain list of inputs with no per-field rules is not a table; a single field goes inline.
- **Related stories** — when parts of this flow are owned by sibling stories (verification, consent, affiliation, a shared step), list them as `-> see <story>` so the reader can find the rest of the flow. These are dependencies, not exclusions.
- **Out of Scope** — only what is deliberately not done: not collected, Future Phase, or explicitly excluded (e.g. "Username is not collected"; a field deferred to a later phase). Never list sibling stories here — those go under Related stories. Omit when nothing genuine applies; never write `n/a`.
- **Call-sites** — for a shared/canonical story (consent, verification, staff role), list the flows that reuse it and the flows that explicitly do not.
- **Permissions** — only when a criterion's behaviour depends on role; state the allowed roles on that criterion (a reference to roles, not an authorization matrix).
- **Pre/Post-conditions** — only for a state-driven flow where they aid clarity (e.g. activation: pre = a Pending account exists; post = the account is Inactive (Registered)).

# House style / formatting

- **must**, never *shall*.
- **Block and group headers in bold** — blocks (Story, Design, Pre-conditions, Acceptance Criteria, Data Dictionary, Related stories, Out of Scope, Open Questions, Call-sites, Post-conditions) and the meaning-based AC group names alike. No underline or colour (they do not survive a Notion paste).
- **Every `TBD` as a code chip** — write `` `TBD` `` so it stands out and survives the paste; never a bare or buried "tbd".
- **One Design link near the top**, never repeated under each criterion. If the story spans several screens, list them once in the Design block.
- **No `(provisioning)` tag** — system-side defaults (purchaser_type, default role, default group, seat decrement) are written as ordinary event-driven criteria, no tag.
- **Open Questions are plainly written** — a clear question the reader understands, with a proposed default where sensible. Do not prefix an owner or route ("Internal", "Client", "Refinement") — the BA assigns the owner afterwards.
- English; one term per concept used identically everywhere; no filler ("as mentioned", "above"), no rationale/commentary inside a criterion, no pronouns (it / he / she); no emojis.

# Anti-hallucination

- AC content comes only from the approved story, the current Product Context, the flowcharts/designs, and the WBS seed. Introduce no field, message, role, branch, or rule that is not there.
- Anything unknown or unclear -> `` `TBD` `` plus an Open-Questions line, or a proposed criterion marked `(proposed — confirm)`. Never present a guess as decided.

# Example (abridged)

```
**Story:** As a Learner, I want to provide my sign-up details, so that I can create an account.

**Design:** Figma — Sign-Up

**Surface:** User front-end (unauthenticated)

**Acceptance Criteria**

**Sign-up**
1. Enter details — The user enters their first name, last name, email, and password.
2. Create account — When the user submits the form with all required fields valid and a unique email, the system must create a Pending account and start email verification.
3. Defaults — When the account is created, the system must set purchaser_type = B2C, assign the Learner role by default, and add the user to the default backend group.

**Validation**
4. Required fields — If a required field is empty on submit, then the system must keep the user on the form and show that field's required message.
5. Accepted values — If a field value is outside its accepted information, then the system must show that field's validation message.
6. Email collision — When the email already belongs to an account, the system must block submission and: 1) for an active account, show "This email is already associated with an account. Please check the email address or sign in to continue."; 2) for a pending B2B invite, re-send the activation link instead. `TBD` (pending-invite message)

**Data Dictionary**
| Field Name | Field Type | Required? | Accepted Information | Comments |
|---|---|---|---|---|
| Email | input | Yes | valid format, contains "@", <= 254 characters | "Enter a valid email address" |
| Password | input | Yes | per the password security rules | `TBD` — see Open Question 2 |
| First name | input | Yes | >= 2 and <= 50 characters | empty: "First name is required" |

**Related stories**
1. Email verification -> see Verify email.
2. Consent to Terms of Use & Privacy Policy -> see Agree to Terms of Use & Privacy Policy.
3. State and compliance framework affiliation -> see Set state and compliance framework affiliation.

**Out of Scope**
1. Username is not collected.
2. Marketing attribution ("How did you hear about us?") -> Future Phase.

**Open Questions**
1. Validation trigger timing: on submit, on field blur, or live? Proposed: validate on blur, re-check on submit, block submit until valid.
2. Provide the password security policy (min length, complexity, max length, breach check, weak-password message). Until defined, Password stays `TBD`.
```


# Final gate — run mechanically before delivering (never skip on large batches)

Before delivering a story's AC, verify each — fix the output until all pass:
1. Numbering is continuous across all AC groups (groups organise, never reset the counter); Related stories / Out of Scope / Open Questions each restart at 1.
2. A **Surface:** line is present under Design.
3. Data Dictionary present if the story has a form with field-level rules; no field captured in the table is repeated in the criteria.
4. Every `TBD` is a code chip and has a matching Open Question.
5. Every `(proposed — confirm)` has a matching Open Question; no orphan tag.
6. No related-story reference appears inline in a criterion — sibling-owned flow lives only in the Related stories block.
7. No NFR (how fast / how many / under what load) is written as a functional criterion.
8. Each criterion is atomic — no or/and hiding multiple requirements or branches.
