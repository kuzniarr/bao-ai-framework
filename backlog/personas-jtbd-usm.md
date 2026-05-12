---
name: personas-jtbd-usm
description: "Sequential discovery chain: Personas → JTBD → User Story Map. Each artifact is generated, reviewed, and approved by the BA before the next begins. Output: Personas.md + JTBD.md + USM.xlsx for Miro import."
---

# Personas → JTBD → User Story Map

## What this skill does

Builds three connected discovery artifacts in sequence. Each step feeds the next. BA approves each artifact before the next one starts — no steps are skipped.

## Flow

```
Step 1: Personas → BA approves → Personas.md
Step 2: JTBD (based on Personas) → BA approves → JTBD.md
Step 3: User Story Map (based on Personas + JTBD) → BA approves → USM.xlsx for Miro
```

## Rules for all steps

- Work one artifact at a time — do not proceed without BA approval
- Mark all information: ⚠️ unconfirmed (assumption) / ✓ confirmed (explicitly stated)
- If contradictions exist — stop and ask the BA
- Actively suggest missing personas, jobs, or stories — mark as [IDEA]
- Language: Ukrainian unless BA specifies otherwise

---

## Step 1: User Personas

Determine how many personas are needed based on distinct user types.

**For each persona:**

```
# Persona [N] — [Name]

1. Basic Info: Name / Age / Location / Occupation / Income level
2. Background and Context
3. Goals: Primary / Secondary / Long-term
4. Needs: What the user wants to get done
5. Pain Points: Main problems / What does not work / What they fear
6. Motivations: What drives them / Why they'd use the product
7. Behaviors: How they decide / Usage frequency
8. Barriers: What could prevent usage
9. Quotes: 1–3 realistic phrases
```

After BA approval → deliver as Personas.md.

---

## Step 2: JTBD

For each approved persona, generate Jobs to Be Done.

**Format:**
> "When [situation], I want to [motivation], so that [expected outcome]."

After BA approval → deliver as JTBD.md.

---

## Step 3: User Story Map

Build based on Personas + JTBD. Structure: Actor → Epic → User Flow → User Story.

Generate as table in chat first:

| Actor | Epic | User Flow | User Story |
|---|---|---|---|

Suggest missing stories — mark as [IDEA].

After BA approval → generate xlsx for Miro using /miro-xlsx-generator.
