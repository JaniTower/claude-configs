# AL Development Configuration

This memory file contains AL (Application Language) development guidelines and patterns for Microsoft Dynamics 365 Business Central.

## MCP Tool Isolation & CLI Tools (IMPORTANT)

**RULE: Minimize MCP overhead. Use CLI tools where available; delegate MCP calls to agents.**

This reduces context bloat and persists findings to files.

### Tool Delegation

| Need | Use |
|------|-----|
| BC specialist advice | `bc-expert` CLI via Bash (see below) |
| BC knowledge base | `bc-knowledge` agent |
| BC code review | `bc-code-reviewer` agent |
| Microsoft docs lookup | `mcp-docs-lookup` agent |
| Code navigation (Serena) | `mcp-serena` agent |
| AL object inspection | `mcp-serena` or direct file tools |

### BC Specialist CLI Commands

Use `bc-expert` CLI directly in Bash for BC specialist access:

```bash
bc-expert ask "<question>"                    # Auto-routes to specialist
bc-expert talk-to <specialist> "<question>"   # Specific specialist
bc-expert who-should-help "<question>"        # Find best specialist
bc-expert specialists --json                  # List all specialists
bc-expert search "<query>"                    # Search knowledge base
bc-expert get "<topic-id>"                    # Get topic details
bc-expert analyze --code workspace            # Analyze AL code
```

Agents return **one-line status** and write details to `.review/` files.

---

## Custom Agents for Large PR Analysis

Agents write all output to `.review/` directory - minimal chat output, full details in files.

### Output Files

```
.review/
├── 01-scan.md              # PR overview and chunks
├── 02-chunk-1.md           # Chunk analysis
├── 02-chunk-2.md           # Chunk analysis
├── 02-chunk-N.md           # ...
├── 03-standards.md         # Naming violations
├── 04-compiler.md          # AL diagnostics
├── 05-dependencies.md      # app.json analysis
├── 99-summary.md           # Final aggregated report
├── expert-[topic].md       # BC expert consultations
├── docs-[topic].md         # MS docs lookups
└── code-[topic].md         # Serena code analysis
```

### Quick Start

```
/review-large-pr main..HEAD
```

### Available Agents

| Agent | Output File | Purpose |
|-------|-------------|---------|
| `pr-scanner` | `01-scan.md` | PR overview, suggest chunks |
| `pr-chunk-analyzer` | `02-chunk-N.md` | Analyze file subset |
| `al-standards-checker` | `03-standards.md` | Naming violations |
| `al-compiler` | `04-compiler.md` | Compile diagnostics |
| `dependency-analyzer` | `05-dependencies.md` | app.json architecture |
| `review-aggregator` | `99-summary.md` | Combine all results |
| `bc-expert` | `expert-[topic].md` | BC specialist consult (CLI) |
| `bc-knowledge` | `knowledge-[topic].md` | BC knowledge base lookup |
| `bc-code-reviewer` | `[file]-review.md` | BC code review (Roger) |
| `mcp-docs-lookup` | `docs-[topic].md` | MS Learn docs |
| `mcp-serena` | `code-[topic].md` | Code navigation |

### Workflow

1. `pr-scanner` → `.review/01-scan.md` (get chunks)
2. Multiple `pr-chunk-analyzer` in parallel → `.review/02-chunk-*.md`
3. `al-standards-checker` + `al-compiler` + `dependency-analyzer` in parallel
4. `review-aggregator` → `.review/99-summary.md`

All agents return one-line status. Read `.review/` files for details.

---

## BC Code Intelligence (CLI)

This profile uses the `bc-expert` CLI tool for BC specialist access, reducing MCP overhead while providing full access to specialized Business Central domain experts.

### Available Specialists
- **Development:** Dean Debug, Sam Coder, Quinn Quality, Isaac Integration
- **Architecture:** Alex Architect, Uri UX
- **Performance:** Pat Performance
- **Review:** Roger Reviewer
- **Security:** Sam Security
- **Testing:** Terra Test
- **Documentation:** Dana Docs
- **Learning:** Leo Learning

### CLI Usage

```bash
# Auto-route to best specialist
bc-expert ask "How do I optimize table queries?"

# Consult specific specialist
bc-expert talk-to roger-reviewer "Review this code pattern"
bc-expert talk-to dean-debug "Performance issues with FindSet"
bc-expert talk-to pat-performance "Query optimization strategies"

# Find specialist for topic
bc-expert who-should-help "Security audit for permissions"

# Search knowledge base
bc-expert search "event subscriber patterns"
bc-expert get "<topic-id>"

# List specialists
bc-expert specialists --json
```

### When to Use BC Code Intelligence
- **Complex problems:** Route questions to appropriate domain experts
- **Code reviews:** Use `bc-code-reviewer` agent (Roger Reviewer)
- **Security concerns:** `bc-expert talk-to sam-security`
- **Performance issues:** `bc-expert talk-to pat-performance`
- **Best practices:** `bc-expert ask` with auto-routing
- **Knowledge lookup:** `bc-knowledge` agent for knowledge base queries

## AL Language Conventions

### Object Naming
- Use PascalCase for all objects (tables, pages, codeunits, reports)
- Prefix custom objects with company/app abbreviation
- Table names should be singular nouns (e.g., `Customer`, `SalesHeader`)
- Page names should match table names with type suffix when needed (e.g., `CustomerCard`, `CustomerList`)
- Codeunit names should be descriptive of their purpose

### Field Naming
- Use PascalCase for field names
- Boolean fields should start with verbs (e.g., `IsBlocked`, `HasCustomer`)
- Date fields should end with `Date` (e.g., `PostingDate`, `ShipmentDate`)
- Avoid abbreviations unless industry standard

### Code Style
- Always use explicit typing, avoid `variant` when possible
- Prefer local procedures over global when appropriate
- Use meaningful variable names (e.g., `Customer` not `Cust`)
- Add XML documentation comments for public procedures
- Group variables logically (parameters, return values, local vars)

### AL Best Practices
- Use table extensions instead of modifying base tables
- Implement proper error handling with meaningful error messages
- Prefer events (subscribers) over modifying base application code
- Use temporary tables for processing when appropriate
- Always validate user input in pages and APIs
- Follow the single responsibility principle for codeunits

### Testing
- Write test codeunits for critical business logic
- Use test isolation principles (setup, execute, verify, teardown)
- Mock external dependencies
- Test both positive and negative scenarios

### Performance Considerations
- Use SetLoadFields to optimize data loading
- Avoid unnecessary loops, prefer FindSet for iteration
- Use filtering before loading records
- Be cautious with LOCKTABLE in multi-user scenarios
- Consider batch operations for bulk data processing

## Common AL Patterns

### Event Pattern
```al
[EventSubscriber(ObjectType::Table, Database::Customer, 'OnBeforeInsertEvent', '', false, false)]
local procedure OnBeforeInsertCustomer(var Rec: Record Customer)
begin
    // Custom logic here
end;
```

### Error Handling Pattern
```al
if not SomeCondition then
    Error('Clear error message: %1', Value);
```

## Project Structure
- Keep related functionality together in subfolders
- Separate tables, pages, codeunits, reports into dedicated folders
- Use consistent file naming: `ObjectType.ObjectName.al`

## Commands to Remember
When working with AL projects:
- `alc` or `ctrl+shift+b` - Compile current project
- Use AL Language extension features for navigation
- Leverage code snippets (t, p, c, r for table, page, codeunit, report)

---

*Note: This configuration is shared across your AL development projects. Update it when you discover new patterns or best practices, and all your projects will benefit.*
