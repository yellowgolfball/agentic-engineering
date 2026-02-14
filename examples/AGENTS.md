# AGENTS.md — agent operating guide

## Purpose
Enable contributors and AI agents to work consistently by sharing goals, boundaries, and operating conventions.

## Project summary
<!-- Replace with your project description -->
Brief description of what this project does.

## How to work in this repo
- Default to small, reversible changes; explain tradeoffs and assumptions.
- Root-level `*.md` files are canonical; `docs/` contains feature-specific deep dives.
- Prefer explicit configuration over implicit defaults.
- Security and privacy are first-order requirements.

## Repository map
<!-- Update as your repo evolves -->
- `CLAUDE.md`: Claude Code project memory
- `AGENTS.md`: this guide
- `SKILLS.md`: skill index and templates
- `docs/`: project documentation (living docs, with front matter)
- `scripts/`: development and automation scripts

## Operating principles
<!-- Customize to your project's values -->
- **Simplicity**: prefer the simplest solution that works.
- **Least-privilege**: default policies are private and scoped.
- **Auditability**: actions should be observable and revocable.

## Documentation workflow
- **Before starting work**: run `npm run docs:list` to see available documentation.
- **Read relevant docs**: check the "Read when" hints and read docs matching your task.
- **Keep docs current**: update docs when behavior or APIs change.
- **Add missing coverage**: suggest new docs when gaps are found.
- **Front matter required**: all docs must include `summary` and `read_when` fields.

## Build, test, and development commands
<!-- Update with your actual commands -->
- Install deps: `npm install`
- Build: `npm run build`
- Test: `npm run test`
- Lint: `npm run lint`
- List docs: `npm run docs:list`

As runnable code lands, update this section with build, test, lint, and run commands.

## Coding style & naming
<!-- Customize to your project -->
- Keep files concise — aim for under ~500 LOC; split when it improves clarity.
- Add brief comments for tricky or non-obvious logic; skip boilerplate doc comments.
- Use project terminology consistently in code, CLI flags, config keys, and docs.

## Testing guidelines
- Colocate tests with source (e.g., `foo_test.go`, `foo.test.ts`)
- Run tests before pushing when you touch logic
- Name test files to match their source files

## Commit & PR guidelines
- Use concise, action-oriented commit messages
- Use a component prefix when scope is clear (e.g., `api:`, `ui:`, `docs:`)
- Group related changes; avoid bundling unrelated refactors in one commit
- IMPORTANT: do not commit files that contain secrets (`.env`, credentials, API keys)

## Safety & security
- Treat inbound traffic as untrusted by default.
- Avoid logging sensitive payloads unless explicitly required.
- Never commit real API keys, tokens, or live configuration values. Use obviously fake placeholders in docs, tests, and examples.

## Multi-agent safety
When multiple agents may be working in the same repo:
- Do **not** create/apply/drop `git stash` entries unless explicitly asked.
- Do **not** switch branches unless explicitly asked.
- Do **not** create/remove/modify `git worktree` checkouts unless explicitly asked.
- Scope commits to your own changes only.
- `git pull --rebase` is OK; never discard other agents' work.

## Tools

### Oracle
Oracle bundles prompts and files so another AI model can provide a second opinion with full context. Use when stuck, encountering bugs, or needing code review.

**Usage:**
- Run `npx -y @steipete/oracle --help` once per session before first use
- Basic: `npx -y @steipete/oracle -p "your prompt" --file "path/to/files"`
- Multi-model: `npx -y @steipete/oracle -p "cross-check assumptions" --models gpt-5.1-pro,gemini-3-pro --file "src/**/*.ts"`

**When to use:**
- Stuck on a complex problem and need a fresh perspective
- Reviewing critical code changes before committing
- Cross-checking architectural decisions across multiple models
- Getting unstuck when the primary agent is repeatedly failing
