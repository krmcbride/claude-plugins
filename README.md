# Claude Plugins

Personal collection of Claude Code plugins with MCP servers, agents, and automation tools.

## What is This?

This repository serves as a **plugin marketplace** for Claude Code. It provides productivity plugins that integrate:

- **MCP Servers**: Model Context Protocol servers for extended capabilities
- **Agents**: Specialized AI agents with specific expertise
- **Hooks**: Automated scripts triggered by Claude Code events
- **Commands**: Custom slash commands for common workflows

## Available Plugins

### Essentials
Essential tools for any project:
- **context7 MCP**: Enhanced context and code understanding
- **code-janitor agent**: Automated code cleanup using Claude 3.5 Haiku

### Zen
Browser automation and testing:
- **zen-browser MCP**: Zen browser automation tools
- Specialized agents for common browser automation tasks (coming soon)

### Observability
DevOps and monitoring tools:
- **k8s MCP**: Kubernetes cluster management and monitoring via [krmcbride/mcp-k8s](https://github.com/krmcbride/mcp-k8s)

## Installation

To use plugins from this marketplace in Claude Code:

```bash
# Add this marketplace to your Claude Code
/plugin add-marketplace https://github.com/krmcbride/claude-plugins

# Or if using locally
/plugin add-marketplace /path/to/claude-plugins

# Install a specific plugin
/plugin install essentials

# List available plugins
/plugin list
```

## Repository Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace configuration
├── plugins/
│   ├── essentials/
│   │   ├── plugin.json           # MCP servers & agents
│   │   └── code-janitor.md       # Agent prompt
│   ├── zen/
│   │   └── plugin.json           # Browser automation MCP
│   └── observability/
│       └── plugin.json           # K8s MCP server
└── README.md
```

## Creating Your Own Plugins

### 1. Create Plugin Directory

```bash
mkdir -p plugins/my-plugin
```

### 2. Create plugin.json Manifest

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What your plugin does",
  "author": {
    "name": "Your Name"
  },
  "commands": {
    "mycommand": {
      "description": "Description of the command",
      "path": "mycommand.md"
    }
  }
}
```

### 3. Create Command File (if applicable)

Create `mycommand.md` with the prompt/instructions for your command.

### 4. Add to marketplace.json

Update `.claude-plugin/marketplace.json` to include your new plugin:

```json
{
  "plugins": [
    {
      "name": "my-plugin",
      "source": "./plugins/my-plugin",
      "description": "What your plugin does",
      "version": "1.0.0",
      "category": "commands",
      "tags": ["tag1", "tag2"]
    }
  ]
}
```

## Plugin Types

### Hooks

Hooks are shell commands that run in response to Claude Code events:

```json
{
  "hooks": {
    "tool-invoked": {
      "description": "Runs after tools are invoked",
      "command": "echo 'Tool used!'"
    },
    "user-prompt-submit": {
      "description": "Runs when user submits a prompt",
      "command": "./scripts/log-prompt.sh"
    }
  }
}
```

### Commands

Custom slash commands that extend Claude's capabilities:

```json
{
  "commands": {
    "deploy": {
      "description": "Deploy the application",
      "path": "deploy.md"
    }
  }
}
```

### Agents

Specialized agents with specific expertise and custom models:

```json
{
  "agents": {
    "code-janitor": {
      "description": "Cleanup agent for fixing lint errors",
      "path": "code-janitor.md",
      "model": "claude-3-5-haiku-20241022"
    }
  }
}
```

### MCP Servers

Model Context Protocol servers extend Claude's capabilities:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@uplink-ai/mcp-context7@latest"]
    },
    "k8s": {
      "command": "npx",
      "args": ["-y", "krmcbride/mcp-k8s"]
    }
  }
}
```

## Marketplace Configuration

The `.claude-plugin/marketplace.json` file defines your marketplace:

- **name**: Unique identifier for your marketplace
- **owner**: Contact information
- **metadata**: Description, version, repository URL
- **plugins**: Array of available plugins

## Resources

- [Claude Code Plugin Documentation](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- [Plugin Manifest Schema](https://docs.claude.com/en/docs/claude-code/plugins)

## License

See [LICENSE](LICENSE) file for details.