---
description: Comprehensive AL code review enforcing SOLID, DRY, and BC best practices
argument-hint: "custom-instructions which should include a file (or multiple)"
allowed-tools: ["*"]
---

# Comprehensive AL Code Review

**User provided arguments:** `$ARGUMENTS`

**Your task:**
1. Parse `$ARGUMENTS` to extract the file path (first part) and custom instructions (remaining)
2. Read the specified file using the Read tool
3. Engage Roger Reviewer (BC Code Intelligence specialist) using `get_specialist_advice`
4. Provide Roger with: file contents, purpose, and any custom instructions
5. Conduct comprehensive review per specifications below

## Execution Steps

1. **Read the File(s)**: Use the Read tool to load the AL code file(s) specified by the user
2. **Analyze Context**: Determine the purpose and type of code (page, codeunit, table, etc.)
3. **Engage Roger Reviewer**: Use `get_specialist_advice` with specialist_id `roger-reviewer`
4. **Provide Complete Context**: Give Roger the file contents, purpose, and request comprehensive review
5. **Follow Roger's Process**: Let Roger conduct the multi-layered analysis detailed below

## Review Scope

You will conduct a multi-layered code review analyzing:

### 1. Software Engineering Principles
- **SOLID Principles**
  - Single Responsibility: Each procedure/codeunit has one clear purpose
  - Open/Closed: Code extensible without modification
  - Liskov Substitution: Derived types properly substitutable
  - Interface Segregation: Focused, cohesive interfaces
  - Dependency Inversion: Depend on abstractions, not concretions

- **DRY (Don't Repeat Yourself)**
  - Identify duplicated code patterns
  - Suggest extraction of reusable procedures
  - Flag repeated string literals, magic numbers

- **Separation of Concerns**
  - UI logic properly separated from business logic
  - Business logic in codeunits, not pages
  - Clear layering: Presentation → Business → Data

- **Clean Code Principles**
  - Meaningful naming (variables, procedures, parameters)
  - Small, focused procedures (ideally < 50 lines)
  - Clear intent, self-documenting code
  - Appropriate abstraction levels

### 2. AL/BC Best Practices
- Event patterns vs. direct modification
- Table extensions vs. base table changes
- Proper use of temporary tables
- SetLoadFields optimization
- Error handling patterns
- Transaction management
- Permission considerations

### 3. Code Quality Indicators
- Testability (can this code be unit tested?)
- Maintainability (will future developers understand this?)
- Performance considerations
- Security implications
- Error handling completeness

### 4. AL Coding Standards
- Naming conventions (PascalCase, prefixes)
- Code formatting and spacing
- Variable declaration order
- Comment quality and necessity
- File organization

## Review Process

**Step 1: Engage Roger Reviewer**
Use the BC Code Intelligence MCP to engage Roger Reviewer specialist:
```
Call: get_specialist_advice
Specialist: roger-reviewer
```

**Step 2: Provide Context**
Give Roger the following information:
- The code file(s) to review (just the paths not the content!)
- Purpose/intent of the code
- Whether it's production code or demo/sample code
- Any specific concerns or areas of focus

**Step 3: Request Comprehensive Analysis**
Ask Roger to analyze through these lenses:
1. Software engineering principles (SOLID, DRY, SoC)
2. AL/BC best practices and patterns
3. Testability and maintainability
4. Performance and security considerations
5. Code organization and structure

**Step 4: Prioritized Recommendations**
Roger should provide:
- **Critical Issues**: Must fix before deployment
- **High Priority**: Significant quality/maintainability impact
- **Medium Priority**: Improvements that reduce technical debt
- **Low Priority**: Minor refinements and optimizations

**Step 5: Refactoring Examples**
For each significant issue, request:
- Explanation of the problem
- Why it violates principles/best practices
- Concrete code example showing the fix
- Benefits of the refactoring

## Output Format

The review should include:

### Executive Summary
- Overall code quality rating (A-F)
- Top 3 strengths
- Top 3 improvement areas
- Recommended next actions

### Detailed Findings

#### ✅ Strengths
List what the code does well

#### ⚠️ Principle Violations

**SOLID Violations:**
- [Specific examples with line numbers]

**DRY Violations:**
- [Duplicated code with locations]

**Separation of Concerns Issues:**
- [Mixed responsibilities]

#### 🔧 Improvement Recommendations

For each issue:
```
**Issue:** [Description]
**Location:** [File:LineNumber]
**Impact:** High/Medium/Low
**Principle:** [Which principle is violated]

**Current Code:**
[Code snippet showing the issue]

**Recommended Fix:**
[Code snippet showing the solution]

**Why This Matters:**
[Explanation of benefits]
```

#### 📊 Metrics
- Testability score: [Rating + explanation]
- Maintainability score: [Rating + explanation]
- Complexity assessment: [Simple/Moderate/Complex]

### Follow-up Actions
1. [Prioritized list of refactoring tasks]
2. [Testing recommendations]
3. [Documentation needs]

## Usage Examples

### Review a specific file:
```
/review-al-code src/MyCodeunit.Codeunit.al
```

### Review with focus area:
```
/review-al-code src/MyPage.Page.al --focus="testability and separation of concerns"
```

### Review multiple files:
```
/review-al-code src/Sales/*.al
```

## Quality Standards

This review should help achieve:
- ✅ Code that can be easily unit tested
- ✅ Code that follows SOLID principles
- ✅ Maintainable code with clear separation of concerns
- ✅ BC-optimized code following platform best practices
- ✅ Production-ready code with proper error handling
- ✅ Code that minimizes technical debt

## After the Review

The code should be refactored before:
- Writing unit tests (use Quinn Tester specialist)
- Deploying to production
- Code review by team members
- Integration with other modules

---

**Note:** This command leverages Roger Reviewer from the BC Code Intelligence MCP. If Roger identifies issues requiring other specialists (e.g., performance issues → Dean Debug, architecture concerns → Alex Architect), he will recommend appropriate handoffs.
