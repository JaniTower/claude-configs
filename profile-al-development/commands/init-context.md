# Command: /init-context

Initialize or update the project context document that speeds up all workflows.

## Purpose

Creates `.dev/project-context.md` by exploring the project once and documenting:
- Project structure
- Key objects and their purposes
- Architectural patterns
- Base app integration points
- Common code locations

This document is then used by ALL agents to avoid redundant exploration, reducing workflow runtime by 40-60%.

## Implementation

### Step 1: Check if Context Exists

```bash
if [ -f .dev/project-context.md ]; then
    Read existing context
    Ask user: [Update] [Regenerate] [Cancel]
else
    Proceed to creation
fi
```

### Step 2: Explore Project Structure

**Use Task tool with subagent_type=Explore (thoroughness: medium):**

```
Explore the AL project and document:

1. Directory structure (where are tables, pages, codeunits, etc.)
2. All custom objects (tables, table extensions, pages, page extensions, codeunits, reports, enums)
   - Object IDs
   - Object names
   - Brief purpose (from code or name)

3. Architectural patterns:
   - How are validations implemented? (OnValidate, events, dedicated codeunits?)
   - How are extensions structured? (field naming, placement)
   - Error handling patterns (Error() vs Message() usage)

4. Base app integration:
   - Which base tables are extended? (find table extensions, note base table)
   - Which events are subscribed to? (find EventSubscriber attributes)
   - Which base app procedures are called? (look for Database:: references)

5. Common code locations:
   - Where is validation logic?
   - Where are posting extensions?
   - Where is UI customization?

6. Dependencies:
   - Internal (which codeunits depend on others)
   - External (any dependencies.json references)

Write findings to .dev/project-context.md using the template from .dev-templates/project-context.md
```

### Step 3: Validate Context

Read the generated context and check:
- All sections have content (not just templates)
- At least 3 objects documented
- Structure makes sense

### Step 4: Confirm

```
Project context initialized → .dev/project-context.md

Documented:
- [N] tables/extensions
- [N] pages/extensions
- [N] codeunits
- [N] base app integration points
- [N] architectural patterns

This context will speed up all future workflows by 40-60%.
Agents will read this first before exploring.

Update this context when you add significant new patterns or objects:
/init-context (will offer to update, not regenerate)
```

## Update Mode

If context exists and user chooses Update:

1. Read existing context
2. Scan project for new objects (compare IDs)
3. Append new findings to appropriate sections
4. Don't rewrite existing content
5. Add timestamp to "Recent Changes Log"

## When to Run

**Run once per project** (first time using AL profile)
**Update when:**
- Adding new architectural patterns
- Major refactoring
- Significant new features added
- Project structure changes

## Integration

This command is automatically suggested if:
- User runs `/dev-cycle` or `/plan`
- `.dev/project-context.md` doesn't exist
- Agents detect they're doing excessive exploration

```
"No project context found. Would you like me to create one?
This one-time setup (2-3 min) will speed up all future workflows by 40-60%."

[Yes, Create Context] [Skip for Now]
```

## Technical Notes

- Uses Explore agent (medium thoroughness)
- First-time cost: 2-4 minutes
- Ongoing benefit: Saves 5-15 minutes per workflow
- ROI: Breaks even after 1-2 workflows

---

**This is infrastructure work that pays dividends immediately.**
