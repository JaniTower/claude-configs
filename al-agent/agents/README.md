# AL Development Agents

This directory contains 5 specialized agents for the simplified AL development workflow.

## Workflow Diagram

```
docs/requirements/notes.md (rough notes)
     │
     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    SPEC FORMATTING                               │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   solution-planner                          │   │
│  │                                                          │   │
│  │  • Auto-detects .md files (excludes *-formatted.md)      │   │
│  │  • Applies spec-template structure                       │   │
│  │  • Infers event subscribers & integration codeunits      │   │
│  │  • Flags items needing user confirmation                 │   │
│  │                                                          │   │
│  │  Output: docs/requirements/[name]-formatted.md           │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
     │
     ▼
docs/requirements/*-formatted.md (structured spec)
     │
     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    DEVELOPMENT                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    al-developer                           │   │
│  │                                                          │   │
│  │  • Auto-detects *-formatted.md files only                │   │
│  │  1. Read formatted spec                                  │   │
│  │  2. Implement AL code                                    │   │
│  │  3. Self-review (standards, DRY/SOLID, performance)      │   │
│  │  4. Run al-compile                                       │   │
│  │  5. Auto-fix safe diagnostics                            │   │
│  │  6. Write development report                             │   │
│  │                                                          │   │
│  │  Output: AL files + .dev/03-development-report.md        │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
└──────────────────────────────│───────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                    TESTING (Optional)                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────┐      ┌──────────────────────┐        │
│  │    test-engineer     │ ──▶  │    test-reviewer     │        │
│  │                      │      │                      │        │
│  │ Output:              │      │ Output:              │        │
│  │ 05-test-plan.md      │      │ 06-test-review.md    │        │
│  │ + test codeunits     │      │                      │        │
│  └──────────────────────┘      └──────────────────────┘        │
│                                         │                       │
└─────────────────────────────────────────│───────────────────────┘
                                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                    DOCUMENTATION (Optional)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────┐                                       │
│  │     docs-writer      │                                       │
│  │                      │                                       │
│  │ Output:              │                                       │
│  │ docs/ files          │                                       │
│  └──────────────────────┘                                       │
│                              │                                   │
└──────────────────────────────│───────────────────────────────────┘
                               ▼
                          ✓ COMPLETE
```

## Agent Summary

| Agent | Purpose | Input | Output |
|-------|---------|-------|--------|
| `solution-planner` | Structure rough specs | `docs/requirements/*.md` (not *-formatted) | `docs/requirements/*-formatted.md` |
| `al-developer` | Implement + review + fix | `docs/requirements/*-formatted.md` | AL files, `.dev/03-development-report.md` |
| `test-engineer` | Create tests | Code + spec | Test codeunits, `.dev/05-test-plan.md` |
| `test-reviewer` | Review tests | Test code | `.dev/06-test-review.md` |
| `docs-writer` | Generate docs | Code + spec | `docs/` files |

## Tool Access

| Agent | Read | Write | Edit | Glob | Grep | Bash |
|-------|:----:|:-----:|:----:|:----:|:----:|:----:|
| `solution-planner` | ✓ | ✓ | - | ✓ | - | - |
| `al-developer` | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| `test-engineer` | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| `test-reviewer` | ✓ | ✓ | - | ✓ | ✓ | - |
| `docs-writer` | ✓ | ✓ | - | ✓ | ✓ | ✓ |

## Agent Models

| Agent | Model | Reason |
|-------|-------|--------|
| `solution-planner` | sonnet | Text transformation |
| `al-developer` | opus | Complex implementation + review |
| `test-engineer` | sonnet | Test generation |
| `test-reviewer` | sonnet | Test analysis |
| `docs-writer` | sonnet | Documentation generation |

## Commands

| Command | Agent(s) Used |
|---------|---------------|
| `/plan` | solution-planner |
| `/develop` | al-developer |
| `/test` | test-engineer → test-reviewer |
| `/document` | docs-writer |

## Output Files

| Agent | Primary Output | Also Updates |
|-------|----------------|--------------|
| `solution-planner` | `docs/requirements/*.md` | - |
| `al-developer` | AL source files | `.dev/03-development-report.md`, `session-log.md` |
| `test-engineer` | Test codeunits | `.dev/05-test-plan.md`, `session-log.md` |
| `test-reviewer` | `.dev/06-test-review.md` | `session-log.md` |
| `docs-writer` | `docs/` files | `session-log.md` |

---

*For detailed agent documentation, see individual agent files in this directory.*
