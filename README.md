# Claude Plugins

Personal collection of Claude Code hooks, agents, and custom commands.

## What is This?

This repository serves as a **plugin marketplace** for Claude Code. It allows you to organize and share your custom Claude Code extensions including:

- **Hooks**: Automated scripts triggered by Claude Code events (e.g., tool-invoked, user-prompt-submit)
- **Commands**: Custom slash commands (e.g., `/hello`, `/deploy`)
- **Agents**: Specialized AI agents with specific expertise

## Installation

To use plugins from this marketplace in Claude Code:

```bash
# Add this marketplace to your Claude Code
/plugin add-marketplace https://github.com/krmcbride/claude-plugins

# Or if using locally
/plugin add-marketplace /path/to/claude-plugins

# Install a specific plugin
/plugin install example-command

# List available plugins
/plugin list
```

## Repository Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace configuration
├── plugins/
│   ├── example-hook/
│   │   └── plugin.json            # Hook plugin manifest
│   └── example-command/
│       ├── plugin.json            # Command plugin manifest
│       └── hello.md               # Command implementation
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

Specialized agents with specific expertise (coming soon).

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