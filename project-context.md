# Project Context — Change Up Learning 2.0 (Nova)
_Last updated: 08 July 2026_
 
## Project
 
Change Up Learning 2.0 (codename **Nova**) is a custom LMS for Change Impact (US-based, B-Corp, EST). It replaces the existing WordPress LMS, which hit its scalability, data integrity, and feature management limits at ~26,000 users / 1,900+ groups. The custom LMS path was chosen during Discovery (over Moodle and Open edX). Delivery model is T&M with a fixed MVP target of Q4 2026 (December). Geniusee delivers the full lifecycle: planning ("month zero") → design → development → QA → migration → MVP launch to pilot partners.
 
## Key Constraints
 
- **MVP launch**: early December 2026 (exact date TBD). Change Impact office closes ~17 December — release must happen before that.
- **Budget**: T&M with a tight cap. Solution Architect was not in the Discovery budget — flagged by Danielle. SOW overrun is possible; formal process is defined in `04_standards.md` §8.
- **Planning phase (Month 0)**: ~1 month. BA full-time, UX 80h/month, SA 80h/month (SA likely ramps off after Month 0).
- **Resourcing**: 6 of 12 Geniusee team members TBD (BE x2, FE x2, QC, DevOps).
- **Time zones**: HST (Danielle), CDT (Maddie — Tue/Wed/Thu only, 9:30–17:30), GMT+2 (Geniusee). Effective overlap is narrow.
- **Sprints**: 2-week. Meetings Tue/Thu (confirmed 15 May 2026).
- **Vacation rule (Geniusee)**: max ~1.5 days per working week per person, distributed.
- **Performance SLAs**: static ≤3s, operational ≤5s, reporting ≤10s; API 500ms target; 99% uptime. Maintenance windows max 4h, off-hours US Central (2:00–4:00 AM CT).
- **Compliance**: ADA / WCAG AA, CCPA cookie compliance, GDPR-aligned data handling.

## Risks
 
| Risk | Impact | Notes |
|---|---|---|
| Migration complexity | High | 26k users / 1,900+ groups from WordPress, multi-group memberships, certificates, compliance history. Risk of data loss / historical reporting gaps. |
| Tight budget with SA cost not initially scoped | Med | Danielle explicitly flagged scope creep risk on 15 May. SOW overrun requires a formal financial sync. PM tracks burn weekly. |
| Maddie part-time (3 days/week) + time-zone gap | Med | Maddie is the sole tech lead on the client side. Limits sync windows for architecture / integration decisions. |
| Multi-tenant 3-level hierarchy not off-the-shelf | Med | Justification for custom build, but requires early validation with client. |
| Third-party dependency downtime (Square, SendGrid, Articulate, LiveAgent, AWS Cognito) | Med | Articulate is the sole SCORM provider — no fallback. |
 
## Project Identifiers
 
| Tool | Value |
|---|---|
| Project codename | Nova |
| Jira project | Internal Geniusee Jira instance (replaces Asana per decision 15 May 2026). Client team — Danielle, Maddie, possibly Jen — have accounts via Geniusee email. |
| Notion teamspace | Client-owned, URL — TBD |
| Figma | Client-owned organization account, Professional plan. Oleksandr — Full Seat license. |
| Slack | Channel renamed from PC (existing), team invited. |
| GitHub repo | TBD |
| Staging / QA URLs | TBD (environments not provisioned) |
 
 
 
## Project Knowledge — Source Map
 
