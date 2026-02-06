---
description: Transform rough feature descriptions into well-structured specifications following the standard template.
---

# Solution Planner

Read an unstructured or incomplete specification file and transform it into a well-structured specification following the standard template format.

## Your Mission

Take rough feature descriptions, notes, or incomplete specs and produce a complete, well-structured specification that the al-developer agent can implement.

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| Source filename | **Auto-detected** | Auto-detects from `docs/requirements/` folder |
| `docs-templates/spec-template.md` (plugin) | **Yes** | Template structure to follow |

**Auto-detection rules:**
- If 1 `.md` file in `docs/requirements/` → use it automatically
- If 0 files → error: "No .md files found in docs/requirements/"
- If 2+ files → ask user which one to format

## Outputs

| Output | Description |
|--------|-------------|
| Structured spec file | New file in `docs/requirements/` with `-formatted` suffix |
| Summary | Brief report of what was added/inferred |

**Example:** `notes.md` → `notes-formatted.md` (both in `docs/requirements/`)

## Workflow

### Phase 0: Find Source File

1. **Glob for files** - Search `docs/requirements/*.md`
2. **Apply detection rules:**
   - If 0 files found → Error: "No .md files found in docs/requirements/. Create a rough spec file first."
   - If 1 file found → Use it automatically, report: "Found: docs/requirements/[filename]"
   - If 2+ files found → List files and ask: "Multiple .md files found. Which one should I format?"
     - Wait for user response before proceeding
3. **Exclude already-formatted files** - Skip files ending in `-formatted.md`

### Phase 1: Analyze Source

4. **Read the source file** - Load from `docs/requirements/[filename]`
5. **Read the template** - Load `docs-templates/spec-template.md` from plugin directory
6. **Extract information** - Identify:
   - Feature name and description
   - Functional requirements (explicit and implied)
   - Technical details (objects, procedures, fields)
   - Event triggers and integration points
   - Edge cases mentioned
   - Any object numbers specified

### Phase 2: Identify Gaps

7. **Check for missing REQUIRED sections:**
   - [ ] Feature name and overview
   - [ ] Functional requirements
   - [ ] Object numbers (Table Ext, Codeunits, Page Ext)
   - [ ] Key procedures with signatures
   - [ ] **Event Subscribers** (CRITICAL - what BC events trigger the logic?)
   - [ ] **Integration Codeunit** (CRITICAL - who wires subscribers to business logic?)

8. **Flag items needing user input:**
   - Missing object numbers → Suggest ranges, ask user
   - Unclear requirements → List ambiguities
   - Missing event triggers → Infer from requirements or ask

### Phase 3: Structure Output

9. **Build structured spec** following template sections:

```markdown
# Feature: [Name]

[Brief description]

## Overview

[1-2 paragraphs - expand from source or summarize]

## Functional Requirements

### FR-1: [Requirement Name]
- [Details extracted/inferred from source]

### FR-2: [Requirement Name]
- [Details]

## Technical Design

### Objects

| Type | Number | Name | Description |
|------|--------|------|-------------|
| [From source or suggested] |

### Key Procedures

```
[CodeunitName]
├── Procedure1(param: Type): ReturnType
│   └── [Description]
```

### Event Subscribers (REQUIRED)

| Event | Publisher | Calls | Purpose |
|-------|-----------|-------|---------|
| [Inferred from requirements] |

**Integration Codeunit:** [Name] ([Number]) - [Purpose]

### Data Flow

```
[Diagram showing how data flows]
```

## Edge Cases

| Scenario | Expected Behavior |
|----------|-------------------|
| [From source or common cases] |

## Out of Scope

- [Items explicitly excluded or inferred]
```

### Phase 4: Write Output

10. **Determine output filename:**
    - Use source filename with `-formatted` suffix
    - Place in `docs/requirements/`
    - Example: `notes.md` → `notes-formatted.md`
    - Example: `credit-limit.md` → `credit-limit-formatted.md`

11. **Write the structured spec file** to `docs/requirements/[name]-formatted.md`

12. **Report to user:**
   - Source file read
   - Output file created
   - What was extracted from source
   - What was inferred/suggested
   - What needs user confirmation (especially object numbers)

## Inference Rules

### Inferring Event Subscribers

From requirements, infer which BC events are needed:

| Requirement Pattern | Likely Event |
|---------------------|--------------|
| "Block posting if..." | OnBeforePost* events |
| "Validate when..." | OnValidate, OnBeforeValidateEvent |
| "When record is created..." | OnAfterInsertEvent, OnBeforeInsertEvent |
| "When record is modified..." | OnAfterModifyEvent, OnBeforeModifyEvent |
| "When record is deleted..." | OnBeforeDeleteEvent, OnAfterDeleteEvent |
| "On sales order..." | Sales Header/Line events |
| "On purchase order..." | Purchase Header/Line events |
| "When posting..." | Codeunit "Sales-Post", "Purch.-Post" events |

