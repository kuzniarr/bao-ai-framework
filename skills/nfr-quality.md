---
name: nfr-quality
description: "Generate or validate Non-Functional Requirements using ISO 25010 quality attributes and Utility Tree thinking. Use when defining measurable NFRs or reviewing existing NFR quality for a feature, epic, or system."
---

# /nfr-quality

## What it does

Generates and validates Non-Functional Requirements (NFRs) for a feature, epic, or system using the Utility Tree approach (ISO 25010 quality attributes). Produces structured NFR scenarios with measurable response measures — not vague statements like "the system must be fast."

Use when: starting a new epic, preparing for architecture review, or when dev/QA flags missing NFRs during refinement.

## Input

- Feature or epic description (Confluence URL, Jira epic key, or pasted text)
- Optional: known constraints (SLA, user volume, regulatory requirements, tech stack limitations)
- Optional: mode — `GENERATE` (write NFRs from scratch) or `VALIDATE` (review existing NFRs)

## Output

**GENERATE mode:**
Utility Tree with NFR scenarios — quality attribute / rationale / business goal / scenario (stimulus → response → measure)

**VALIDATE mode:**
Review report — pass/fail per NFR with specific issues flagged

## Process

### GENERATE mode

**Step 1 — Identify relevant quality attributes**

Based on the feature context, select applicable attributes from ISO 25010:

| Category | Attributes to consider |
|---|---|
| Performance efficiency | Time behaviour, resource utilisation, capacity |
| Reliability | Fault tolerance, recoverability, availability |
| Security | Confidentiality, integrity, authentication, authorisation |
| Usability | Learnability, operability, accessibility |
| Maintainability | Modularity, testability, modifiability |
| Portability | Adaptability, installability |
| Compatibility | Interoperability, co-existence |

Select only those relevant to the feature — not all attributes apply to every story.

**Step 2 — Build Utility Tree**

For each selected quality attribute, generate a scenario:

| Field | Description |
|---|---|
| Quality attribute | Name per ISO 25010 |
| Rationale | Why this attribute matters for this feature |
| Business goal | Which business goal it supports |
| Source of stimulus | Who or what triggers the scenario |
| Stimulus | The specific triggering event |
| Environment | Conditions under which it occurs |
| Artifact | What part of the system is affected |
| Response | What the system does |
| Response measure | Measurable threshold (Goal / Stretch / Wish) |

**Step 3 — Flag missing NFRs**

After generating, check if the following are covered (mark ❌ if missing):
- Performance under peak load
- Error / failure recovery
- Data security and access control
- User-facing error messages
- Logging and auditability (if regulated domain)

**Step 4 — Deliver output**

```
## NFR Utility Tree — [Feature / Epic name]

### Performance — Time behaviour
**Rationale:** Users expect payment confirmation within seconds or will abandon.
**Business goal:** 4.8+ app store rating

| Field | Value |
|---|---|
| Source of stimulus | Mobile app user |
| Stimulus | Confirms payment under peak load |
| Environment | Peak load (10,000 concurrent users) |
| Artifact | Payment processing service |
| Response | Transaction confirmed in app |
| Response measure | Goal: ≤2s / Stretch: ≤3s / Wish: ≤1s |

---
[next attribute...]

### Coverage check
✅ Performance — covered
✅ Security — covered
❌ Recoverability — not covered — recommended: add offline/retry scenario
```

---

### VALIDATE mode

**Step 1 — Read existing NFRs**

Fetch from Confluence or pasted content.

**Step 2 — Check each NFR against quality criteria**

| Criterion | Check |
|---|---|
| Measurable | Has a specific numeric threshold, not "fast" or "secure" |
| Testable | Can be verified in a test scenario |
| Contextual | Linked to a specific feature or component, not generic |
| Realistic | Achievable given tech stack and project constraints |
| Traceable | Linked to a business goal or user need |

**Step 3 — Return report**

```
## NFR Validation Report — [Feature]

| NFR | Measurable | Testable | Contextual | Issue |
|---|---|---|---|---|
| "System must be fast" | ❌ | ❌ | ❌ | No threshold, not testable, not scoped |
| "Response ≤2s under 10k users" | ✅ | ✅ | ✅ | — |

### Recommended fixes
- NFR 1: rewrite as "API response time ≤ [X]ms for [Y]% of requests under [Z] concurrent users"
```

## Rules

- Do not generate NFRs with invented technical values (throughput, latency thresholds) — use TBD where real data is unknown
- Always link each NFR to a business goal or user need — standalone technical NFRs are not acceptable
- Do not include NFR categories not relevant to the feature scope
- Anti-hallucination: if project constraints (SLA, volume, regulations) are unknown — write TBD and flag for BA to confirm with tech lead

## MCP Path

If Confluence URL or Jira key provided:
→ Fetch feature/epic content via Atlassian MCP
→ Extract functional scope → identify applicable quality attributes → generate Utility Tree

## MCP Unavailable Fallback

If MCP is not accessible:
→ Ask BA to paste feature description and any known constraints
→ Generate from pasted content
→ Mark all numeric thresholds as TBD unless BA provides them explicitly
