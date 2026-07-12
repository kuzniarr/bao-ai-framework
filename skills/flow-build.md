---
name: flow-build
description: "Build editable stakeholder-review flowcharts for an epic in FigJam, one section per flow, from the delivery roadmap (which flows) and the Product Context (the flow content). Use when the BA wants to generate flows/flowcharts for review — e.g. 'збудуй флоучарти для епіку', 'зроби флоу для Course Creation', 'намалюй флоу CRS у Figma', 'build the flowcharts for this epic', 'flow-build for GRP'. The BA pastes the roadmap and names the epic + Flow ID prefix; this skill filters the roadmap to that prefix, derives each flow strictly from the Product Context (PC silence → OQ, never invented), and writes one FigJam file with one titled section per flow. This is the counterpart to design-validation: design-validation checks an existing flow against the PC; flow-build generates the flow from the PC. Does NOT validate an existing design and does NOT edit the PC."
---

# /flow-build

Generate editable stakeholder-review flowcharts for one epic into a single FigJam file — one section per flow. This skill writes to Figma; it does not validate or edit requirements.
## When to use vs design-validation
flow-build (this skill): no flow exists yet — generate it from the Product Context.
design-validation: a flow already exists (PDF) — check it against the PC, report findings only.
Comments coming back on a flow built here are handled by Refine, not here.
## Inputs (per run)
Roadmap — the full delivery roadmap, pasted by the BA (CSV, schema: Flow ID (Name) · Flow Name · Delivery Readiness · Budget-Fit Delivery Level · Notes/Assumptions). This is the flow registry — which flows exist. It is NOT a content source.
Epic + Flow ID prefix — named by the BA (e.g. "Course Creation, CRS"). The prefix is how flows are selected from the flat roadmap; do not infer it — the BA states it.
Product Context (PC) — the single source of truth for flow content: roles (canonical names), permissions, states, transitions, business rules, scope, glossary. Read access required.
Existing draft (optional) — a per-flow draft (usually PDF) the BA may attach. When present, it feeds the "what's right / what to fix" line of the flow brief (Step 3). It is a draft to check against the PC, not a content source: where the draft asserts a rule the PC does not state, flag it as OQ (draft ahead of PC) — never adopt it silently.
Nothing else is a content source. Do not use discovery, WBS, PRD, stakeholder files, or older PC copies for content.
Flow set. Normally the flow set = roadmap rows matching the prefix (Step 1). Alternatively the BA hands a specific set of flows/drafts for the session; then build exactly those, skip the prefix filter, and still derive content from the PC.
## Output
One editable FigJam file for the epic, containing one separate section per flow — never one giant connected diagram, never unrelated flows connected. Each section independently readable.

## Procedure
### Step 1 — Select the flows (roadmap, deterministic)
From the pasted roadmap, take every row whose Flow ID starts with the given prefix. Then apply the status filter:
Draw the flow if Delivery Readiness is anything other than the three below.
Skip the flow if Delivery Readiness ∈ { Out of MVP / Future, Blocked / Approach TBD, Internal Reference Only }.
Covered by Parent / Related Flow → still draw it. That column describes mockup reuse, not flow logic; the flow is distinct logic and needs its own flowchart.
Echo the resulting list to the BA before any Figma call (this doubles as the approval gate):
Epic: <name> (prefix <PREFIX>)
Will build (N):
  <FLOW-ID> · <Flow Name> · <Delivery Readiness>
  ...
Skipped (M):
  <FLOW-ID> · <Flow Name> · <reason: Out of MVP / Blocked / Internal Reference>
