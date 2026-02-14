---
summary: "Step-by-step guide for adding docs:list and Oracle to a project"
read_when:
  - docs:list
  - Oracle
  - documentation discovery
  - multi-model
  - second opinion
  - setup
---

# Adding docs:list and Oracle to Your Project

A practical guide for integrating **docs:list** (documentation discovery) and **Oracle** (multi-model second opinions) into repositories that use agentic coding tools such as Claude Code or OpenAI Codex.

---

## Part 1 — docs:list (documentation discovery)

### What it does

`docs:list` is a convention where every markdown file in your `docs/` directory carries YAML front matter with a short summary and a set of keyword hints. A small script walks the directory and prints a catalog so that an AI agent can decide which docs to read before starting work.

The result: agents stop guessing and start reading.

### Prerequisites

- Node.js 18+
- A `docs/` directory in your project root
- `tsx` (TypeScript execute) as a dev dependency

### Step 1 — Create the script

Save the following to `scripts/docs-list.ts`:

```ts
#!/usr/bin/env tsx

import { readdirSync, readFileSync } from 'node:fs';
import { dirname, join, relative } from 'node:path';
import { fileURLToPath } from 'node:url';

const scriptDir = dirname(fileURLToPath(import.meta.url));
const DOCS_DIR = join(scriptDir, '..', 'docs');

// Directories inside docs/ to skip
const EXCLUDED_DIRS = new Set(['archive', 'research']);

function compactStrings(values: unknown[]): string[] {
  const result: string[] = [];
  for (const value of values) {
    if (value === null || value === undefined) continue;
    const normalized = String(value).trim();
    if (normalized.length > 0) result.push(normalized);
  }
  return result;
}

function walkMarkdownFiles(dir: string, base: string = dir): string[] {
  const entries = readdirSync(dir, { withFileTypes: true });
  const files: string[] = [];
  for (const entry of entries) {
    if (entry.name.startsWith('.')) continue;
    const fullPath = join(dir, entry.name);
    if (entry.isDirectory()) {
      if (EXCLUDED_DIRS.has(entry.name)) continue;
      files.push(...walkMarkdownFiles(fullPath, base));
    } else if (entry.isFile() && entry.name.endsWith('.md')) {
      files.push(relative(base, fullPath));
    }
  }
  return files.sort((a, b) => a.localeCompare(b));
}

function extractMetadata(fullPath: string): {
  summary: string | null;
  readWhen: string[];
  error?: string;
} {
  const content = readFileSync(fullPath, 'utf8');

  if (!content.startsWith('---')) {
    return { summary: null, readWhen: [], error: 'missing front matter' };
  }

  const endIndex = content.indexOf('\n---', 3);
  if (endIndex === -1) {
    return { summary: null, readWhen: [], error: 'unterminated front matter' };
  }

  const frontMatter = content.slice(3, endIndex).trim();
  const lines = frontMatter.split('\n');

  let summaryLine: string | null = null;
  const readWhen: string[] = [];
  let collectingField: 'read_when' | null = null;

  for (const rawLine of lines) {
    const line = rawLine.trim();

    if (line.startsWith('summary:')) {
      summaryLine = line;
      collectingField = null;
      continue;
    }

    if (line.startsWith('read_when:')) {
      collectingField = 'read_when';
      const inline = line.slice('read_when:'.length).trim();
      if (inline.startsWith('[') && inline.endsWith(']')) {
        try {
          const parsed = JSON.parse(inline.replace(/'/g, '"')) as unknown;
          if (Array.isArray(parsed)) {
            readWhen.push(...compactStrings(parsed));
          }
        } catch {
          // ignore malformed inline arrays
        }
      }
      continue;
    }

    if (collectingField === 'read_when') {
      if (line.startsWith('- ')) {
        const hint = line.slice(2).trim();
        if (hint) readWhen.push(hint);
      } else if (line === '') {
        // skip blank lines within the list
      } else {
        collectingField = null;
      }
    }
  }

  if (!summaryLine) {
    return { summary: null, readWhen, error: 'summary key missing' };
  }

  const summaryValue = summaryLine.slice('summary:'.length).trim();
  const normalized = summaryValue
    .replace(/^['"]|['"]$/g, '')
    .replace(/\s+/g, ' ')
    .trim();

  if (!normalized) {
    return { summary: null, readWhen, error: 'summary is empty' };
  }

  return { summary: normalized, readWhen };
}

// Main
console.log('Listing all markdown files in docs folder:\n');

const markdownFiles = walkMarkdownFiles(DOCS_DIR);

if (markdownFiles.length === 0) {
  console.log(
    '  (no docs found — add markdown files to docs/ with front matter)'
  );
} else {
  for (const relativePath of markdownFiles) {
    const fullPath = join(DOCS_DIR, relativePath);
    const { summary, readWhen, error } = extractMetadata(fullPath);
    if (summary) {
      console.log(`  ${relativePath} — ${summary}`);
      if (readWhen.length > 0) {
        console.log(`    Read when: ${readWhen.join('; ')}`);
      }
    } else {
      const reason = error ? ` — [${error}]` : '';
      console.log(`  ${relativePath}${reason}`);
    }
  }
}

console.log(
  '\nReminder: read docs matching your current task before coding. ' +
    'Keep docs updated when behavior or APIs change.'
);
```

