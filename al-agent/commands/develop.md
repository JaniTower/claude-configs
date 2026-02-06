---
description: Run development from specification - implement AL code with integrated review and diagnostics
---

# Development Command

Implement AL code from a specification file with integrated self-review and diagnostics fixing.

## Usage

```
/develop
```

No parameters needed - auto-detects formatted spec files in `docs/requirements/`.

**Prerequisite:** Must have at least one `*-formatted.md` file in `docs/requirements/` folder. Use `/plan` first to create one from rough notes.

## What It Does

1. **Finds specification** - Auto-detects or uses provided filename
2. **Creates development task** - Tracks progress
3. **Spawns al-developer** - Implements code with self-review and diagnostics

The al-developer agent handles everything:
- Reads specification from the detected/specified file
- Implements AL code
- Performs self-review (standards, DRY/SOLID, performance)
- Runs compilation and auto-fixes safe diagnostics
- Produces development report

## Workflow

```
docs/requirements/notes.md (rough notes)
    ↓
/plan
    ↓
docs/requirements/notes-formatted.md (structured spec)
    ↓
/develop
    ↓
al-developer agent:
    1. Read spec
    2. Implement AL code
    3. Self-review against standards
    4. Fix any Critical/High issues
    5. Run al-compile
    6. Auto-fix safe diagnostics
    7. Write development report
    ↓
.dev/03-development-report.md
    ↓
Ready for /test or /document
```

## Step 1: Find Specification

```
Auto-detect formatted spec files:
  - Glob for docs/requirements/*-formatted.md files
  - If 1 file found → Use it automatically
  - If 0 files found → Error: "No formatted spec files found. Run /plan first."
  - If 2+ files found → Ask user: "Multiple formatted specs found. Which one should I implement?"
    - List the files
    - Wait for user response before proceeding
```

**Note:** Only `*-formatted.md` files are considered. Raw/unformatted specs are ignored.

## Step 2: Create Task

```
TaskCreate: "Development"
  - description: "Implement AL code per specification"
  - activeForm: "Implementing code"
```

## Step 3: Run Development

```
TaskUpdate: "Development" → status: "in_progress"

Spawn al-developer agent with prompt:
  "Implement the feature specified in [spec-file-path].
   Follow all phases: implement → self-review → diagnostics.
   Write development report to .dev/03-development-report.md"

TaskUpdate: "Development" → status: "completed"
```

## Output Files

- `.dev/03-development-report.md` - Development summary with review findings
- AL code files in `src/` directory
- `.dev/session-log.md` - Updated with activity

## Example Sessions

**Auto-detect (single formatted file):**
```
User: /develop

Claude: Found: docs/requirements/customer-discount-formatted.md

        Starting development...

        [Spawns al-developer]

        Development complete → .dev/03-development-report.md
        ...
```

**Multiple formatted files:**
```
User: /develop

Claude: Multiple formatted specs found in docs/requirements/:
        - customer-discount-formatted.md
        - inventory-tracking-formatted.md

        Which one should I implement?

User: customer-discount-formatted.md

Claude: Using: docs/requirements/customer-discount-formatted.md

        Starting development...
        ...
```

**No formatted files:**
```
User: /develop

Claude: No formatted spec files found in docs/requirements/.

        Run /plan first to create a structured specification.
```

## Creating a Specification

**Recommended workflow:**

1. Create rough notes in `docs/requirements/notes.md`
2. Run `/plan` to structure it
3. Review `docs/requirements/notes-formatted.md`
4. Run `/develop` to implement

**Or** create a properly structured spec directly following the template in `docs-templates/spec-template.md` and name it with `-formatted.md` suffix.

### Required Sections in Formatted Spec

- Feature name and overview
- Functional requirements
- Technical design with object numbers
- **Event Subscribers** (which BC events trigger the logic)
- **Integration Codeunit** (wires subscribers to business logic)

See `docs-templates/spec-template.md` for the full template structure.

## When to Use

- You have a formatted specification ready (`*-formatted.md`)
- You ran `/plan` and reviewed the output
- You're ready to implement the feature

## Next Steps

After `/develop` completes:

1. **Review the report** - Check `.dev/03-development-report.md`
2. **Test the code** - Run `/test` to create automated tests
3. **Generate docs** - Run `/document` for user documentation

---

**Note:** The al-developer agent handles implementation, self-review, and diagnostics in one integrated workflow. Only formatted specs (`*-formatted.md`) are used to ensure proper structure.