Wait for "ok" on the list before moving to the per-flow briefs. (For a draft-driven session, echo the handed set instead of the prefix filter.)
Step 2 — Derive each flow from the PC (PC-first)
For each selected flow, before drawing, build the flow from the PC — do not draw from the roadmap Flow Name alone:
Primary actor + supporting/escalation actor(s), canonical role names only (no simplified/client-facing renames).
Role scope / permission boundary (own org / own group / own subgroup / assigned records / platform-wide / requires approval / escalate to higher role) — always shown explicitly for scoped roles.
Entry point(s) and exit/outcome(s).
States and transitions the flow must reflect.
Decision points and business rules that govern branches.
For each role in a multi-role flow: what happens without permission (can view only / cannot edit / must escalate / system blocks / higher role required). Do not assume the highest role's permissions apply to all.
Read the roadmap Notes/Assumptions cell for the flow: a "reuses X" / assumption note becomes a yellow scope-note on the flow (e.g. "reuses USR-01 form"). This is the one thing pulled from the roadmap into content — as a note, never as logic.
PC silence is never filled. Anything the PC does not state → OQ: Needs validation note attached to the relevant step. Never invent screens, permissions, states, or rules. If two PC statements conflict, do not resolve silently → Note: Needs validation.
Step 3 — Brief each flow in text, then wait for "ok" (per flow)
Before drawing anything, present each flow as text in this fixed format, one flow at a time, and wait for the BA's "ok" on that flow before generating it. Ground the brief in the PC (cite the sections used); use the draft (if attached) only for point 3.
Roles — actors in this flow (canonical names) + their scope.
Start and end events — the true entry and the terminal outcome(s).
Draft: what's right / what to fix — only when a draft is attached; otherwise "no draft". Where the draft asserts a rule the PC is silent on → OQ (draft ahead of PC).
Happy path — step by step, brief.
Alternative scenarios — errors, denials, expired/blocked states, branches.
Open questions — what the PC does not settle.
Default cadence: one flow at a time (brief → ok → draw → next). Only batch all briefs first if the BA asks.
Step 4 — Build in FigJam
Resolve planKey first: if the BA provided a team/org key, use it verbatim; otherwise call Figma:whoami, and if there is more than one plan, ask which to use before writing. Confirm target workspace/project if unset.
Orchestration — create the file first (generate_diagram never returns a usable fileKey, so it cannot seed the file; create_new_file is the reliable way to get one file that every flow lands in):
Create the file once: Figma:create_new_file(editorType="figjam", fileName="<Epic> — Flows", planKey=…) → capture file_key from the response.
Each flow: Figma:generate_diagram(fileKey=<file_key>, mermaidSyntax=…, name="<Flow ID> · <Flow Name>") — one call per flow = one section, all in the one file. Draw a flow only after its brief was approved in Step 3.
If create_new_file fails or FigJam is unavailable, stop and tell the BA — recommend a plan/seat with edit rights. Do not scatter flows across separate files, and do not silently fall back to a Claude artifact or raw-Mermaid-only output.
Return the FigJam link when done.
Step 5 — Reverse-gap pass against the PC
Steps 2–4 derive each selected flow from the PC. This step runs the check the other way: does every relevant PC rule for the epic have a home in some flow? It catches epic logic that fell through because no flow happened to cover it (the kind of miss that otherwise only surfaces when the BA asks "did we cover everything?").
After the flows are built:
Re-read the PC sections for this epic and list the concrete rules/behaviours they state (by rule ID / anchor where the PC has them).
For each, check whether it landed in a built flow (a step, branch, decision, or note).
Report the ones that did not as a short Coverage gaps list — each with the PC reference and where it would belong (which flow to patch, or "needs its own flow / roadmap check").
Do not silently patch or invent. Present the gaps; the BA decides what to patch, raise as an OQ, or route as a roadmap/other-epic check.
Keep it to genuine misses — a rule that clearly belongs to another epic's flow is a boundary note, not a gap.
Fallback rule
If editable FigJam creation is unavailable entirely, stop and tell the BA — do not silently switch to a Claude artifact or Mermaid-only output.

