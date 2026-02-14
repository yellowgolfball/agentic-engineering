# CLAUDE.md — project memory

## Project summary
<!-- Replace with a 2-3 sentence description of your project -->
Brief description of what this project does, its architecture, and primary goals.

## How to work in this repo
- Read `AGENTS.md` for operating principles and team conventions.
- Default to small, reversible changes; explain tradeoffs and assumptions.
- Prefer explicit configuration over implicit defaults.

## Documentation workflow
- **Before starting work**: run `npm run docs:list` to see available documentation.
- **Read relevant docs**: check the "Read when" hints and read docs matching your task.
- **Keep docs current**: update docs when behavior or APIs change.
- **Front matter required**: all docs must include `summary` and `read_when` fields.

## Build, test, and development commands
<!-- Update these with your actual commands -->
- Install deps: `npm install`
- Build: `npm run build`
- Test: `npm run test`
- Lint: `npm run lint`
- List docs: `npm run docs:list`

## Coding style
<!-- Customize to your project -->
- TypeScript: ESM, strict typing, avoid `any`
- Keep files under ~500 LOC; split when it improves clarity
- Add comments for non-obvious logic; skip boilerplate doc comments

## Commit guidelines
- Use concise, action-oriented commit messages
- Use a component prefix when scope is clear (e.g., `api: add auth middleware`)
- IMPORTANT: never commit files containing secrets (`.env`, API keys, credentials)

## Tools

### Oracle
Oracle bundles prompts and files so another AI model can provide a second opinion with full context. Use when stuck, reviewing critical code, or cross-checking decisions.

**Usage:**
- Run `npx -y @steipete/oracle --help` once per session before first use
- Basic: `npx -y @steipete/oracle -p "your prompt" --file "path/to/files"`
- Multi-model: `npx -y @steipete/oracle -p "prompt" --models gpt-5.1-pro,gemini-3-pro --file "src/**/*.ts"`

**When to use:**
- Stuck on a complex problem and need a fresh perspective
- Reviewing critical code before committing
- Cross-checking architectural decisions across models
