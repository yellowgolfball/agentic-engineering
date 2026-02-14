# SKILLS.md — project skill index

This file lists repo-specific skills for agents working in this project.
Skills are short, task-focused playbooks. Keep them minimal and actionable.

## Current skills
<!-- Add skills as you create them -->
- (none yet — add skills to `.claude/skills/` and list them here)

## Skill template
Use this template when adding a new skill to `.claude/skills/<name>/SKILL.md`.

```yaml
---
name: my-skill
description: Brief description of what this skill does
argument-hint: [optional-argument]
allowed-tools: Read, Grep, Glob, Bash, Edit
---

# Skill Name

## When to use
- <trigger condition>

## Steps
1. <step>
2. <step>

## Outputs
- <files or artifacts produced>

## Notes
- <edge cases / safety considerations>
```

## Conventions
- `SKILLS.md` in the repo root is the human-readable index.
- `.claude/skills/<name>/SKILL.md` is where Claude Code reads skills from.
- Use clear triggers so agents know when to apply each skill.
- Keep commands and file paths explicit.
- Use `allowed-tools` in frontmatter to grant permissions without prompting.
- Use `context: fork` for skills that should run in a subagent with separate context.