### Step 2 — Wire it up in package.json

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

Run `npm install` to pull in `tsx`.

### Step 3 — Add front matter to every doc

Each markdown file in `docs/` needs two fields:

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

| Field | Purpose |
|---|---|
| `summary` | One-line description, under 80 characters |
| `read_when` | Keywords that tell an agent when the doc is relevant |

Tips:
- Be specific. "WebSocket reconnection logic" beats "networking".
- One topic per doc. Agents can read multiple short docs faster than one long one.
- The script skips `archive/` and `research/` by default. Edit `EXCLUDED_DIRS` to change this.

### Step 4 — Tell agents to use it

Add a documentation workflow section to `CLAUDE.md`, `AGENTS.md`, or both:

```markdown
## Documentation workflow
- **Before starting work**: run `npm run docs:list` to see available documentation.
- **Read relevant docs**: check the "Read when" hints and read docs matching your task.
- **Keep docs current**: update docs when behavior or APIs change.
- **Front matter required**: all docs must include `summary` and `read_when` fields.
```

For **Codex**, put the same instructions in the `AGENTS.md` that Codex discovers at session start.

### Step 5 — Verify

```bash
npm run docs:list
```

Expected output:

```
Listing all markdown files in docs folder:

  api.md — REST API endpoints and authentication
    Read when: API; REST; authentication; endpoints
  architecture.md — System architecture and component interactions
    Read when: architecture; system design; component roles

Reminder: read docs matching your current task before coding...
```

### Adapting for projects without Node.js

The pattern does not require TypeScript. The script is just a directory walk with YAML front-matter extraction. You can rewrite it in Python, Bash, or any language your project already uses. The convention (front matter with `summary` + `read_when`, an npm/make script named `docs:list`) is what matters.

---

## Part 2 — Oracle (multi-model second opinions)

### What it does

