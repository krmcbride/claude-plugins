# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Claude Code plugin marketplace. All changes must maintain compatibility with the Claude Code plugin marketplace specification.

## Required Marketplace Structure

### Root Directory Requirements

**MUST HAVE:**

- `.claude-plugin/marketplace.json` - Marketplace manifest (required at repository root)

### Marketplace Manifest Schema (`marketplace.json`)

**Required fields:**

```json
{
  "name": "string", // Kebab-case identifier, no spaces
  "owner": {
    "name": "string" // Maintainer name
  },
  "plugins": [] // Array of plugin entries
}
```

**Optional fields:**

- `description`: Brief marketplace overview
- `version`: Semantic version
- `metadata.pluginRoot`: Base path for relative plugin sources

### Plugin Entry Schema (in marketplace.json)

**Required fields:**

```json
{
  "name": "string", // Kebab-case identifier, no spaces
  "source": "string|object" // Plugin location (see below)
}
```

**Source types:**

- Relative path: `"./plugin-name"`
- GitHub: `{"source": "github", "repo": "owner/repo"}`
- Git URL: `{"source": "url", "url": "https://..."}`

**Optional fields:**

- `description`, `version`, `author`, `homepage`, `repository`, `license`
- `keywords`, `category`, `tags`
- `strict`: Boolean (default: true) - controls manifest handling
- Component paths: `commands`, `agents`, `skills`, `hooks`, `mcpServers`

**Strict mode behavior:**

- `strict: true` (default): Plugin must have `.claude-plugin/plugin.json`; marketplace entry supplements it
- `strict: false`: Plugin's `.claude-plugin/plugin.json` is optional; marketplace entry serves as complete manifest

## Required Plugin Structure

### Plugin Directory Layout

**MUST HAVE:**

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json          # Required when strict=true (default)
```

**OPTIONAL components** (at plugin root, not inside `.claude-plugin/`):

```
├── commands/                # Custom slash commands (markdown files)
├── agents/                  # Custom subagents (markdown files with frontmatter)
├── skills/                  # Agent skills (directories containing SKILL.md)
├── hooks/
│   └── hooks.json          # Event handlers
└── .mcp.json               # MCP server configurations
```

### Plugin Manifest Schema (`plugin.json`)

**Required fields:**

```json
{
  "name": "string", // Must match marketplace entry
  "description": "string",
  "version": "string", // Semantic version
  "author": {
    "name": "string"
  }
}
```

**Optional fields:** Same as marketplace plugin entry optional fields

## Agent (Subagent) Requirements

**File location:** `agents/*.md` (markdown files at plugin root)

**File format:** Markdown with YAML frontmatter

**Required frontmatter:**

```yaml
---
name: agent-identifier # Lowercase letters and hyphens only
description: string # Natural language purpose and invocation context
---
```

**Optional frontmatter:**

- `tools`: Comma-separated list of allowed tools (omit to inherit all tools)
- `model`: Model alias (sonnet/opus/haiku) or 'inherit'

**Markdown body:** System prompt with detailed instructions, examples, and constraints

**Best practices:**

- Single, focused responsibility per agent
- Action-oriented descriptions for proactive invocation
- Only grant necessary tools for security

## Skill Requirements

**Directory structure:** `skills/skill-name/` (each skill is a directory)

**MUST HAVE:** `SKILL.md` file in skill directory

**File format:** Markdown with YAML frontmatter

**Required frontmatter:**

```yaml
---
name: skill-identifier # Lowercase letters, numbers, hyphens; max 64 chars
description: string # What it does and when to use it; max 1024 chars
---
```

**Optional frontmatter:**

- `allowed-tools`: Comma-separated list (e.g., `Read, Grep, Glob`)

**Markdown body:** Instructions and examples

**Supporting files** (optional):

- Additional documentation, scripts, templates in skill directory
- Use forward slashes (Unix-style) for cross-platform path compatibility

**Critical description requirements:**

- Include specific keywords users would mention
- Specify both capabilities AND trigger conditions
- Example: "Analyze Excel spreadsheets, generate pivot tables, create charts. Use when working with .xlsx files"

**Activation:** Skills are model-invoked (Claude decides when to use them based on description)

## Command Requirements

**File location:** `commands/*.md` (markdown files, subdirectories allowed for namespacing)

**File format:** Markdown with optional frontmatter

**Required frontmatter:**

- `description`: Brief explanation of command purpose

**Features:**

- Arguments via placeholders: `{arg1}`
- Bash integration support
- File references allowed
- Invocation: `/plugin-name:command-name` or `/command-name` if no conflicts

## Hook Requirements

**File location:** `hooks/hooks.json`

**Schema:**

```json
{
  "description": "string",
  "hooks": {
    "PostToolUse": [
      // Event type
      {
        "matcher": "Write|Edit", // Tool name pattern (case-sensitive, regex, or *)
        "hooks": [
          {
            "type": "command", // or "prompt"
            "command": "string", // Bash command
            "timeout": 30 // Optional seconds
          }
        ]
      }
    ]
  }
}
```

**Environment variables available:**

- `${CLAUDE_PLUGIN_ROOT}`: Absolute path to plugin directory
- `${CLAUDE_PROJECT_DIR}`: Project root directory

## MCP Server Configuration

**File location:** `.mcp.json` at plugin root

**Schema:**

```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio",
      "command": "string",
      "args": ["array"],
      "env": {}
    }
  }
}
```

**Behavior:** MCP servers automatically configure when plugin is installed

## Naming Conventions

**All identifiers** must use:

- Lowercase letters
- Numbers (where applicable)
- Hyphens for word separation
- No spaces or special characters

**Examples:**

- ✅ `code-janitor`, `documentation-lookup`, `my-plugin`
- ❌ `Code_Janitor`, `documentationLookup`, `my plugin`

## Adding New Plugins to Marketplace

**Process:**

1. Create plugin directory with required `.claude-plugin/plugin.json`
2. Add components: `agents/`, `skills/`, `commands/`, etc. (all optional)
3. Add plugin entry to `.claude-plugin/marketplace.json`
4. Ensure `name` field matches in both `marketplace.json` and `plugin.json`
5. Use relative path in `source` field: `"./plugin-name"`

**Testing workflow:**

```bash
# Add marketplace locally
/plugin marketplace add krmcbride file:///home/kevin/Sites/GitHub/krmcbride/claude-plugins

# Or test via GitHub
/plugin marketplace add krmcbride https://github.com/krmcbride/claude-plugins

# List plugins
/plugin marketplace list krmcbride

# Install and test
/plugin install plugin-name@krmcbride
```
