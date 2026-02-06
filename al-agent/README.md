# AL Development Profile - Simplified Workflow

**Version:** 3.0.0

Claude Code profile for Microsoft Dynamics 365 Business Central AL development with a simplified spec-driven workflow.

## Overview

Write your feature specification in `docs/requirements/`, run `/develop`, and the al-developer agent handles implementation, self-review, and diagnostics in one integrated pass.

## Key Features

- **Spec-Driven Development** - Write spec first, agent implements
- **Integrated Quality** - Self-review and diagnostics built into development
- **Document-Driven Output** - All work documented in `.dev/` directory
- **Simple Commands** - Just 3 commands: `/develop`, `/test`, `/document`
- **4 Focused Agents** - al-developer, test-engineer, test-reviewer, docs-writer

## Quick Start

### 1. Enable in Your AL Project

In your AL project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "my-configs": {
      "source": {
        "source": "directory",
        "path": "/home/user/claude-configs"
      }
    }
  },
  "enabledPlugins": {
    "al-agent@my-configs": true
  }
}
```

### 2. Write Your Specification

Create your spec in `docs/requirements/` in your project:

```markdown
# Feature: Customer Credit Limit

Add credit limit validation to customers.

## Requirements

- Add CreditLimit field to Customer table (Decimal)
- Validate credit limit is not negative
- Block sales posting if customer exceeds credit limit
- Show credit limit on Customer Card

## Object Numbers

- Table Extension: 50100
- Codeunit: 50100
- Page Extension: 50100
```

### 3. Run Development

```
/develop
```

The al-developer agent will:
1. Read your spec
2. Implement AL code
3. Self-review against standards (DRY, SOLID, performance)
4. Auto-fix safe diagnostics
5. Produce a development report

### 4. (Optional) Create Tests

```
/test
```

### 5. (Optional) Generate Documentation

```
/document
```

## Available Commands

| Command | Description |
|---------|-------------|
| `/develop` | Implement code from spec in `docs/requirements/` (auto-detect or specify) |
| `/develop file.md` | Implement from specific spec file |
| `/test` | Create tests for implemented code |
| `/document` | Generate user documentation |

## Agents

| Agent | Purpose |
|-------|---------|
| `al-developer` | Implement code + self-review + diagnostics |
| `test-engineer` | Create test codeunits |
| `test-reviewer` | Review test quality |
| `docs-writer` | Generate documentation |

## Workflow Diagram

```
docs/requirements/*.md (you write your spec)
    ↓
/develop [optional-filename]
    ↓
al-developer
├── Reads spec
├── Implements AL code
├── Self-reviews (standards, DRY, SOLID)
├── Runs al-compile
├── Auto-fixes safe diagnostics
└── Writes development report
    ↓
.dev/03-development-report.md
    ↓
(optional) /test → test-engineer → test-reviewer
    ↓
(optional) /document → docs-writer
```

## Output Files

```
.dev/
├── 03-development-report.md  # Development summary
├── 05-test-plan.md           # Test strategy (from /test)
├── 06-test-review.md         # Test review (from /test)
└── session-log.md            # Activity log
```

## Writing Good Specifications

### Minimal Spec

```markdown
# Feature Name

Brief description.

## Requirements
- Requirement 1
- Requirement 2

## Object Numbers
- Table Extension: 50100
```

### Detailed Spec

Include:
- Functional requirements
- Technical design (object list, key procedures)
- Non-functional requirements (performance, security)
- Edge cases to handle

## AL Coding Rules

This profile includes `rules/*.json` with AL coding rules that the agent enforces.

- `"Error"` = Must follow
- `"Warning"` = Should follow
- `"None"` = Ignored

**To customize:** Edit `rules/main.ruleset.json` or add more `.json` files to `rules/`.

## AL Coding Standards

This profile enforces BC best practices:

- **PascalCase** naming
- **Table extensions** over base modifications
- **Event subscribers** for base app integration
- **SetLoadFields** for performance
- **DataClassification** on all fields
- **ApplicationArea** on all controls

See `CLAUDE.md` for complete standards.

## Directory Structure

```
al-agent/
├── .claude-plugin/
│   └── plugin.json           # Plugin metadata
├── agents/
│   ├── al-developer.md       # Main development agent
│   ├── test-engineer.md      # Test creation
│   ├── test-reviewer.md      # Test review
│   └── docs-writer.md        # Documentation
├── commands/
│   ├── develop.md            # /develop command
│   ├── test.md               # /test command
│   └── document.md           # /document command
├── docs-templates/
│   ├── spec-template.md      # Full spec template
│   └── spec-minimal-example.md  # Minimal example
├── rules/
│   └── main.ruleset.json     # AL coding rules (70+ rules)
├── CLAUDE.md                 # Profile instructions
└── README.md                 # This file
```

## Spec Templates

Copy a template to your AL project's `docs/requirements/`:

```bash
# Create the folder
mkdir -p docs/requirements

# Full template with all sections
cp ~/claude-configs/al-agent/docs-templates/spec-template.md ./docs/requirements/

# Or minimal example
cp ~/claude-configs/al-agent/docs-templates/spec-minimal-example.md ./docs/requirements/
```

## Typical Session

```bash
# 1. Write your spec
vim docs/requirements/my-feature.md

# 2. Run development
/develop

# Output:
# Development complete → .dev/03-development-report.md
# Files created:
# - src/Tables/Tab-Ext50100.CustomerExt.al
# - src/Codeunits/Cod50100.CreditLimitMgt.al
# Self-review: Passed
# Compilation: Clean

# 3. Create tests (optional)
/test

# 4. Generate docs (optional)
/document

# 5. Done! Commit your changes
```

## Requirements

- Claude Code CLI
- AL Language extension
- BC development environment

## Contributing

Improvements benefit all your AL projects:

```bash
cd ~/claude-configs
git add al-agent/
git commit -m "Improve [aspect]"
git push
```

## Resources

- [AL Language Documentation](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-programming-in-al)
- [BC Best Practices](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-dev-best-practices)

---

**Simplified AL development: write spec, run /develop, done.**
