# Agentic Engineering

Best practices for structuring repositories to work effectively with AI coding agents, with a focus on [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and [OpenAI Codex](https://openai.com/index/introducing-codex/).

These patterns are tool-agnostic where possible, but examples use Claude Code and Codex conventions since those are the primary agents this template targets.

## Table of contents

- [Project structure](#project-structure)
- [CLAUDE.md / AGENTS.md](#claudemd--agentsmd)
- [Skills](#skills)
- [OpenAI & Codex best practices](#openai--codex-best-practices)
- [docs:list — documentation discovery](#docslist--documentation-discovery)
- [Oracle — multi-model second opinions](#oracle--multi-model-second-opinions)
- [Settings and permissions](#settings-and-permissions)
- [Modular rules](#modular-rules)
- [MCP servers](#mcp-servers)
- [Examples](#examples)

---

## Project structure

A well-structured repo for agentic development looks like this:

```
your-project/
  CLAUDE.md                        # Project memory (read at session start)
  AGENTS.md                        # Agent operating guide (optional, see below)
  SKILLS.md                        # Skill index (optional)
  .gitignore
  package.json                     # Include docs:list script
  .claude/
    settings.json                  # Team settings (committed)
    settings.local.json            # Personal settings (gitignored)
    rules/
      code-style.md                # Modular, topic-specific rules
      testing.md
      security.md
    skills/
      fix-issue/
        SKILL.md                   # Custom skill
      review-pr/
        SKILL.md
  .agents/
    skills/                        # Codex skills (OpenAI Codex)
      fix-issue/
        SKILL.md
  .codex/
    config.toml                    # Codex project config (loaded when trusted)
  .mcp.json                        # Project-scoped MCP servers (committed)
  docs/
    architecture.md                # Feature docs with front matter
    api.md
    ...
  scripts/
    docs-list.ts                   # Documentation discovery script
```

The key principle: **make project knowledge discoverable and machine-readable**. Agents work best when they can find what they need without guessing.

---

## CLAUDE.md / AGENTS.md

These files provide context and instructions that agents read at the start of every session.

### CLAUDE.md

`CLAUDE.md` is Claude Code's native convention. Claude Code automatically loads it at session start. It supports a hierarchy of locations:

| File | Purpose | Shared with team? |
|---|---|---|
| `CLAUDE.md` (project root) | Team-wide project instructions | Yes (committed) |
| `.claude/CLAUDE.md` | Alternative location for project instructions | Yes (committed) |
| `CLAUDE.local.md` | Personal project overrides | No (gitignored) |
| `~/.claude/CLAUDE.md` | User-wide preferences | No (personal) |

Claude Code also supports `.claude/rules/*.md` files for modular, topic-specific rules (see [Modular rules](#modular-rules)).

### AGENTS.md

`AGENTS.md` is a more agent-agnostic convention. It serves the same purpose as `CLAUDE.md` but is not specific to any one tool. Some teams use both: `CLAUDE.md` for Claude Code-specific configuration and `AGENTS.md` for general operating principles.

If you use `AGENTS.md` alongside Claude Code, reference it from your `CLAUDE.md`:

```markdown
# CLAUDE.md
@AGENTS.md
```

The `@` import syntax tells Claude Code to pull in the contents of `AGENTS.md`.

### Codex AGENTS.md discovery (OpenAI)

Codex reads `AGENTS.md` files before doing any work, and the CLI automatically enumerates and injects them into the conversation.

Discovery order (summary):
- Global scope: in your Codex home directory (default `~/.codex`, or `$CODEX_HOME`), Codex reads `AGENTS.override.md` if present, otherwise `AGENTS.md`. It uses only the first non-empty file at this level.
- Project scope: starting at the project root (typically the Git root), Codex walks down to your current working directory and reads any `AGENTS.md` it finds; if no project root exists, it only checks the current directory.

Best practice: keep broad guidance at the project root, and place narrower instructions in subdirectories when a subtree needs different rules. When you want a subdirectory to fully override its parent guidance, use `AGENTS.override.md` in that directory.

### What to include

**Do include:**
- Build, test, and lint commands (anything an agent cannot guess)
- Code style rules that differ from language defaults
- Repository conventions (branch naming, commit message format, PR process)
- Architectural decisions and key terminology
- Documentation workflow instructions
- Multi-agent coordination rules (if applicable)

**Do not include:**
- Things agents can figure out by reading code (standard patterns, obvious conventions)
- Long tutorials or explanations (link to docs instead)
- Information that changes frequently
- File-by-file descriptions of the codebase

### Best practices

1. **Keep it concise.** If the file is too long, agents will lose track of important rules. Aim for under 200 lines.
2. **Use emphasis for critical rules.** "IMPORTANT: never commit API keys" is more likely to be followed than a buried sentence.
3. **Check it into git** so the whole team benefits and can contribute.
4. **Review and prune periodically.** Stale instructions cause confusion.
5. **Use `@path/to/file` imports** to keep the root file short while pulling in detailed sections from other files. Max import depth is 5.

### Example

See [`examples/CLAUDE.md`](examples/CLAUDE.md) and [`examples/AGENTS.md`](examples/AGENTS.md) for starter templates.

---

## Skills

Skills are task-focused playbooks that agents can invoke. Claude Code supports the [Agent Skills](https://agentskills.io) open standard.

### How skills work

Skills live in `.claude/skills/<skill-name>/SKILL.md`. They are loaded on demand — metadata scanning uses ~100 tokens, and full content loads only when invoked.

```
.claude/skills/fix-issue/
  SKILL.md           # Main instructions (required)
  template.md        # Optional template for output
  examples/
    sample.md        # Optional example
  scripts/
    validate.sh      # Optional script the agent can run
```

### SKILL.md structure

```yaml
---
name: fix-issue
description: Diagnose and fix a GitHub issue
argument-hint: [issue-number]
allowed-tools: Read, Grep, Glob, Bash, Edit
---

# Fix Issue

## Steps
1. Fetch issue details: `gh issue view $ARGUMENTS`
2. Understand the reported problem
3. Search the codebase for relevant code
4. Implement the fix
5. Run tests to verify
6. Summarize the change
```

### Key frontmatter fields

| Field | Purpose |
|---|---|
| `name` | Display name and `/slash-command` trigger |
| `description` | Helps the agent decide when to load it |
| `argument-hint` | Shown during autocomplete |
| `allowed-tools` | Tools available without asking permission |
| `disable-model-invocation` | Only the user can invoke (not the agent) |
| `user-invocable` | Set `false` to hide from the `/` menu (agent-only) |
| `model` | Model to use when the skill is active |
| `context` | Set to `fork` to run in a subagent |

### Dynamic context injection

Use `` !`command` `` syntax to run shell commands and inject their output:

```markdown
## Current PR context
- PR diff: !`gh pr diff`
- Changed files: !`gh pr diff --name-only`
```

### SKILLS.md — the index file

A root-level `SKILLS.md` file serves as a human-readable index of available skills. This is optional but useful for discoverability. It is separate from the `.claude/skills/` directory which is what Claude Code actually reads.

See [`examples/SKILLS.md`](examples/SKILLS.md) for a template.

### Codex skills (OpenAI)

Codex scans for skills in repository, user, admin, and system locations. For repositories, it scans `.agents/skills` in every directory from your current working directory up to the repository root.

Codex uses progressive disclosure for skills: it loads skill metadata first and only loads the full `SKILL.md` when the skill is used. The `SKILL.md` must include at least `name` and `description`.

## OpenAI & Codex best practices

- Put personal defaults in `~/.codex/config.toml` and team defaults in project-scoped `.codex/config.toml`. Codex loads project configs only after you trust the project.
- Codex reads every `.codex/config.toml` from repo root to your current working directory; when keys conflict, the closest config wins.
- The CLI inherits defaults from `~/.codex/config.toml`. For one-off runs, use `-c key=value` overrides (or dedicated flags like `--model`).
- The Codex IDE extension uses the same CLI config file, so set defaults like model, approvals, and sandbox there to keep CLI + IDE consistent.
- Use `.codex/rules/*.rules` files to control which commands Codex can run outside the sandbox (rules are experimental).
- In Codex CLI, prefix a line with `!` to run a local shell command; its output is treated like user-provided results and still respects approvals/sandbox settings.
- Use slash commands (`/`) in the Codex CLI to switch models, adjust permissions, or summarize long sessions without leaving the terminal.
- For OpenAI API usage, follow safety best practices: use moderation, add human oversight, and perform adversarial testing (red-teaming).
- Use the Moderation API to check whether text or images are potentially harmful; the moderation endpoint is free to use.

---

## docs:list — documentation discovery

`docs:list` is a documentation-driven development pattern. It scans your `docs/` directory and lists available documentation with summaries and "read when" hints, so agents know which docs are relevant to their current task.

### How it works

1. Every markdown file in `docs/` includes YAML front matter:

```markdown
---
summary: "REST API endpoints and authentication flows"
read_when:
  - API
  - endpoints
  - authentication
---

# API Reference
...
```

2. A script (`scripts/docs-list.ts`) walks the `docs/` directory and extracts this metadata.

3. Agents run `npm run docs:list` at the start of a session to see what documentation is available and when to read each doc.

### Setting it up

**1. Add the script to your project:**

Copy [`examples/docs-list.ts`](examples/docs-list.ts) to `scripts/docs-list.ts`.

**2. Add the npm script:**

```json
{
  "scripts": {
    "docs:list": "tsx scripts/docs-list.ts"
  },
  "devDependencies": {
    "tsx": "^4.19.2"
  }
}
```

**3. Add front matter to your docs:**

Every file in `docs/` should have:
- `summary`: one-line description (under 80 characters)
- `read_when`: list of keywords indicating when an agent should read the doc

**4. Instruct agents to use it:**

Add to your `CLAUDE.md` or `AGENTS.md`:
```markdown
## Documentation workflow
- Before starting work: run `npm run docs:list`
- Read docs that match your task (check "Read when" hints)
- Keep docs updated when behavior or APIs change
```

### Front matter tips

- **Be specific with `read_when` hints.** "WebSocket connections" is better than "networking".
- **Keep docs focused.** One doc per major topic.
- **Exclude noisy directories.** The script skips `archive/` and `research/` by default. Edit the `EXCLUDED_DIRS` set to customize.

### Example output

```
$ npm run docs:list

Listing all markdown files in docs folder:
api.md - REST API endpoints and authentication
  Read when: API; REST; authentication; endpoints
architecture.md - System architecture and component interactions
  Read when: architecture; system design; component roles
security.md - Threat model and security policies
  Read when: security; authentication; authorization; TLS

Reminder: keep docs up to date as behavior changes...
```

See [`examples/docs-list.ts`](examples/docs-list.ts) for the full script.

---

## Oracle — multi-model second opinions

[Oracle](https://github.com/steipete/oracle) is a tool that bundles prompts and files so another AI model can provide a second opinion with full context. It is useful for getting unstuck, cross-checking architectural decisions, and reviewing code with a different model's perspective.

### When to use Oracle

- **Stuck on a complex problem** and need a fresh perspective from a different model
- **Reviewing critical code** before committing
- **Cross-checking architectural decisions** across multiple models
- **Getting unstuck** when the primary agent is repeatedly failing

### Installation

```bash
# Use directly with npx (no install needed)
npx -y @steipete/oracle --help

# Or install globally
npm install -g @steipete/oracle
```

### Basic usage

```bash
# Get a second opinion on code
npx -y @steipete/oracle -p "review this code for bugs" --file "src/**/*.ts"

# Cross-check with multiple models
npx -y @steipete/oracle -p "cross-check assumptions" \
  --models gpt-5.1-pro,gemini-3-pro \
  --file "src/**/*.ts"

# Copy context bundle to clipboard (for pasting into another chat)
npx -y @steipete/oracle --render --copy -p "review this" --file "src/**/*.ts"

# Check recent sessions
npx -y @steipete/oracle status --hours 72
```

### Supported models

| Provider | Models |
|---|---|
| OpenAI | `gpt-5.1-pro`, `gpt-5.2`, `gpt-5.1-codex` |
| Google | `gemini-3-pro` |
| Anthropic | `claude-4.5-sonnet`, `claude-4.1-opus` |
| OpenRouter | Any OpenRouter model ID |

### Configuration

Oracle needs API keys for whichever provider you want to use:

```bash
# OpenAI
export OPENAI_API_KEY="your-key"

# Google
export GEMINI_API_KEY="your-key"

# Anthropic
export ANTHROPIC_API_KEY="your-key"

# Azure OpenAI
export AZURE_OPENAI_API_KEY="your-key"
export AZURE_OPENAI_ENDPOINT="https://your-resource.openai.azure.com"
export AZURE_OPENAI_DEPLOYMENT="your-deployment"
export AZURE_OPENAI_API_VERSION="2025-04-01-preview"
```

Browser mode (experimental) uses Chrome cookies and works without API keys.

### Integrating with your CLAUDE.md

Add an Oracle section to your `CLAUDE.md` or `AGENTS.md` so agents know when and how to use it:

```markdown
## Tools

### Oracle
Oracle bundles prompts and files for a second opinion from another AI model.

**Usage:**
- Run `npx -y @steipete/oracle --help` once per session before first use
- Basic: `npx -y @steipete/oracle -p "your prompt" --file "path/to/files"`
- Multi-model: `npx -y @steipete/oracle -p "prompt" --models gpt-5.1-pro,gemini-3-pro --file "src/**/*.ts"`

**When to use:**
- Stuck on a complex problem
- Reviewing critical code before committing
- Cross-checking architectural decisions
```

See the [upstream Oracle docs](https://github.com/steipete/oracle) for the full feature set.

---

## Settings and permissions

Claude Code uses a layered settings system. Higher-precedence settings override lower ones.

### Settings hierarchy

| Scope | File | Committed? | Purpose |
|---|---|---|---|
| Managed | `/etc/claude-code/managed-settings.json` | N/A | IT/org-wide policy |
| Local project | `.claude/settings.local.json` | No (gitignored) | Personal project overrides |
| Shared project | `.claude/settings.json` | Yes | Team settings |
| User | `~/.claude/settings.json` | No | Global personal settings |

### Permission model

The `permissions` object controls what tools the agent can use without asking:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test *)",
      "Bash(git add *)",
      "Bash(git commit *)"
    ],
    "deny": [
      "Bash(curl *)",
      "Read(./.env)",
      "Read(./secrets/**)"
    ]
  }
}
```

**Best practice:** Put team-shared permissions in `.claude/settings.json` (committed) and personal or secret-dependent permissions in `.claude/settings.local.json` (gitignored).

### Example settings.json

See [`examples/settings.json`](examples/settings.json) for a starter template with common permission patterns.

---

## Modular rules

Instead of putting everything in `CLAUDE.md`, you can split rules into `.claude/rules/*.md` files. Each rule file can optionally scope itself to specific file paths using frontmatter:

```markdown
---
paths:
  - "src/api/**/*.ts"
---

# API Development Rules

- All API endpoints must include input validation
- Use the shared error handler from `src/lib/errors.ts`
- Write integration tests for every new endpoint
```

Rules without `paths` frontmatter apply globally. Rules with `paths` only load when the agent is working on matching files.

### Benefits over a single CLAUDE.md

- **Scoped context**: rules load only when relevant, saving context window
- **Team collaboration**: different team members can own different rule files
- **Discoverability**: each rule file is focused on one topic

See [`examples/rules/`](examples/rules/) for example rule files.

---

## MCP servers

[MCP (Model Context Protocol)](https://modelcontextprotocol.io) lets you connect Claude Code to external tools and services.

### Configuration

```bash
# Add an HTTP MCP server (recommended for remote services)
claude mcp add --transport http my-server https://mcp.example.com/mcp

# Add a stdio MCP server (local process)
claude mcp add --transport stdio my-tool -- npx -y my-mcp-server

# Add with environment variables
claude mcp add --transport stdio --env API_KEY=your-key my-tool -- npx -y my-mcp-server

# List configured servers
claude mcp list

# Remove a server
claude mcp remove my-server
```

### Scopes

| Scope | Storage | Shared? |
|---|---|---|
| `local` (default) | `~/.claude.json` | No |
| `project` | `.mcp.json` in project root | Yes (committed) |
| `user` | `~/.claude.json` | No |

### Project-scoped .mcp.json

For team-shared MCP servers, create `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}"
      }
    }
  }
}
```

Environment variable expansion (`${VAR}` and `${VAR:-default}`) is supported.

Codex stores MCP configuration alongside other Codex settings in `config.toml`. You can scope MCP servers to a project by adding them to `.codex/config.toml` for trusted projects.

---

## Examples

The [`examples/`](examples/) directory contains ready-to-use templates:

| File | Description |
|---|---|
| [`CLAUDE.md`](examples/CLAUDE.md) | Starter project memory file |
| [`AGENTS.md`](examples/AGENTS.md) | Starter agent operating guide |
| [`SKILLS.md`](examples/SKILLS.md) | Skill index template |
| [`settings.json`](examples/settings.json) | Team settings with common permissions |
| [`docs-list.ts`](examples/docs-list.ts) | Documentation discovery script |
| [`package.json`](examples/package.json) | npm package with `docs:list` script |
| [`rules/`](examples/rules/) | Example modular rule files |
| [`examples/.codex/rules/`](examples/.codex/rules/) | Example Codex CLI rules (sandbox escape allowlist) |

To use these in your project, copy the files you need and adapt them.

---

## Further reading

- [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Claude Code memory management](https://code.claude.com/docs/en/memory)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Claude Code MCP](https://code.claude.com/docs/en/mcp)
- [Claude Code settings](https://code.claude.com/docs/en/settings)
- [Agent Skills open standard](https://agentskills.io)
- [Oracle — multi-model context bundler](https://github.com/steipete/oracle)
- [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