### Inferring Object Numbers

If not specified, suggest:
- Start from 50100 (standard custom range)
- Table Extension: 50100
- Main Codeunit: 50100
- Integration Codeunit: 50101
- Page Extension: 50100
- Enum: 50100

**Always ask user to confirm object numbers.**

### Inferring Procedures

From requirements, identify needed procedures:
- "Calculate X" → `CalculateX(): Decimal`
- "Validate X" → `ValidateX(Value: Type): Boolean`
- "Check if X" → `IsX(): Boolean`
- "Get X" → `GetX(): Type`
- "Set X" → `SetX(Value: Type)`

## Output Format

### Chat Response

```
Structured spec created:
  Source: docs/requirements/[filename].md
  Output: docs/requirements/[filename]-formatted.md

## Extracted from source:
- [List of requirements found]
- [Object numbers if specified]

## Inferred/Added:
- Event Subscribers: [List with reasoning]
- Integration Codeunit: [Name] (50101)
- Edge cases: [List]

## Needs confirmation:
- Object numbers: [Suggested range]
- [Any ambiguous requirements]

Review the spec and confirm object numbers before running /develop.
```

### If Critical Information Missing

```
Cannot create complete spec - missing critical information:

## Found:
- [What was extracted]

## Missing (REQUIRED):
- [ ] [Missing item 1 - explain why needed]
- [ ] [Missing item 2]

Please provide:
1. [Specific question about missing info]
2. [Another question if needed]
```

## Example Transformation

### Input (rough notes):
```
need to add discount validation for customers
- vip customers get 10%
- regular get 5%
- don't let them enter more than allowed
- show on customer card
```

### Output (structured spec):
```markdown
# Feature: Customer Discount Validation

Validate and enforce maximum discount percentages based on customer type.

## Overview

Implement a discount validation system that enforces maximum discount limits based on customer classification. VIP customers can receive up to 10% discount, while regular customers are limited to 5%.

## Functional Requirements

### FR-1: Customer Type Classification
- Add Customer Type field to Customer table
- Values: Regular (default), VIP
- Display on Customer Card

### FR-2: Discount Limits
- Regular customers: Maximum 5% discount
- VIP customers: Maximum 10% discount

### FR-3: Discount Validation
- Validate discount percentage on sales lines
- Prevent saving if discount exceeds customer's maximum
- Show clear error message with allowed maximum

## Technical Design

### Objects

| Type | Number | Name | Description |
|------|--------|------|-------------|
| Enum | 50100 | Customer Type | Regular, VIP values |
| Table Extension | 50100 | Customer Discount Ext | Adds Customer Type field |
| Codeunit | 50100 | Discount Validation Mgt. | Validation logic |
| Codeunit | 50101 | Discount Validation Integration | Event subscribers |
| Page Extension | 50100 | Customer Card Ext | Shows Customer Type |

### Key Procedures

```
Discount Validation Mgt. (50100)
├── GetMaxDiscountPct(CustomerNo: Code[20]): Decimal
│   └── Returns maximum allowed discount for customer type
├── ValidateDiscountPct(CustomerNo: Code[20]; DiscountPct: Decimal): Boolean
│   └── Returns true if discount is within allowed limit
└── GetCustomerType(CustomerNo: Code[20]): Enum "Customer Type"
    └── Returns customer's type classification
```

### Event Subscribers (REQUIRED)

| Event | Publisher | Calls | Purpose |
|-------|-----------|-------|---------|
| OnBeforeValidateEvent (Line Discount %) | Table "Sales Line" | ValidateDiscountPct() | Block invalid discounts |

**Integration Codeunit:** Discount Validation Integration (50101) - Subscribes to Sales Line validation and calls Discount Validation Mgt.

## Edge Cases

| Scenario | Expected Behavior |
|----------|-------------------|
| Customer Type is blank | Default to Regular (5% max) |
| Discount = 0 | Always valid |
| Negative discount entered | Error: must be >= 0 |

## Out of Scope

- Automatic discount calculation (manual entry only)
- Date-based promotional discounts
- Quantity-based discounts
```

## Key Principles

1. **Never lose information** - Everything from source must appear in output
2. **Infer sensibly** - Use patterns to suggest likely needs
3. **Flag uncertainty** - Clearly mark inferred vs. explicit items
4. **Event subscribers are REQUIRED** - Always determine what triggers the logic
5. **Integration codeunit is REQUIRED** - Business logic needs callers
6. **Ask don't assume** - For object numbers and ambiguous requirements

---

**Remember:** A well-structured spec prevents implementation confusion. Take time to get it right before the al-developer starts coding.
