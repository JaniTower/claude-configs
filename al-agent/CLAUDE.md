# AL Development Profile - Simplified Workflow

**Version:** 1.0.0

## Core Principles

### 1. Spec-Driven Development
**You write the spec, the agent implements it.**

- Write your feature specification in `docs/requirements/`
- Run `/develop` to implement
- Agent handles coding, review, and diagnostics in one pass

### 2. Document-Driven Output
**All work is documented in `.dev/` directory.**

Main conversation stays clean - agents write detailed results to files, return concise status updates.

### 3. Integrated Quality
**No separate review or diagnostics phases.**

The al-developer agent performs self-review and fixes diagnostics as part of implementation.

## Development Workflow

```
docs/requirements/notes.md (rough notes/ideas)
    ↓
/plan (auto-detects file, structures it)
    ↓
docs/requirements/notes-formatted.md (structured spec)
    ↓
/develop (auto-detects *-formatted.md files)
    ↓
al-developer implements + self-reviews + fixes diagnostics
    ↓
.dev/03-development-report.md
    ↓
(optional) /test → test-engineer writes tests
    ↓
(optional) /document → docs-writer generates docs
```

## Directory Structure

```
your-project/
├── docs/
│   └── requirements/
│       └── *.md             # Your feature specification(s) (YOU WRITE THESE)
├── src/                     # AL source files (agent creates)
└── .dev/
    ├── project-context.md   # Project memory (optional, speeds up work)
    ├── 03-development-report.md  # Development summary
    ├── 05-test-plan.md      # Test strategy (from /test)
    ├── 06-test-review.md    # Test review (from /test)
    └── session-log.md       # Activity log
```

## Available Commands

### Main Commands

| Command | Purpose |
|---------|---------|
| `/plan` | Transform rough notes into structured specification |
| `/develop` | Implement code from formatted specification |
| `/test` | Create tests for implemented code |
| `/document` | Generate documentation |

### Usage

```
/plan                 # Auto-detect .md file, output *-formatted.md
/develop                     # Auto-detect *-formatted.md file, implement code
/test                        # Create tests after development
/document                    # Generate user documentation
```

**Auto-detection rules:**
- `/plan` - Finds `.md` files (excludes `*-formatted.md`)
- `/develop` - Finds `*-formatted.md` files only
- Both ask which file if multiple found

## Writing Specifications

Create your spec file in `docs/requirements/` before running `/develop`.

### Minimal Spec

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
- Codeunit: 50100 (Credit Limit Mgt.)
- Codeunit: 50101 (Credit Limit Integration)
- Page Extension: 50100

## Event Subscribers

| Event | Publisher | Calls |
|-------|-----------|-------|
| OnBeforePostSalesDoc | Codeunit "Sales-Post" | CheckCreditLimit() |
```

### Detailed Spec

```markdown
# Feature: Customer Credit Limit Validation

## Overview

Implement credit limit tracking and validation for customers.

## Functional Requirements

### FR-1: Credit Limit Storage
- Add CreditLimit (Decimal) field to Customer table
- DataClassification: CustomerContent

### FR-2: Validation
- Credit limit cannot be negative
- Error on sales posting if exceeds limit

### FR-3: User Interface
- Show Credit Limit on Customer Card
- Add Outstanding Amount (calculated)

## Technical Design

| Type | Number | Name |
|------|--------|------|
| Table Extension | 50100 | Customer Credit Limit |
| Codeunit | 50100 | Credit Limit Mgt. |
| Codeunit | 50101 | Credit Limit Integration |
| Page Extension | 50100 | Customer Card Ext |

## Key Procedures
- CheckCreditLimit(CustomerNo: Code[20])
- GetOutstandingAmount(CustomerNo: Code[20]): Decimal

## Event Subscribers

| Event | Publisher | Calls | Purpose |
|-------|-----------|-------|---------|
| OnBeforePostSalesDoc | Codeunit "Sales-Post" | CheckCreditLimit() | Block posting if over limit |

