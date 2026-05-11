# BAO AI-Native BA Framework

Shared skills library and delivery framework for Business Analysts.

---

## What this is

A standardized AI setup for the BA role on Scrum delivery projects. Built on Claude.ai, it covers the full BA workflow from project setup through requirements, Jira, and ongoing delivery activities.

The framework consists of:
- **Skills** — reusable methodology packages installed into Claude.ai via `/skill-name`
- **Prompt templates** — lightweight prompts for one-off tasks
- **Delivery guide** — phase-by-phase BA workflow with step-by-step instructions

---

## Repository Structure

```
bao-ai-framework/
├── skills/          → .md files — install into Claude.ai via Settings → Skills
├── prompts/         → prompt templates for one-off tasks
├── guide/           → delivery guide (phase-by-phase BA workflow)
├── SKILLS.md        → full index of all skills with status and descriptions
└── README.md        → this file
```

---

## How to Install Skills

1. Open [Claude.ai](https://claude.ai) → **Settings → Skills → Add Skill**
2. Open the skill file from `/skills/` in this repo
3. Copy the full content
4. Paste into Claude.ai → Save

Once installed, invoke any skill in a Claude Project chat with `/skill-name`.

> Skills are project-agnostic — they work on any delivery project. Project-specific context lives in the Claude Project Knowledge files, not in skills.

---

## How to Use

1. Install the skills you need (see [SKILLS.md](./SKILLS.md) for the full list)
2. Connect the Atlassian MCP connector in Claude.ai → Settings → Connectors
3. Create a Claude Project for your delivery project
4. Follow the guide in `/guide/` phase by phase

---

## How to Contribute

**Adding a new skill:**
1. Create a branch: `git checkout -b skill/your-skill-name`
2. Add your skill file: `/skills/your-skill-name.md`
3. Update `SKILLS.md` — add a row to the index table
4. Open a Pull Request with a short description of what the skill does and when to use it

**Updating an existing skill:**
1. Create a branch: `git checkout -b update/skill-name`
2. Edit the skill file in `/skills/`
3. Open a Pull Request — describe what changed and why

**Branch naming convention:**
- New skill: `skill/skill-name`
- Update: `update/skill-name`
- Fix: `fix/skill-name`
- Guide update: `guide/section-name`

---

## Maintainer

Volodymyr — Business Analyst, Geniusse
BAO Chapter initiative
