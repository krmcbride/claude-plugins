# Claude Code Plugin Marketplace

A personal plugin marketplace for [Claude Code](https://claude.com/code) featuring essential tools for deep investigation, systematic code review, documentation lookup, and code maintenance.

## Quick Start

```bash
/plugin marketplace add krmcbride/claude-plugins
/plugin marketplace list krmcbride
/plugin install essentials@krmcbride
# Restart Claude Code to activate
```

## Available Plugins

### Essentials

Essential tooling for discovering docs, cleaning up code, deep thinking (ultraplan), and systematic code review.

**Features:**

#### UltraPlan - Deep Investigation Methodology

Systematic multi-step investigation of complex problems with confidence-based progression.

```bash
/ultraplan I need help designing a better architecture for...
```

**What it does:**

- Progressive confidence tracking (exploring → low → medium → high → certain)
- Multi-step investigation with state accumulation
- Iterative Haiku agent guidance for next-step suggestions
- Final Sonnet agent validation for comprehensive analysis
- Practical engineering focus with trade-off analysis
- Implementation options with pros/cons (similar to Claude Code plan mode)

#### CodeReview - Systematic Code Analysis

Multi-step code review with severity-classified findings and actionable fixes.

```bash
/codereview src/auth.py src/middleware.py
```

**What it does:**

- Progressive confidence tracking (exploring → low → medium → high → certain)
- Severity classification for all issues (🔴 CRITICAL, 🟠 HIGH, 🟡 MEDIUM, 🟢 LOW)
- Multi-step investigation with state accumulation
- Iterative Haiku agent guidance for coverage validation
- Final Sonnet agent validation for independent review
- Actionable fixes with before/after code examples
- Top 3 priorities with effort estimates
- Quick wins vs. long-term improvements

**Focus areas:**

- Security vulnerabilities (SQL injection, XSS, hardcoded secrets, auth gaps)
- Concurrency issues (race conditions, deadlocks, thread-safety)
- Resource management (memory leaks, unclosed connections)
- Error handling gaps
- Performance bottlenecks (algorithmic complexity, N+1 queries)
- Code quality and maintainability

**Best for:**

- Security audits before production
- Bug investigation and root cause analysis
- Performance bottleneck identification
- Code quality assessment
- Pre-deployment validation

#### Documentation Lookup Skill

Fetches library documentation via Context7's HTTP API using the "code execution" pattern.

**What it does:**

- Proactively activates when you mention libraries or frameworks
- Writes curl commands to fetch documentation from Context7 API
- Two-step process: search for library ID, then fetch docs
- Falls back to web search if needed
- Use: "Check the docs for..."

**Why code execution?** Instead of always loading MCP tool definitions into context, Claude writes API calls only when needed—reducing context usage and demonstrating the pattern from [Anthropic's code execution blog post](https://www.anthropic.com/engineering/code-execution-with-mcp).

#### Code Examples Skill

Find real-world code examples across millions of GitHub repositories using [grep.app](https://grep.app).

**What it does:**

- Finds how others implement specific patterns in production code
- Filters by language, repository, or file path
- Use: "How do others implement X?" or "Show me usage examples for..."

**Complements documentation-lookup:** While documentation-lookup fetches official docs, code-examples finds real-world usage in production code.

#### Code Janitor Agent

Mechanical code maintenance specialist for tedious cleanup work.

**What it does:**

- Fixes linting errors systematically
- Renames variables consistently across files
- Updates import statements
- Fixes formatting issues
- Iterates until all fixable issues are resolved

#### Consensus - Multi-Perspective Decision Analysis

Analyze complex decisions from multiple viewpoints using specialized debate agents.

```bash
/consensus Should we migrate to microservices?
```

**What it does:**

- Launches three parallel Sonnet agents with distinct analytical stances:
  - **FOR** - Advocates for the proposal with optimistic perspective
  - **AGAINST** - Critiques the proposal rigorously
  - **NEUTRAL** - Provides objective analysis weighing evidence
- Presents balanced view of trade-offs and considerations
- Helps avoid confirmation bias in decision-making

## Plugin Development

This marketplace follows the [Claude Code plugin specification](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces.md).

Structure:

```
.claude-plugin/
  marketplace.json       # Marketplace manifest

essentials/
  .claude-plugin/
    plugin.json          # Plugin manifest
  commands/              # Custom slash commands
  agents/                # Specialized subagents
  skills/                # Proactive capabilities
  .mcp.json             # MCP server configurations
```

## Requirements

- Claude Code installed and configured

## License

MIT