**Integration Codeunit:** Credit Limit Integration (50101)
```

## Agents

### solution-planner
**Spec structuring agent** - Transforms rough notes into well-structured specs.

- Reads unstructured notes or incomplete specs
- Applies standard template structure
- Infers event subscribers and integration codeunits
- Outputs to `docs/requirements/`

### al-developer
**Main development agent** - Implements AL code from specification.

- Reads spec from `docs/requirements/`
- Writes AL code to `src/`
- Performs self-review (standards, DRY/SOLID, performance)
- Auto-fixes safe diagnostics (spacing, parentheses)
- Outputs `.dev/03-development-report.md`

### test-engineer
**Test creation agent** - Creates comprehensive tests.

- Reads implemented code and spec
- Writes test codeunits
- Outputs `.dev/05-test-plan.md`

### test-reviewer
**Test review agent** - Reviews test quality and coverage.

- Reads test code
- Checks coverage against spec
- Outputs `.dev/06-test-review.md`

### docs-writer
**Documentation agent** - Generates user documentation.

- Reads code and spec
- Writes user guides, READMEs
- Outputs to `docs/` directory

## AL Compilation

**ALWAYS use `al-compile` wrapper.**

```bash
al-compile                    # Default: compile with all analyzers
al-compile --verbose          # Show detailed output
al-compile --analyzers all    # Include ALL analyzers
```

The wrapper auto-detects:
- VS Code AL extension and compiler version
- Workspace structure (single vs multi-app)
- All `.alpackages` directories
- Ruleset files
- AppSourceCop (if config exists)

## AL Coding Rules

**The plugin includes `rules/*.json` with AL coding rules.**

The al-developer agent reads these rules and applies them based on action level:
- `"Error"` = Must follow - code violating these is rejected
- `"Warning"` = Should follow - violations are noted
- `"None"` = Ignored - rule is disabled

**To customize:** Edit `rules/main.ruleset.json` or add additional `.json` files to `rules/`.

## AL Coding Standards

### Object Naming
- **PascalCase** for all objects
- **Prefix** custom objects with company/app abbreviation
- **Table names**: Singular nouns (`Customer`, `SalesHeader`)
- **Page names**: Match table + type (`CustomerCard`, `CustomerList`)

### Field Naming
- **PascalCase** for field names
- **Boolean fields**: Start with verbs (`IsBlocked`, `HasCustomer`)
- **Date fields**: End with `Date` (`PostingDate`, `ShipmentDate`)

### Code Style
- Explicit typing (avoid `variant`)
- Local procedures over global when appropriate
- Meaningful names (`Customer` not `Cust`)
- XML documentation for public procedures

### Required Properties
- **DataClassification** on all table fields
- **ApplicationArea** on all page controls
- **ToolTip** on all user-facing fields

### Best Practices
- Table extensions instead of modifying base tables
- Event subscribers over modifying base code
- Error handling with meaningful messages
- SetLoadFields for performance
- FindSet for iteration (not `Find('-')`)

## Standard Patterns

### Event Subscriber
```al
[EventSubscriber(ObjectType::Table, Database::Customer, 'OnBeforeInsertEvent', '', false, false)]
local procedure OnBeforeInsertCustomer(var Rec: Record Customer)
begin
    // Custom logic here
end;
```

### Error Handling
```al
if not SomeCondition then
    Error('Clear error message: %1', Value);
```

### Performance
```al
Customer.SetLoadFields("No.", "Name", CreditLimit);
if Customer.Get(CustomerNo) then
    // Only loaded specified fields
```

## Typical Workflow

### 1. Write Specification
```bash
# Create your spec file in docs/requirements/
```

### 2. Implement
```bash
/develop
# Agent reads spec, implements code, reviews, fixes diagnostics
# Check .dev/03-development-report.md for summary
```

### 3. Test (Optional)
```bash
/test
# Agent creates test codeunits
# Check .dev/05-test-plan.md and .dev/06-test-review.md
```

### 4. Document (Optional)
```bash
/document
# Agent generates user documentation
```

## Success Criteria

A complete development produces:
- Working AL code in `src/`
- Development report with review findings
- Clean compilation (no errors)
- (Optional) Comprehensive tests
- (Optional) User documentation

**All documented in `.dev/` for future reference.**

---

*Simplified AL development: write spec, run /develop, done.*
