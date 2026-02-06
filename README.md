# Claude Code Configuration Repository

This repository contains reusable Claude Code plugin configurations for streamlining development workflows across multiple projects and computers.

## Overview

This setup allows you to:
- Maintain consistent Claude Code configurations across all your projects
- Sync improvements across multiple computers via GitHub
- Compose multiple plugin profiles for specific project needs
- Keep project-specific customizations separate from shared configurations

## Repository Structure

```
claude-configs/
├── al-agent/    # AL (Business Central) development profile (v3.0.0)
│   ├── .claude-plugin/
│   │   └── plugin.json        # Plugin metadata
│   ├── CLAUDE.md              # AL coding standards and workflow docs
│   ├── commands/              # Slash commands: /plan, /develop, /test, /document
│   ├── agents/                # 5 agents: solution-planner, al-developer, test-engineer, test-reviewer, docs-writer
│   ├── docs-templates/        # Specification templates for your projects
│   ├── rules/                 # AL coding rules (70+ rules with actions)
│   └── .mcp.json              # MCP server configuration (GitHub, MS Docs, Context7)
├── .gitignore
└── README.md (this file)
```

## Quick Start Configuration

Before using these plugins:

1. **Set GitHub token (if using GitHub MCP):**
   - Set `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable
   - Or remove the github MCP server from `.mcp.json` if not needed

All other MCP servers (Microsoft Docs, Context7) work without configuration.

## Setup Instructions

### Initial Setup (First Computer)

1. **Clone this repository:**
   ```bash
   cd ~
   git clone git@github.com:YOUR_USERNAME/claude-configs.git
   ```

2. **That's it!** The plugins are now available on your computer. You'll enable them per-project (see next section).

### Setup on Additional Computers

Simply clone the repository:
```bash
cd ~
git clone git@github.com:YOUR_USERNAME/claude-configs.git
```

Plugins will be available for use in your projects immediately.

### Using Plugins in Projects

In each project where you want to use these plugins, create or edit `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "local": {
      "source": {
        "source": "directory",
        "path": "~/claude-configs"
      }
    }
  },
  "enabledPlugins": {
    "al-agent@local": true
  }
}
```

**Note:** The `~` expands to your home directory automatically.

#### Compose Multiple Profiles

As you add more profiles, you can combine them in a single project:
```json
{
  "extraKnownMarketplaces": {
    "my-configs": {
      "source": {
        "source": "directory",
        "path": "~/claude-configs"
      }
    }
  },
  "enabledPlugins": {
    "al-agent@my-configs": true,
    "profile-al-testing@my-configs": true,
    "profile-devops@my-configs": true
  }
}
```

## Workflow for Updating Configurations

### Making Improvements

When you discover a useful pattern or want to improve the configuration:

```bash
cd ~/claude-configs

# Edit files (e.g., al-agent/CLAUDE.md)
# Add new commands, update patterns, etc.

git add .
git commit -m "Add pattern for handling table extensions"
git push
```

### Syncing to Other Computers

On your other computer(s):

```bash
cd ~/claude-configs
git pull
```

All your projects immediately benefit from the updates without any additional configuration.

## Available Plugins

### al-agent (v3.0.0)

AL (Application Language) development configuration for Microsoft Dynamics 365 Business Central.

**Workflow:** Write spec → `/develop` → Done (integrated self-review and diagnostics)

**Commands:**
- `/plan` - Transform rough notes into structured specification
- `/develop` - Implement code from specification (includes self-review + diagnostics)
- `/test` - Create and review tests for implemented code
- `/document` - Generate user documentation

**Agents:**
- `solution-planner` - Structures rough notes into specs
- `al-developer` - Implements code with integrated quality checks
- `test-engineer` / `test-reviewer` - Test creation and review
- `docs-writer` - Documentation generation

**Also Includes:**
- AL coding rules (70+ rules in `rules/main.ruleset.json`)
- Specification templates (`docs-templates/`)
- MCP servers: GitHub, Microsoft Docs, Context7

**Documentation:** See `al-agent/README.md`

## Adding New Plugins

To create a new plugin profile:

1. **Create plugin directory:**
   ```bash
   cd ~/claude-configs
   mkdir -p profile-name/{.claude-plugin,commands,agents}
   ```

2. **Create plugin.json:**
   ```json
   {
     "name": "profile-name",
     "description": "Brief description of what this profile provides",
     "version": "1.0.0",
     "author": {
       "name": "Your Name"
     }
   }
   ```

3. **Add configuration files:**
   - `CLAUDE.md` - Instructions and standards
   - `commands/*.md` - Custom slash commands
   - `agents/*.md` - Specialized subagents
   - `.mcp.json` - MCP server configuration (optional)

4. **Document in README:**
   - Create `profile-name/README.md`
   - Update this main README

5. **Commit and push:**
   ```bash
   git add profile-name
   git commit -m "Add profile-name plugin"
   git push
   ```

## Project-Specific Customizations

While plugins provide shared configuration, each project can still have its own customizations:

**Project directory structure:**
```
your-project/
├── .claude/
│   ├── settings.json        # Enable plugins + project-specific settings
│   ├── settings.local.json  # Personal overrides (gitignored)
│   ├── CLAUDE.md            # Project-specific instructions
│   └── commands/            # Project-only commands
```

**How it works:**
1. Plugin configurations load first (from `~/claude-configs/`)
2. Project configurations merge on top (from `your-project/.claude/`)
3. Project settings can override plugin defaults
4. All configurations are additive (commands, agents, etc. from all sources are available)

## Configuration Hierarchy

Claude Code loads configurations in this order (later overrides earlier):

1. **Enterprise managed settings** (if configured)
2. **User settings** (`~/.claude/settings.json`)
3. **User plugins** (registered in user settings)
4. **Project settings** (`.claude/settings.json`)
5. **Project plugins** (enabled in project settings)
6. **Local settings** (`.claude/settings.local.json` - gitignored)

## Security Best Practices

- Never commit sensitive data (credentials, API keys, certificates)
- Use `.gitignore` to prevent accidental commits of sensitive files
- Keep authentication in project-local files (`.env`, gitignored)
- Use permission rules in `settings.json` to deny access to sensitive paths

## Troubleshooting

### Plugin not loading

1. Check plugin registration in `~/.claude/settings.json`
2. Verify path is absolute (not relative)
3. Run `/config` in Claude Code to see loaded plugins
4. Check plugin.json syntax is valid JSON

### Conflicts between plugins

- Commands from different plugins are namespaced automatically
- CLAUDE.md files from all plugins are merged
- Settings follow precedence rules (project > user > plugin)

### Changes not appearing

1. Settings hot-reload automatically (no restart needed)
2. For CLAUDE.md changes, start a new session
3. Verify you committed and pushed changes
4. On other computer, verify you pulled latest changes

## Resources

- [Claude Code Documentation](https://docs.claude.com/claude-code)
- [Plugin System Documentation](https://docs.claude.com/claude-code/plugins)
- [Configuration Guide](https://docs.claude.com/claude-code/configuration)

## Contributing

This is a personal configuration repository. If you're working in a team:
- Fork this repository for your own configurations
- Or create a team repository with shared configurations
- Use pull requests to review configuration changes

## License

Personal configuration repository. Use as you see fit.
