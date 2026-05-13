---
name: sprint-zero-plan
description: "Generate `sprint_zero_plan.md` as the working plan for Sprint 0, covering priorities, prep work, design-first epics, client open questions, and milestones before the full team joins."
---

# Skill: /sprint-zero-plan

## Purpose

Generate `sprint_zero_plan.md` — a working agenda for the first sprint on a delivery project, when the BA joins before the full dev team. Covers: priorities, which epics go to design first, what to prepare before the team joins, list of open questions for the client, day-by-day or week-by-week milestones.

## When to use

- Phase 1 · Project Setup, after charter + stakeholders + standards are in Project Knowledge.
- Typical scenario: BA + Designer in Sprint 0, dev team joins in Sprint 1.
- Also useful when BA joins mid-project and needs a structured plan for the first 2 weeks.

## Inputs

- WBS (MVP / Phase 1 epics — to identify what's in scope first)
- `01_project_context.md`
- `02_stakeholders.md`
- Optional: PM's planned timeline, design team availability, client cadence preference

## Output

Single file: `sprint_zero_plan.md`.

Structure:

```markdown
# Sprint 0 Plan

## Goal
<1-2 sentences — what Sprint 0 must achieve for the team to start delivery>

## Scope in Sprint 0
- Design scope: <epics that go to design first, with rationale>
- BA scope: <which Phase 1 / Phase 3 / Phase 4 artifacts must be ready>
- Out of Sprint 0: <epics deferred to later sprints>

## Priorities (ranked)
1. <highest priority item with reasoning>
2. ...

## Weekly milestones
**Week 1:**
- <day-level or item-level checklist>

**Week 2:**
- ...

## Prep checklist (before team joins)
- [ ] Jira project imported from WBS
- [ ] Confluence structure created per epic
- [ ] Top N epics refined to DoR
- [ ] Design scope delivered to designer per epic
- [ ] Communication plan agreed with PM and client
- [ ] DoR / DoD shared with team
- [ ] <project-specific items>

## Open questions for the client
<grouped by topic — sourced from Phase 1 inputs, not invented>

## Risks & dependencies
- <risk> → <mitigation>
- <dependency on stakeholder> → <owner>

## Daily routine (BA in Sprint 0)
<2-3 bullets on what a typical day looks like — sync with designer, refine epics, client touchpoint>
```

## Behavior rules

1. **Anchor on WBS phases.** If WBS has MVP / Phase 1 / Phase 2 markers — use them to define scope.
2. **Designer-first thinking.** If Designer is the only other team member in Sprint 0 — prioritize epics with high UI footprint and clear user flows. Defer epics that are mostly backend.
3. **Prep checklist is the deliverable.** Everything in the checklist must have an owner and a deadline (relative to Sprint 0 end).
4. **Open questions ≠ invented questions.** Pull only from inputs (gaps in charter, TBD items in standards, missing AC patterns in WBS). Do not invent.
5. **Living doc.** Output is a working file — BA will update weekly. Output format should be edit-friendly markdown, not a polished report.

## Example invocation

```
/sprint-zero-plan

Inputs:
- WBS attached, Phase 1 epics highlighted
- Designer joins Day 3, dev team joins start of Sprint 1 (2 weeks from now)
- Client cadence: weekly demo
```

## Anti-hallucination

If unknown — write `TBD`:
- Designer availability dates
- Client meeting cadence not in inputs
- Specific epic priorities not visible in WBS

Never invent specific stakeholder commitments, deadlines, or scope items not present in inputs.

## Relation to other skills

- Reads outputs of: `/project-charter`, `/stakeholder-analysis`, `/quick-standards`
- Feeds into: `/elicitation-prep` (open questions become elicitation input), `/decompose-epic` (scoped epics for Sprint 0 go to decomposition first)
- Updated by BA, not regenerated — once created, edit in place or via `/refine` follow-ups
