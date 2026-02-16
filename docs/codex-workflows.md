---
summary: "Quick-start checklist for Codex project setup and safe execution"
read_when:
  - Codex
  - onboarding
  - setup
  - permissions
  - sandbox
  - approvals
---

# Codex Quick-Start Checklist

Use this when onboarding a repo to Codex or standardizing team defaults.

## 1. Trust level
- Decide whether the repo should be trusted.
- Add a project trust entry in `.codex/config.toml` if needed.

## 2. Config placement
- Personal defaults go in `~/.codex/config.toml`.
- Team defaults go in `.codex/config.toml` (loaded only after trust).
- If multiple `.codex/config.toml` files exist, the closest one to your working directory wins.

## 3. Agent instructions
- Add `AGENTS.md` at the repo root with build/test/lint commands and team conventions.
- Use `AGENTS.override.md` in subdirectories only when you need to fully replace parent guidance.

## 4. Skills
- Put Codex skills in `.agents/skills/<name>/SKILL.md`.
- Keep skills short and task-focused; include explicit commands and file paths.

## 5. Rules and approvals
- If you need to allow safe commands outside the sandbox, add `.codex/rules/*.rules`.
- Start with an allowlist for read-only commands; add prompts for mutating commands; forbid dangerous ones.
- Prefer narrow prefixes that match the exact argv you intend to allow.

## 6. Documentation discovery
- Add `docs:list` and front matter to `docs/` so agents can quickly find relevant docs.
- Keep docs current when APIs or behavior change.

## 7. Verification
- Run the repo’s normal tests or checks after code changes.
- Use a second-opinion tool (like Oracle) for high-risk changes.
