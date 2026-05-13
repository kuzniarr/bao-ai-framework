---
name: elicitation-prep
trigger: /elicitation-prep
phase: 2 · Discovery & Elicitation
output: questions_list.md — ready to use in the meeting or share with client
---

# Elicitation Prep

Generates a structured question list for a client meeting or workshop. Questions are grouped by topic and prioritized by what needs to be resolved before requirements can be written. Prevents blank-page paralysis before sessions and ensures nothing critical is missed.

Reads project context from Project Knowledge automatically. Add the meeting topic or focus area in the prompt, that's all that's needed.

---

## When to Use

- Before any client meeting where requirements or scope will be discussed
- Before a refinement session where a feature is not yet clear enough
- When preparing for a discovery call on a new epic or integration
- When you have a blank page and don't know where to start

---

## Inputs

- Project context (auto-read from Project Knowledge)
- Scope parameter — **required**:
 - `project-wide` — general gaps across full project
 - `epic:<epic-name>` — questions specific to one epic
 - `sprint:<sprint-id>` — questions for upcoming sprint scope
 - `topic:<topic>` — focused topic (e.g. payments, auth)
- Optional source artifacts: S&V section, WBS epic rows, Confluence page URL

## Output filename pattern

`questions_list_<scope>.md`

Examples:
- `questions_list_project_wide.md`
- `questions_list_epic_user_management.md`
- `questions_list_sprint_3.md`
- `questions_list_topic_payments.md`

---

## Steps

**Step 1, Read project context**

Read `01_project_context.md`, `02_stakeholders.md`, `03_tech_context.md` from Project Knowledge. Extract:
- Product domain and goals
- Scope boundaries (what's in, what's out)
- Relevant stakeholders for this session
- Known technical constraints related to the topic

**Step 2, Determine session scope**

Based on the topic provided, determine:

| Session type | Signals | Focus |
|---|---|---|
| Kickoff | "kickoff", no specific feature | Broad, product vision, goals, users, process, constraints |
| Feature / Epic discovery | Feature or epic name provided | Mid-level, functional scope, flows, roles, edge cases |
| Integration deep-dive | Integration name (e.g. "X Ads") | Narrow, connection flow, API constraints, error handling, permissions |
| Refinement prep | "refinement", story name | Narrow, AC gaps, edge cases, technical feasibility questions |

**Step 3, Generate question list**

Generate questions grouped by topic. For each group:
- List questions in priority order, most blocking first
- Mark questions that must be resolved before writing requirements as `[MUST]`
- Mark nice-to-have clarifications as `[GOOD TO HAVE]`
- Mark questions that can be resolved async (not in the meeting) as `[ASYNC OK]`

**Step 4, Add session prep notes**

After the question list, add a short section:
- What to prepare or send to the client beforehand (if anything)
- Who should be in the room (based on `02_stakeholders.md`)
- What the expected output of this session is

---

## Output

Markdown file named per pattern above. Structure:

```markdown
# Elicitation Questions — <scope>

## Context
<1-2 sentences — what scope these questions cover>

## Questions by topic

### <Topic 1>
1. <question> — priority: <high/med/low>
2. ...

### <Topic 2>
...

## Open assumptions
<things skill assumed when generating — BA should validate>
```

---

## Kickoff Mode, Additional Topics to Cover

When session type is **kickoff**, always include question groups for:

- Product vision and business goals
- Target users and their roles
- Scope (what's in, what's explicitly out, what's TBD)
- Existing system or legacy (is this greenfield or existing product?)
- Key constraints (deadline, budget type, team, compliance)
- Preferred ways of working (meeting cadence, approval process, communication tools)
- Open risks or concerns from the client side

---

## Anti-hallucination Rules

- Do not invent questions based on general product type, base questions on what is actually known and what is actually unknown from Project Knowledge
- Do not assume technical answers, if a constraint is unclear, write a question about it, not an assumption
- Do not add questions that are already answered in Project Knowledge, check before generating
- If Project Knowledge files are missing or empty, flag it before generating:

> "Project Knowledge is not loaded or incomplete. Questions below are based on general BA practice for this type of session, review carefully before using."

---

## After Generation

1. BA reviews and removes questions already answered
2. Reorder or adjust priority based on meeting time available
3. Share with client beforehand if helpful (`MUST` questions only)
4. Use as a live checklist during the session
5. After the session, feed outputs into `/meeting-to-requirements`