Diagram rules
Build stakeholder-review flowcharts, not technical architecture diagrams. A reviewer should follow each happy path in 5–10 seconds and be able to add comments to a branch without untangling arrows.
Title + metadata (per flow)
Wrap the whole flow in a Mermaid subgraph whose label is the title: <Flow ID> · <Flow Name> (the subgraph label renders as the on-canvas title).
Directly under the title, one single-line metadata note node — only Actor + Scope (fields joined by --; drop Source and Status, they add noise). For a multi-surface flow, fold the surfaces into Scope (e.g. Scope: Admin panel + User front-end):
Actor: … -- Scope: …
Style it :::scope.
Shapes
Start event → plain circle id(("Start …")); End event → filled circle styled :::endev (green fill) so terminal outcomes stand out. Circles are for the true entry and terminal events only (e.g. Start, END, User is logged in, Learner Active); intermediate states/actions are rectangles, never circles. Do not use the stadium/pill (["…"]) for start/end.
User action → rectangle ["…"]. One step = one action. Where two sequential actions were joined with + / & / "and", split them into a chain (A --> B), not one node. Do not split a list of attributes/fields into separate steps — an attribute-setting step (e.g. "Assign categories, tags, duration") stays a single node.
System action → rectangle, System: prefix.
Decision / gateway → diamond, phrased as a question {"…?"} (e.g. Is Link Valid?, account state?, B2B or B2C?). The node text is always a question; the branch labels are the answers (Yes / No / Valid / Expired).
Permission denial / blocked → a normal branch in the flow (its own step/message node); no special colour.
Destructive action (reset / delete / remove) → a Confirm …? decision with an explicit terminal Cancel — no change branch.
Cross-role handoff → rectangle Handoff to [Role].
External step → rectangle Off-platform: ….
Colors (paste this classDef into every diagram)
Two note colours only. Red = questions / open questions. Yellow = scope notes and explanatory notes (rules, assumptions, "reuses X"). There is no gray — denial/blocked steps are normal flow nodes. Green is not a note colour; it only fills the terminal end event.
classDef oq fill:#FBE0E0,stroke:#D14343,color:#7A1F1F;
classDef scope fill:#FFF4CC,stroke:#E6B800,color:#665600;
classDef endev fill:#CDEBD6,stroke:#3C7D4F,color:#1E4D2B;
Layout (per flow)
Mermaid direction LR by default; happy path left-to-right, entry top-left, outcomes bottom-right. Large flows may continue top-to-bottom in clean rows. Avoid bottom-up.
Alternative paths, errors, denials, expired states, validation branches → below the related happy-path step, each readable as a near-independent mini-flow.
Avoid long diagonal / crossing connectors and central hub nodes. If one block needs many arrows, duplicate the block in the relevant branch instead — duplicated blocks are fine, unclear arrows are the problem.
Keep whitespace around branches for later comments; prefer readable width over density. Split a flow into subflows if it gets too large to review comfortably.
Put all Mermaid shape/edge text in quotes (["Text"], -->|"Edge"|). No emojis. No literal \n. Do not use the word end in classNames.
Notes discipline
Short labels inside flow-step blocks (1–2 lines). This limit does not apply to side notes — an OQ or scope note may be as long as it needs. Move explanations / assumptions / OQs into side notes beside the step they relate to — never between two main-flow steps.
OQ notes are written as real, client-ready questions, not flags. Give the actual question, and where useful why the decision matters + the options at stake (e.g. "Can the enrollment mode be changed after learners are enrolled, and what happens to existing enrollments?"). Do not write meta-phrases like "PC is silent", "not in PC", "needs validation" — the artifact is client-facing; just ask the question.
When an OQ in one flow affects a rule or step in another flow, say so inside the OQ (e.g. "…must line up with Mark Completed, which is Admin/SA only").
Include only notes that affect flow logic; do not dump every roadmap note in.
Business-readable language, not engineering-only.
Optional overview frame
If useful, a small overview frame at the start of the file: selected epic, included flows, main actors, key permission boundaries, main open questions, MVP/Future notes. Keep it short.

Anti-hallucination
Content comes from the PC only. PC silence → OQ: Needs validation. Conflict → Note: Needs validation. Never invent screens, permissions, states, roles, or rules.
Canonical role and glossary names only — never rename/merge/reclassify roles or groups.
Flow selection comes from the pasted roadmap + BA's prefix only — never infer a different prefix or top up flows the roadmap doesn't list.
Unknown Figma planKey / workspace → ask, do not guess.
