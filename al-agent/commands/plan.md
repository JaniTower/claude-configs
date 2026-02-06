---
description: Transform rough notes or unstructured specs into well-structured specification files.
user-invocable: true
---

# /plan

Transform an unstructured or incomplete specification file into a well-structured spec following the standard template.

## Usage

```
/plan
```

No parameters needed - the agent auto-detects files in `docs/requirements/`.

## Auto-Detection

- **1 file found** → Uses it automatically
- **0 files found** → Error: "No .md files found"
- **2+ files found** → Asks which one to format

Files ending in `-formatted.md` are excluded from detection.

## What It Does

1. Reads your rough notes/unstructured spec
2. Extracts requirements, technical details, and constraints
3. Infers event subscribers and integration points
4. Structures everything into the standard spec template
5. Creates a new file in `docs/requirements/`
6. Reports what was extracted vs. inferred

## Example

**Single file scenario:**
```
User: /plan

Claude: Found: docs/requirements/notes.md
        Formatting...

        Structured spec created:
          Source: docs/requirements/notes.md
          Output: docs/requirements/notes-formatted.md
        ...
```

**Multiple files scenario:**
```
User: /plan

Claude: Multiple .md files found in docs/requirements/:
        - notes.md
        - ideas.md

        Which one should I format?

User: notes.md

Claude: Formatting docs/requirements/notes.md...
        ...
```

## Implementation

Spawn the `solution-planner` agent:

```
Agent: solution-planner
Prompt: Format the specification in docs/requirements/
```

The agent will:
1. Auto-detect file in `docs/requirements/` (or ask if multiple)
2. Read the source file
3. Read `docs-templates/spec-template.md` for structure
4. Transform and structure the content
5. Write to `docs/requirements/[name]-formatted.md`
6. Report what needs user confirmation

## After Formatting

1. Review the generated spec in `docs/requirements/`
2. Confirm or adjust object numbers
3. Verify event subscribers are correct
4. Delete original file or rename formatted file
5. Run `/develop` to implement

## Notes

- Auto-detects source file in `docs/requirements/`
- Excludes `-formatted.md` files from detection
- Output file created with `-formatted` suffix
- Original file is preserved (not overwritten)
- The agent infers event subscribers from requirement patterns
- Object numbers are suggested but should be confirmed
- Ambiguous requirements are flagged for clarification
