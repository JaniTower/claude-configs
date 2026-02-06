# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a configuration repository for Claude Code plugins. It contains reusable plugin profiles that can be synced across multiple projects and computers via GitHub. The repository does not contain application code - it contains plugin configurations, custom commands, specialized agents, and MCP server configurations.

## Architecture

### Plugin System

This repository uses Claude Code's plugin architecture where:

1. **Plugin profiles** are self-contained directories with a `.claude-plugin/plugin.json` file
2. **Plugins are registered** in project `.claude/settings.json` files via `extraKnownMarketplaces` and `enabledPlugins`
3. **Multiple plugins can be composed** together in a single project
4. **Configuration is additive** - all plugins load together with project-specific settings

### Repository Structure

```
claude-configs/
├── al-agent/          # AL/Business Central development profile (v1.0.0)
│   ├── .claude-plugin/
│   │   └── plugin.json              # Plugin metadata (name, version, author)
│   ├── CLAUDE.md                    # AL coding standards and workflow documentation
│   ├── agents/                      # 5 specialized agents for AL development
│   │   ├── solution-planner.md      # Transforms rough notes into structured specs
│   │   ├── al-developer.md          # Implements code + self-review + diagnostics
│   │   ├── test-engineer.md         # Creates test codeunits
│   │   ├── test-reviewer.md         # Reviews test quality
│   │   └── docs-writer.md           # Generates documentation
│   ├── commands/                    # Slash commands (user-invocable)
│   │   ├── plan.md                  # Transform notes into structured spec
│   │   ├── develop.md               # Implement code from spec
│   │   ├── test.md                  # Create and review tests
│   │   └── document.md              # Generate documentation
│   ├── docs-templates/              # Specification templates
│   │   ├── spec-template.md         # Full spec template
│   │   └── spec-minimal-example.md  # Minimal example
│   ├── rules/                       # AL coding rules
│   │   └── main.ruleset.json        # 70+ AL rules with actions
│   ├── .mcp.json                    # MCP server configuration
│   └── README.md                    # Profile documentation
├── .gitignore
└── README.md                        # Repository overview and setup
```

## Key Concepts

### Spec-Driven Development (AL Profile v1.0.0)

The AL profile implements a simplified spec-driven workflow where:

1. **You write the spec** - Create feature specification in `docs/requirements/`
2. **Agent implements** - Run `/develop` and the al-developer handles everything
3. **Integrated quality** - Self-review and diagnostics are built into implementation
4. **Document-driven output** - All work documented in `.dev/` directory

### Agent Workflow

```
docs/requirements/notes.md (rough notes/ideas)
    ↓
/plan → solution-planner
    ↓
docs/requirements/notes-formatted.md (structured spec)
    ↓
/develop → al-developer (implements + self-reviews + fixes diagnostics)
    ↓
.dev/03-development-report.md + AL source files
    ↓
(optional) /test → test-engineer → test-reviewer
    ↓
(optional) /document → docs-writer
```

### MCP Server Integration

The AL profile uses three MCP servers:

1. **GitHub MCP** (`github`)
   - GitHub integration via @modelcontextprotocol/server-github
   - Repository operations, issues, pull requests

2. **Microsoft Docs MCP** (`microsoft_docs_mcp`)
   - Official AL/BC documentation lookup
   - HTTP-based MCP server at learn.microsoft.com

3. **Context7 MCP** (`context7`)
   - Library documentation lookup
   - Up-to-date code examples for any framework

## Common Development Tasks

### Adding a New Plugin Profile

```bash
cd ~/claude-configs
mkdir -p profile-name/{.claude-plugin,commands,agents}

# Create plugin.json
cat > profile-name/.claude-plugin/plugin.json <<EOF
{
  "name": "profile-name",
  "description": "Brief description",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  }
}
EOF

# Add configuration files
# - profile-name/CLAUDE.md
# - profile-name/commands/*.md
# - profile-name/agents/*.md
# - profile-name/.mcp.json (if needed)

git add profile-name/
git commit -m "Add profile-name plugin"
git push
```

### Updating an Existing Plugin

```bash
cd ~/claude-configs

# Edit files (e.g., al-agent/CLAUDE.md)
# Make improvements to agents, commands, or instructions

git add al-agent/
git commit -m "Improve [specific aspect]"
git push

# On other computers
git pull  # Changes immediately available to all projects
```

### Creating a Command

Commands are user-invocable slash commands stored in `commands/*.md`:

```markdown
# Command: /command-name

Brief description of what this command does.

## Implementation

[Detailed instructions for Claude on how to execute this command]
```

### Creating an Agent

Agents are autonomous subprocesses stored in `agents/*.md`:

```markdown
# Agent: agent-name

Role description and purpose.

## Input

What this agent reads (e.g., .dev/01-requirements.md)

## Output

What this agent produces (e.g., .dev/02-solution.md)

## Process

[Detailed steps the agent should follow]
```

