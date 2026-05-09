---
name: design-validation
description: "Validates Figma designs against requirements. Input: Figma file link + scope (flow name + channel). Output: gap table with severity (Critical / Major / Minor) + open questions. Requires Claude Code + Figma MCP for full frame access."
---

# Design Validation

## What this skill does

Reads Figma frames for a specified flow and validates them against the project requirements. Returns a gap table — every missing screen, state, or element — with severity classification.

## Prerequisites

For full frame access, use Claude Code + Figma MCP (figma-mcp-go):
- Figma Desktop open with the project file
- figma-mcp-go plugin active (status: Connected)
- figma-mcp-go MCP server running on :1994
- Claude Code connected to figma-mcp-go MCP

*Claude.ai Figma connector has a 3-request/month limit — insufficient for real validation work.*

## Input

- Figma file link
- Scope: flow name (e.g. "connect flow", "campaign creation") + channel name (e.g. Meta, LinkedIn)

## Steps

**Step 1 — Read the design**
Use Figma MCP to get screenshots and metadata for the relevant frames.

**Step 2 — Validate against requirements**
Cross-reference frames with all relevant requirements. Check:
1. All required screens present (happy path)
2. All required error states present
3. All required loading / empty states present
4. UI elements match requirements (buttons, labels, fields, messages)
5. Flow transitions are complete — no missing steps, no dead ends
6. Channel-specific constraints are respected

**Step 3 — Output**

### Gaps

| # | Screen / State | Gap description | Severity |
|---|---|---|---|

**Severity:**
- Critical — missing required screen or state, blocks the user
- Major — wrong behavior, missing validation, contradicts requirements
- Minor — label mismatch, UX inconsistency, missing edge case

### Questions
Open questions for designer or client — only genuine gaps, not assumptions.

**Q1** [Screen name] — [Question]

### Summary
One paragraph: overall design coverage for this flow, main risk areas.

## Rules

- If a requirement has TBD status — note it but do not flag as gap
- Do not invent requirements not present in project context
- If a screen is not accessible via Figma MCP — state it explicitly, do not guess
- Write in English
