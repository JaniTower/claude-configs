# Feature: [Feature Name]

Brief description of what this feature does.

## Overview

[1-2 paragraphs explaining the business need and solution approach]

## Functional Requirements

### FR-1: [Requirement Name]
- Detail 1
- Detail 2
- Detail 3

### FR-2: [Requirement Name]
- Detail 1
- Detail 2

### FR-3: [Requirement Name]
- Detail 1
- Detail 2

## Technical Design

### Objects

| Type | Number | Name | Description |
|------|--------|------|-------------|
| Table Extension | 50100 | [Name] | [Purpose] |
| Codeunit | 50100 | [Name] | [Purpose] |
| Page Extension | 50100 | [Name] | [Purpose] |

### Key Procedures

```
[CodeunitName]
├── Procedure1(param: Type): ReturnType
│   └── Description of what it does
├── Procedure2(param: Type)
│   └── Description of what it does
└── Procedure3(): ReturnType
    └── Description of what it does
```

### Event Subscribers (REQUIRED)

List all BC events that trigger your business logic:

| Event | Publisher | Calls | Purpose |
|-------|-----------|-------|---------|
| OnBeforePostSalesDoc | Codeunit "Sales-Post" | CheckCreditLimit() | Validate before posting |
| OnAfterInsertEvent | Table Customer | InitializeDefaults() | Set default values |

**Integration Codeunit:** [Name] (50101) - Wires event subscribers to business logic procedures

> **Note:** Every business logic procedure must have a caller. If a procedure handles BC events, specify which events and create an integration codeunit to wire them up.

### Data Flow

```
[Describe how data flows through the system]

User Action → Page → Codeunit → Table
                         ↓
                   Validation
                         ↓
                   Event Published
```

## Non-Functional Requirements

### Performance
- [Performance requirement 1]
- [Performance requirement 2]

### Security
- [Security requirement 1]

### Extensibility
- [Extensibility requirement 1]

## Edge Cases

| Scenario | Expected Behavior |
|----------|-------------------|
| [Edge case 1] | [How to handle] |
| [Edge case 2] | [How to handle] |

## Out of Scope

- [What this feature does NOT include]
- [Future enhancement ideas]

---

## Usage

1. Copy this template to your AL project: `docs/requirements/[feature-name].md`
2. Fill in the sections relevant to your feature
3. Run `/develop` to implement

**Minimal spec requires:**
- Feature name
- Requirements list
- Object numbers
- Event subscribers (which BC events trigger your logic)
- Integration codeunit (if business logic needs event wiring)
