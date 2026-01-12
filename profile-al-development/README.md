# AL Development Profile

Claude Code plugin for Microsoft Dynamics 365 Business Central AL (Application Language) development.

## What's Included

### CLAUDE.md - AL Coding Standards
Comprehensive AL development guidelines including:
- Object and field naming conventions
- Code style best practices
- Event patterns and error handling
- Performance optimization techniques
- Testing guidelines
- Common AL patterns and examples

### MCP Server Configuration
Pre-configured AL MCP server for:
- Searching AL objects in workspace (.app packages)
- Getting object definitions (tables, pages, codeunits, reports)
- Finding references across codebase
- Analyzing AL code structure

### Directory Structure
```
profile-al-development/
├── .claude-plugin/
│   └── plugin.json        # Plugin metadata
├── CLAUDE.md              # AL coding standards
├── commands/              # Custom slash commands (add your own)
├── skills/                # Model-invoked skills (add your own)
├── agents/                # Custom subagents (add your own)
├── .mcp.json              # AL MCP server config
└── README.md              # This file
```

## Usage

### Enable in Your AL Projects

In your AL project's `.claude/settings.json`:
```json
{
  "extraKnownMarketplaces": {
    "my-configs": {
      "source": {
        "source": "directory",
        "path": "/home/stefan/claude-configs"
      }
    }
  },
  "enabledPlugins": {
    "profile-al-development@my-configs": true
  }
}
```

**Note:** Adjust the path if your username is different from `stefan`.

## Customization

### Adding Custom Commands

Create slash commands in the `commands/` directory:

```bash
cd ~/claude-configs/profile-al-development/commands
```

Create a file like `deploy.md`:
```markdown
---
description: Deploy AL extension to sandbox
allowed-tools: ["Bash"]
---

Deploy the current AL extension to the configured sandbox environment.

Run the deployment script and handle any errors appropriately.
```

Use it in any AL project: `/deploy`

### Adding Skills

Create model-invoked skills in the `skills/` directory:

```bash
mkdir -p ~/claude-configs/profile-al-development/skills/al-debugging
cd ~/claude-configs/profile-al-development/skills/al-debugging
```

Create `SKILL.md`:
```markdown
---
name: al-debugging
description: Debug AL runtime errors and provide solutions
allowed-tools: ["Read", "Grep", "mcp__al-mcp-server__al_search_objects"]
---

When debugging AL errors:
1. Analyze the error message
2. Search for related objects in the codebase
3. Identify common AL pitfalls (e.g., RecordRef issues, missing permissions)
4. Suggest fixes with code examples
```

Claude will automatically invoke this skill when debugging AL code.

### Project-Specific Overrides

Each project can override or extend the plugin configuration:

**In your project's `.claude/CLAUDE.md`:**
```markdown
# Project-Specific AL Guidelines

## This Project Uses
- Custom table prefix: ABC
- Special validation rules for customer records
- Integration with external API for pricing

@/home/stefan/claude-configs/profile-al-development/CLAUDE.md
```

The `@` import loads the plugin's CLAUDE.md, and your project-specific instructions augment it.

## Updating the MCP Server Path

If your AL MCP server is installed in a different location, update `.mcp.json`:

```json
{
  "mcpServers": {
    "al-mcp-server": {
      "type": "stdio",
      "command": "node",
      "args": [
        "/path/to/your/al-mcp-server/dist/index.js"
      ],
      "env": {}
    }
  }
}
```

Or use npx for automatic resolution:
```json
{
  "mcpServers": {
    "al-mcp-server": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "@sxkod/al-mcp-server"
      ],
      "env": {}
    }
  }
}
```

## Best Practices

### When to Update This Plugin
- Discovered a useful AL pattern or best practice
- Created a reusable command that works across projects
- Found a better way to structure AL code
- Want to share improvements across all AL projects

### When to Use Project-Specific Config
- Project-specific naming conventions or prefixes
- Client-specific requirements or constraints
- Temporary experimental patterns
- One-off commands for specific projects

### Committing Changes

After making improvements:
```bash
cd ~/claude-configs
git add profile-al-development/
git commit -m "Update AL error handling pattern"
git push
```

On other computers:
```bash
cd ~/claude-configs
git pull
```

All your AL projects across all computers immediately benefit.

## AL Development Tips

### Using the AL MCP Server

The included MCP server provides powerful AL code analysis:

**Search for objects:**
```
@al-mcp-server Search for all tables related to customers
```

**Get object definition:**
```
@al-mcp-server Get the definition of the Customer table
```

**Find references:**
```
@al-mcp-server Find all references to the PostingDate field
```

### Common AL Workflows

**Implementing a table extension:**
1. Claude knows AL naming conventions from CLAUDE.md
2. Creates properly structured table extension
3. Follows field naming standards automatically

**Creating events:**
1. Claude uses standard event subscriber pattern
2. Applies proper event naming from guidelines
3. Includes error handling best practices

**Performance optimization:**
1. Claude references performance patterns from CLAUDE.md
2. Suggests SetLoadFields usage
3. Recommends batch operations when appropriate

## BC Specialist Access

This profile uses CLI-based BC specialist access instead of MCP registration to minimize context overhead.

### Available via Agents

| Agent | Purpose |
|-------|---------|
| `bc-expert` | General BC specialist consultation |
| `bc-knowledge` | Knowledge base queries |
| `bc-code-reviewer` | Code review with Roger Reviewer |

### Direct CLI Usage (if needed)

```bash
# Auto-route question to best specialist
bc-expert ask "How do I optimize table queries?"

# Consult specific specialist
bc-expert talk-to dean-debug "Performance issues"
bc-expert talk-to roger-reviewer "Review this code pattern"
bc-expert talk-to pat-performance "Query optimization"

# Find best specialist
bc-expert who-should-help "Security audit"

# Search knowledge base
bc-expert search "event subscriber patterns"
bc-expert get "<topic-id>"

# List all specialists
bc-expert specialists --json
```

Agents automatically use these commands via Bash tool.

---

## Troubleshooting

### Plugin not loading
```bash
# Verify registration
cat ~/.claude/settings.json

# Check plugin is valid
cat ~/claude-configs/profile-al-development/.claude-plugin/plugin.json
```

### CLAUDE.md not applying
- Start a new Claude Code session
- Check for syntax errors in CLAUDE.md
- Verify file is readable: `cat ~/claude-configs/profile-al-development/CLAUDE.md`

### MCP server not connecting
- Verify MCP server is installed
- Check path in `.mcp.json` is correct
- Test MCP server manually: `node /path/to/al-mcp-server/dist/index.js`

## Contributing

When you make improvements to this profile:
1. Test in a real AL project
2. Document the change in this README if significant
3. Update CLAUDE.md with new patterns
4. Commit with descriptive message
5. Push to sync across computers

## Resources

- [AL Language Documentation](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-programming-in-al)
- [AL Development Best Practices](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-dev-best-practices)
- [Claude Code Plugin System](https://docs.claude.com/claude-code/plugins)
