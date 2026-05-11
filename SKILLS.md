# Skills Index

All skills in the BAO AI-Native BA Framework.

Install each skill via Claude.ai → Settings → Skills → Add Skill.
Invoke in any Claude Project chat with `/skill-name`.

---

| Skill | Status | Phase | Description | Trigger |
|---|---|---|---|---|
| `/project-charter` | ✅ ready | 1 · Project Setup | Generates a Project Charter from kickoff notes. Output: `01_project_context.md` for Project Knowledge. | Kickoff notes → structured charter |
| `/stakeholder-analysis` | ✅ ready | 1 · Project Setup | Generates a structured stakeholder map from the Project Charter. Output: `02_stakeholders.md`. | Need stakeholder map for a project |
| `/ba-communication-plan` | ✅ ready | 1 · Project Setup | Generates a BA communication plan: meeting cadence, contact matrix, escalation path, approval process. | Need to define communication structure |
| `/ba-governance` | ✅ ready | 1 · Project Setup | Generates a BA governance document: requirements management, change tracking, DoR, DoD, approval matrix. | Need to define governance approach |
| `/elicitation-prep` | ✅ ready | 2 · Discovery | Generates a structured question list for a client meeting or workshop, grouped by topic and prioritized. | Preparing for a client session |
| `/confluence-specification-skeleton` | ✅ ready | 3 · Requirements | Creates Confluence page structure for a feature or epic: overview, goals, scope, NFRs, stories section. | Starting a new feature specification |
| `/user-stories` | ✅ ready | 3 · Requirements | Generates FE and BE user stories in mandatory format with all sections. Publishes to Confluence via MCP. | Writing stories for a feature |
| `/validate-requirements` | ✅ ready | 3 · Requirements | Reviews requirements against structure, scope, clarity, FE/BE split, and API mapping. Returns issues only — does not rewrite. | Reviewing a requirements document |
| `/meeting-to-requirements` | ✅ ready | 3 · Requirements | Converts a meeting transcript into structured requirements: decisions, action items, numbered FE/BE requirements. | After a client call with new scope |
| `/jira-tasks` | ✅ ready | 4 · Jira | Creates Story + FE Task + BE Task in Jira from an approved user story. Uses Atlassian MCP. | Pushing approved stories to Jira |
| `/traceability-matrix` | ✅ ready | 4 · Jira | Builds a traceability + progress matrix in xlsx: Team / Epic / Feature / US / Tasks / Status / Progress %. | Need full project coverage overview |
| `/api-mapping` | ✅ ready | 3 · Requirements | Builds an API field mapping table between product UI fields and a third-party API. Output: `mapping.xlsx`. | Starting a new integration |
| `/design-validation` | ✅ ready | Iterative | Validates Figma designs against requirements. Output: gap table with severity + open questions. | Figma handoff review |
| `/personas-jtbd-usm` | ✅ ready | 2 · Discovery | Sequential chain: Personas → JTBD → User Story Map. Approve-before-proceed pattern. Output: Personas.md + JTBD.md + USM.xlsx. | Starting discovery on a new product area |
| `/invest-check` | ✅ ready | 3 · Requirements | Reviews user stories against INVEST criteria. Returns pass/fail per criterion + improvement suggestions. Does not rewrite. | Before publishing stories for development |
| `/gherkin-ac` | ✅ ready | 3 · Requirements | Writes Acceptance Criteria in Gherkin (BDD) format: Given / When / Then. Covers happy path, alt flows, edge cases. | Team practices BDD or QA writes automated tests |
| `/ears-ac` | ✅ ready | 3 · Requirements | Writes Acceptance Criteria in EARS syntax. Formal, unambiguous, independently verifiable requirements. | Formal requirement notation needed |
| `/miro-xlsx-generator` | ✅ ready | Iterative | Generates xlsx for Miro import as sticky notes. Supports Story Map and Flow/Discovery Map types. | Need to put content into a Miro board |
| `/drawio-diagram-style` | ✅ ready | Iterative | Generates draw.io XML diagrams with unified style: colors, layout, arrow routing. | Any architecture or flow diagram |

---

## Status Legend

| Status | Meaning |
|---|---|
| ✅ ready | Skill is complete and tested — install and use |
| 🚧 in progress | Skill is being written — not ready for use |
| 📋 planned | Skill is on the roadmap — does not exist yet |