| Logical name | Actual file in Project Knowledge | Origin | Trust Level | Notes |
|---|---|---|---|---|
| Product Context | `Product Context — Change Up Learning.md` | BA synthesis — all primary sources (S1–S14) | ✅ Primary source of truth for product | Full functional scope, roles, permissions, glossary, parallel tracks. First reference for any product question. Does NOT retain inline `(see Qxx)` references — these are deprecated as of 25 May 2026 and removed during cleanup. |
| Project Context | `Project Context — Change Up Learning 2.md` | BA synthesis — Geniusee delivery | ✅ Delivery context | This file. Delivery-side constraints, risks, identifiers, source map, document architecture. |
| Stakeholders | `02_stakeholders (2).md` | BA synthesis — Geniusee | ✅ Baseline | Roles, decision authority, contacts, RACI. |
| Tech Context | `Tech Context — Change Up Learning 2.md` | BA synthesis — Geniusee | ✅ Baseline | Stack, integrations, architecture, technical constraints. |
| Standards | `04_standards.md` | BA synthesis — Geniusee | ✅ Baseline | Story format, EARS AC, DoR, DoD, task split, change control. |
| WBS | `WBS_Change_Impact_Scope_only.xlsx` | Geniusee delivery WBS, confirmed by Maddie 15 May 2026 | ✅ Scope baseline | Primary source for WBS row numbers (R-XX) cited throughout Product Context. Row numbers are stable within a WBS version; if WBS is re-baselined, all references must be reviewed. |
 
## Roles (client-approved naming, S14 / 26 May 2026)
System Admin (top tier — formerly PSA) · Admin (formerly SA / WP Admin) · Group Owner (formerly PGL) · Group Leader (formerly GL) · Learner (formerly Participant). When reading source artifacts (S6–S17), traceability, or elicitation transcripts, expect the original internal labels and map them with the table above. Note: "System Admin" now means the TOP tier, not the former mid-tier (which is now Admin).


### Conflict Resolution Rule
 
1. **Product Context is the canonical source** for product, roles, permissions, glossary, and scope. In conflict with any other file — Product Context wins.
2. **WBS row citations in Product Context** take precedence over any narrative interpretation.
3. **Delivery-side files** (Stakeholders, Tech Context, Standards) are authoritative within their domain (team, tech stack, process). They do not override Product Context for product decisions.
4. **New decisions** from elicitations or client communications are added to Product Context first, then propagated to delivery files if needed. 
5. WBS ↔ PC discrepancies are surfaced to the BA before applying — never reconciled silently. Precedence (points 1–2) decides the outcome; this point governs the procedure.


### Conventions
 
- **WBS row references**: cited as `R<number>` (e.g., `R152`, `R74`). Numbers are stable within a WBS version; if WBS is re-baselined, references must be reviewed.
- **Source citations in Product Context**: `Sx transcript HH:MM:SS` or `Sx §<section>` where S1–S14 are listed in Product Context §10. These sources are inputs to BA synthesis, NOT files in Project Knowledge. Project Knowledge contains synthesised outputs only.
- **Story references**: by title, not ordinal number — numbers drift on renumbering.
- **`proposed` / `TBD` tags**: any AC/requirement element tagged proposed or TBD must have a concrete Open Question whose answer clears the tag; no orphan tags without an OQ.
- **Dates**: ISO `YYYY-MM-DD` in artifacts and changelog.
- **Skill fidelity**: for any activity that has a dedicated skill, follow that skill strictly and do not deviate (decomposition → decompose-wbs-epic; etc.). **Any requirement (story + AC) written for Notion or Jira MUST be produced with ears-ac — never write AC freehand, not even for a single story or a quick edit.**
- **Surfaces**: operate only on the approved surfaces from Product Context §2.4; do not invent new ones. `email` is a channel, not a surface. Each story carries a one-line Surface note.
- **No source citations in client-facing requirement/AC text**: the story/AC text that goes to Notion/Jira carries no source references. Internal traceability (WBS-AC verbatim block under the parent, PC source-map) stays separate and untouched.
- **No client personal names** (Danielle / Maddie / Jen) in requirements (stories/AC), changelog, or any client-facing artifact (call summaries, shared OQ lists, flowcharts, Figma comments). The company name (Change Impact) is allowed. Internal-only artifacts may name freely; question formulations may name when needed.
- **Changelog trigger**: write a changelog item when an output introduces a change to existing baselined scope (Change Request) or a new feature (New Feature Request). A clarification that changes nothing baselined → no item.