[Oracle](https://github.com/steipete/oracle) bundles your prompt and selected files into a context package, sends it to a different AI model, and returns its response. This gives your primary agent (Claude Code, Codex, etc.) a second opinion from GPT, Gemini, or any OpenRouter model — useful when stuck, reviewing critical code, or cross-checking decisions.

### Prerequisites

- Node.js 18+
- An API key for at least one provider (OpenAI, Google, Anthropic, or Azure OpenAI)

### Step 1 — Install Oracle

```bash
# Use directly with npx (no permanent install)
npx -y @steipete/oracle --help

# Or install globally
npm install -g @steipete/oracle
```

### Step 2 — Set API keys

Export keys for the providers you want to use:

```bash
# OpenAI
export OPENAI_API_KEY="sk-..."

# Google
export GEMINI_API_KEY="..."

# Anthropic
export ANTHROPIC_API_KEY="sk-ant-..."

# Azure OpenAI (all four are required)
export AZURE_OPENAI_API_KEY="..."
export AZURE_OPENAI_ENDPOINT="https://your-resource.openai.azure.com"
export AZURE_OPENAI_DEPLOYMENT="your-deployment"
export AZURE_OPENAI_API_VERSION="2025-04-01-preview"
```

Add these to your shell profile (`~/.bashrc`, `~/.zshrc`) or a local `.envrc` (if using direnv). Never commit them.

### Step 3 — Test it manually

```bash
# Basic usage — ask a model to review a file
npx -y @steipete/oracle -p "review this code for bugs" --file "src/index.ts"

# Cross-check with two models at once
npx -y @steipete/oracle -p "is this approach correct?" \
  --models gpt-5.1-pro,gemini-3-pro \
  --file "src/auth.ts"

# Copy the context bundle to clipboard (for pasting into a web chat)
npx -y @steipete/oracle --render --copy -p "review this" --file "src/**/*.ts"

# Check status of recent sessions
npx -y @steipete/oracle status --hours 72
```

### Step 4 — Tell agents to use it

Add an Oracle section to `CLAUDE.md`, `AGENTS.md`, or both:

```markdown
## Tools

### Oracle
Oracle bundles prompts and files so another AI model can provide a second opinion
with full context.

**Usage:**
- Run `npx -y @steipete/oracle --help` once per session before first use
- Basic: `npx -y @steipete/oracle -p "your prompt" --file "path/to/files"`
- Multi-model: `npx -y @steipete/oracle -p "prompt" --models gpt-5.1-pro,gemini-3-pro --file "src/**/*.ts"`

**When to use:**
- Stuck on a complex problem and need a fresh perspective
- Reviewing critical code before committing
- Cross-checking architectural decisions across models
- Getting unstuck when the primary agent is repeatedly failing
```

For **Codex**, put the same block in your `AGENTS.md`.

### Step 5 — Allow Oracle in Claude Code permissions

If your project uses Claude Code settings, allow the Oracle command so the agent can run it without prompting:

```json
{
  "permissions": {
    "allow": [
      "Bash(npx -y @steipete/oracle *)"
    ]
  }
}
```

Add this to `.claude/settings.json` (team) or `.claude/settings.local.json` (personal).

### Supported models

| Provider | Example model IDs |
|---|---|
| OpenAI | `gpt-5.1-pro`, `gpt-5.2`, `gpt-5.1-codex` |
| Google | `gemini-3-pro` |
| Anthropic | `claude-4.5-sonnet`, `claude-4.1-opus` |
| OpenRouter | Any OpenRouter model ID |

---

## Putting it all together

A minimal setup adds three things to your project:

1. **`scripts/docs-list.ts`** + the `docs:list` npm script
2. **YAML front matter** on every file in `docs/`
3. **An Oracle section** in `CLAUDE.md` / `AGENTS.md`

Here is what the relevant parts of a configured project look like:

```
your-project/
  CLAUDE.md              # References docs:list and Oracle
  AGENTS.md              # Same — for Codex and other agents
  package.json           # "docs:list": "tsx scripts/docs-list.ts"
  scripts/
    docs-list.ts         # Documentation discovery script
  docs/
    api.md               # Front matter: summary + read_when
    architecture.md
  .claude/
    settings.json        # allow: ["Bash(npx -y @steipete/oracle *)"]
```

### Workflow in practice

1. Agent starts a session.
2. Agent runs `npm run docs:list` and sees which docs are relevant to the task.
3. Agent reads the matching docs.
4. Agent works on the task.
5. When stuck or before committing critical changes, agent calls Oracle for a second opinion from a different model.
6. Agent incorporates feedback and finishes.

This loop works identically in Claude Code and Codex — the only difference is which file the agent reads first (`CLAUDE.md` vs `AGENTS.md`).