### Testing Plugin Changes

```bash
# In a test AL project
cd ~/path/to/test-project

# Ensure plugin is enabled in .claude/settings.json
cat .claude/settings.json

# Start Claude Code and test the change
claude

# Test specific command
/command-name "test input"
```

## Configuration Hierarchy

Claude Code loads configurations in this order (later overrides earlier):

1. Enterprise managed settings (if configured)
2. User settings (`~/.claude/settings.json`)
3. User plugins (registered in user settings)
4. **Project settings** (`.claude/settings.json`)
5. **Project plugins** (enabled in project settings) ← This repository's plugins load here
6. Local settings (`.claude/settings.local.json` - gitignored)

## File Naming Conventions

- **Commands**: `commands/command-name.md` (kebab-case)
- **Agents**: `agents/agent-name.md` (kebab-case)
- **Config**: `.claude-plugin/plugin.json`, `.mcp.json`
- **Documentation**: `CLAUDE.md` (uppercase), `README.md`

## MCP Configuration Structure

MCP servers are configured in `.mcp.json` at the plugin root:

```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio|http|sse",
      "command": "executable",
      "args": ["arg1", "arg2"],
      "env": {
        "VAR_NAME": "value"
      }
    }
  }
}
```

## Git Workflow

This repository should be cloned to `~/claude-configs/` and kept synchronized:

```bash
# Initial setup
cd ~
git clone git@github.com:YOUR_USERNAME/claude-configs.git

# Regular sync
cd ~/claude-configs
git pull  # Get updates from other computers
# ... make changes ...
git add .
git commit -m "Descriptive message"
git push  # Share with other computers
```

## Security Considerations

- Never commit credentials, API keys, or certificates
- Use `.gitignore` to prevent accidental commits
- Keep authentication in project-local files (`.env`, gitignored)
- The `.gitignore` already excludes common sensitive patterns

## AL Profile Specifics (v1.0.0)

### Simplified Workflow

The AL profile v1.0.0 consolidates the development pipeline:

1. **Write spec** - Create your specification in `docs/requirements/`
2. **Run `/develop`** - al-developer handles implementation, self-review, and diagnostics
3. **Optional testing** - Run `/test` for test generation and review
4. **Optional docs** - Run `/document` for user documentation

### Integrated Quality

The al-developer agent performs self-review as part of implementation:

```
al-developer
├── Read spec
├── Implement AL code
├── Self-review (standards, DRY/SOLID, performance)
├── Run al-compile
├── Auto-fix safe diagnostics
└── Write development report
```

No separate code-reviewer or diagnostics-fixer phases - quality is integrated.

### AL Coding Rules

The profile includes `rules/*.json` with 70+ AL coding rules:
- `"Error"` = Must follow - code violating these is rejected
- `"Warning"` = Should follow - violations are noted
- `"None"` = Ignored - rule is disabled

### AL Compilation

Use `al-compile` wrapper which auto-detects:
- VS Code AL extension and compiler version
- Workspace structure (single vs multi-app)
- All `.alpackages` directories
- Ruleset files
- Standard analyzers

Always use `al-compile` instead of manual AL compiler commands.

## Plugin Version Management

Plugins use semantic versioning in `plugin.json`:

- **Major**: Breaking changes (e.g., renamed commands, removed agents)
- **Minor**: New features (e.g., new commands, enhanced agents)
- **Patch**: Bug fixes, documentation improvements

Update the version in `plugin.json` and document changes in the profile's README.md.

## Troubleshooting

### Plugin Not Loading

1. Verify registration in project `.claude/settings.json`
2. Check `extraKnownMarketplaces` path is absolute
3. Validate `plugin.json` syntax
4. Run `/config` in Claude Code to see loaded plugins

### MCP Server Issues

1. Check `.mcp.json` syntax
2. Verify executable paths (e.g., `bc-code-intelligence-mcp` in PATH)
3. Test MCP servers independently
4. Check environment variables are set correctly

### Command Not Found

1. Ensure plugin is enabled in project settings
2. Command files must be in `commands/` directory
3. Command names are kebab-case without `.md` extension
4. Restart Claude Code session if needed

### Changes Not Appearing

1. Settings and CLAUDE.md hot-reload automatically
2. For command/agent changes, start a new Claude Code session
3. Verify changes are committed and pushed
4. On other computers, verify `git pull` was run

## Best Practices

1. **Test before pushing** - Verify changes work in a test project
2. **Clear commit messages** - Describe what changed and why
3. **Update documentation** - Keep READMEs in sync with changes
4. **Semantic versioning** - Increment version in plugin.json
5. **Backward compatibility** - Avoid breaking changes when possible
6. **Scope plugins narrowly** - One technology/domain per plugin
7. **Document agent inputs/outputs** - Clear data flow in agent definitions
8. **Use approval gates** - Stop for user validation at major decision points
