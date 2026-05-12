# Skills Index

All skills in the BAO AI-Native BA Framework.

Install each skill via Claude.ai → Settings → Skills → Add Skill.

Invoke in any Claude Project chat with `/skill-name`.

---

| Skill | Status | Phase | Description | Trigger |
| --- | --- | --- | --- | --- |
| `/project-charter` | ✅ ready | 1 · Project Setup | Generates a Project Charter from kickoff notes. Output: `01_project_context.md` for Project Knowledge. | Kickoff notes → structured charter |
| `/stakeholder-analysis` | ✅ ready | 1 · Project Setup | Generates a structured stakeholder map from the Project Charter. Output: `02_stakeholders.md`. | Need stakeholder map for a project |
| `/tech-context` | ✅ ready | 1 · Project Setup | Generates technical project grounding: stack, integrations, architecture notes, glossary, constraints. Output: `03_tech_context.md`. | Need technical context for BA work |
| `/ba-communication-plan` | ✅ ready | 1 · Project Setup | Generates a BA communication plan: meeting cadence, contact matrix, escalation path, approval process. | Need to define communication structure |
| `/ba-governance` | ✅ ready | 1 · Project Setup | Generates a BA governance document: requirements management, change tracking, DoR, DoD, approval matrix. | Need to define governance approach |
| `/elicitation-prep` | ✅ ready | 2 · Discovery | Generates a structured question list for a client meeting or workshop, grouped by topic and prioritized. | Preparing for a client session |
| `/confluence-specification-skeleton` | ✅ ready | 3 · Requirements | Creates Confluence page structure for a feature or epic: overview, goals, scope, NFRs, stories section. | Starting a new feature specification |
| `/decompose-epic` | ✅ ready | 3 · Requirements | Decomposes an epic into 5–10 vertical user stories using INVEST + splitting patterns. Output: grouped story list + rationale + open questions. | Breaking down an approved epic into stories |
| `/meeting-to-requirements` | ✅ ready | 3 · Requirements | Converts a meeting transcript into structured content: Decisions / Action Items / Requirements / Open Questions. Implementation-agnostic. | After a client call with new scope |
| `/invest-check` | ✅ ready | 3 · Requirements | Reviews user stories against INVEST criteria. Returns pass/fail per criterion + improvement suggestions. Does not rewrite. | Before publishing stories for development |
| `/gherkin-ac` | ✅ ready | 3 · Requirements | Writes Acceptance Criteria in Gherkin (BDD) format: Given / When / Then. Covers happy path, alt flows, edge cases. | Team practices BDD or QA writes automated tests |
| `/ears-ac` | ✅ ready | 3 · Requirements | Writes Acceptance Criteria in EARS syntax. Formal, unambiguous, independently verifiable requirements. | Formal requirement notation needed |
| `/push-to-jira` | ✅ ready | 4 · Jira | Creates a Jira Story ticket from an approved story with AC. Single ticket — no FE/BE split. Uses Atlassian MCP. | Pushing approved stories to Jira |
| `/generate-subtasks` | ✅ ready | 4 · Jira | Breaks a Jira Story into Subtasks following the project's configured `Task Split Pattern` from `00_quick_standards.md`. Project-configurable. | Splitting a Story into implementation pieces |
| `/traceability-matrix` | ✅ ready | 4 · Jira | Builds a traceability + progress matrix in xlsx: Team / Epic / Feature / US / Tasks / Status / Progress %. | Need full project coverage overview |
| `/design-validation` | ✅ ready | Iterative | Validates Figma designs against requirements. Output: gap table with severity + open questions. | Figma handoff review |

---

## Status Legend

| Status | Meaning |
| --- | --- |
| ✅ ready | Skill is complete and tested — install and use |
| 🚧 in progress | Skill is being written — not ready for use |
| 📋 planned | Skill is on the roadmap — does not exist yet |

---

## Universal Story Chain

The framework follows a universal chain for moving from epic to implementation:

Epic ↓ /decompose-epic Stories (titles + context) ↓ /gherkin-ac OR /ears-ac Story + AC ↓ /push-to-jira Jira Story (with AC) ↓ /generate-subtasks Subtasks (project-configurable split via 00_quick_standards.md)

Each step is a separate skill. Implementation split (FE/BE/Design/etc) is **project-specific** — configured once per project in `00_quick_standards.md`, not hardcoded in skills.
