# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a curated collection of Claude Code agent skills sourced from [anthropics/skills](https://github.com/anthropics/skills) and [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills). Each skill extends Claude's capabilities for specific domains (design, documents, deployment, testing, etc.).

## Skill Location

All skills live under `.agent/skills/`. This is the directory configured in Claude Code settings as the skills search path. Vercel Labs skills additionally include `metadata.json` (version/owner/abstract), `AGENTS.md`, and sometimes a `rules/` directory with lint-style rules.

## Skill Anatomy

Every skill directory follows this structure:

```
.agent/skills/skill-name/
├── SKILL.md          # Required: YAML frontmatter + markdown instructions
├── LICENSE.txt        # Optional: license for the skill
├── scripts/           # Executable code for deterministic/repetitive tasks
├── references/        # Docs loaded into context as needed
├── assets/            # Files used in output (templates, icons, fonts)
├── templates/         # Template files for code generation
├── examples/          # Example inputs/outputs
└── <language-specific dirs>/  # e.g. python/, typescript/, go/
```

## SKILL.md Format

Every SKILL.md has YAML frontmatter with required fields:

```yaml
---
name: skill-name
description: When to trigger and what it does. This is the primary triggering mechanism.
license: Optional license info
---
```

The description field determines when Claude invokes the skill — it should be specific about trigger conditions, including both what the skill does and the contexts/domain where it applies.

The body is free-form markdown with instructions for Claude when the skill is active.

## Skill Loading Model

Skills use three-level progressive disclosure:
1. **Metadata** (name + description) — always in context
2. **SKILL.md body** — loaded when skill triggers
3. **Bundled resources** — loaded on demand as referenced from SKILL.md

Keep SKILL.md bodies under 500 lines. When approaching this limit, split content into `references/` files with clear pointers from SKILL.md.
