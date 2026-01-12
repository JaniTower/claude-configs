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
3. **Run AL Compilation**: Execute `AL compile` to gather all diagnostics
4. **Verify Code Cop Configuration**: Ensure all required code cops are enabled
5. **Check AppSourceCop Configuration**: Verify `AppSourceCop.json` exists and is properly configured
6. **Analyze Dependencies**: Review `app.json` for external dependencies per best practices
7. Engage Roger Reviewer (BC Code Intelligence specialist) using `get_specialist_advice`
8. Provide Roger with: file contents, diagnostics, purpose, and any custom instructions
9. Conduct comprehensive review per specifications below

## Execution Steps

1. **Run AL Compilation First**:
   - Execute `AL compile` in the project directory to capture all diagnostics
   - This will catch warnings, errors, and code cop violations
   - Document all compilation output for inclusion in the review

### AL Command Line Compiler Usage

#### Compilation Commands
```bash
# Basic compilation with package cache
AL compile /project:"." /packagecachepath:".alpackages"

# Check AL compiler version
AL --version

# Get help on available commands
AL --help
AL help compile
```

#### Common AL Compiler Options
- `/project:"."` - Specifies the project folder (current directory)
- `/packagecachepath:".alpackages"` - Specifies where dependency packages are stored
- AL tools are installed globally via dotnet tool

2. **Verify Project Configuration**:
   - Check that all required code cops are enabled in AL project settings:
     - **CodeCop**: Enforces AL coding conventions
     - **UICop**: Enforces UI and user experience best practices
     - **PerTenantExtensionCop**: Validates per-tenant extension requirements
     - **AppSourceCop**: Validates AppSource submission requirements
   - Verify `AppSourceCop.json` file exists in the project root
   - If `AppSourceCop.json` is missing or misconfigured, note this as a critical issue

3. **Analyze Dependencies (app.json)**:
   - Read and review the `app.json` file
   - Identify all external dependencies in the `dependencies` section
   - **Best Practice Check**: Per AL best practices, external dependencies should be handled by individual "connector apps"
   - Flag if the main extension has too many external dependencies (violates single responsibility principle at the app level)
   - Suggest refactoring to use dedicated connector apps for external integrations

4. **Read the File(s)**: Use the Read tool to load the AL code file(s) specified by the user

5. **Analyze Context**: Determine the purpose and type of code (page, codeunit, table, etc.)

6. **Engage Roger Reviewer**: Use `get_specialist_advice` with specialist_id `roger-reviewer`

7. **Provide Complete Context**: Give Roger:
   - File contents to review
   - **AL Compilation diagnostics** from `AL compile`
   - Code cop configuration status
   - AppSourceCop.json configuration analysis
   - Dependency analysis from app.json
   - Purpose and any custom instructions

8. **Follow Roger's Process**: Let Roger conduct the multi-layered analysis detailed below

## Review Scope

You will conduct a multi-layered code review analyzing:

### 0. Compilation Diagnostics & Project Configuration
- **AL Compilation Results**
  - Review all errors, warnings, and information messages from `AL compile`
  - Categorize diagnostics by code cop (CodeCop, UICop, PerTenantExtensionCop, AppSourceCop)
  - Flag any compilation errors as critical blockers
  - Assess warning severity and impact on code quality

- **Code Cop Configuration**
  - Verify all 5 required code cops are enabled:
    - CodeCop (coding conventions)
    - UICop (UI/UX best practices)
    - PerTenantExtensionCop (PTE requirements)
    - AppSourceCop (AppSource requirements)
  - Check if ruleset files are properly configured
  - Ensure no cops are disabled without justification

- **AppSourceCop Configuration**
  - Verify `AppSourceCop.json` exists in project root
  - Validate required fields:
    - `name`: Application name
    - `publisher`: Publisher name
    - `version`: Baseline version for breaking change detection
  - Check for mandatory rules compliance
  - Flag if file is missing or incomplete

- **Dependency Analysis (app.json)**
  - Review external dependencies in `app.json`
  - **Single Purpose Principle**: Extensions should have focused purpose
  - **Best Practice**: External integrations should use dedicated "connector apps"
  - Flag violations:
    - Too many external dependencies in a single app
    - Direct dependencies that should be abstracted through connector apps
    - Missing dependency isolation for external systems
  - Recommend refactoring to connector app pattern when appropriate

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

**Step 1: Run AL Compilation**
Execute the compilation to gather diagnostics:
```bash
AL compile
```
- Capture all output (errors, warnings, info)
- Document compilation status
- Note all code cop violations

**Step 2: Verify Project Configuration**
Check project configuration files:
- Review `.alpackages` and project settings for enabled code cops
- Verify `AppSourceCop.json` exists and is valid
- Read `app.json` to analyze dependencies

**Step 3: Engage Roger Reviewer**
Use the BC Code Intelligence MCP to engage Roger Reviewer specialist:
```
Call: get_specialist_advice
Specialist: roger-reviewer
```

**Step 4: Provide Complete Context**
Give Roger the following information:
- The code file(s) to review (just the paths not the content!)
- **AL compilation diagnostics and results**
- **Code cop configuration status**
- **AppSourceCop.json validation results**
- **Dependency analysis from app.json**
- Purpose/intent of the code
- Whether it's production code or demo/sample code
- Any specific concerns or areas of focus

**Step 5: Request Comprehensive Analysis**
Ask Roger to analyze through these lenses:
1. **Compilation diagnostics** (errors, warnings from all code cops)
2. **Project configuration** (code cop setup, AppSourceCop.json, dependencies)
3. Software engineering principles (SOLID, DRY, SoC)
4. AL/BC best practices and patterns
5. Testability and maintainability
6. Performance and security considerations
7. Code organization and structure
8. **Dependency architecture** (connector app pattern adherence)

**Step 6: Prioritized Recommendations**
Roger should provide:
- **Compilation Blockers**: Errors that prevent compilation
- **Critical Issues**: Must fix before deployment
- **High Priority**: Significant quality/maintainability impact
- **Medium Priority**: Improvements that reduce technical debt
- **Low Priority**: Minor refinements and optimizations

**Step 7: Refactoring Examples**
For each significant issue, request:
- Explanation of the problem
- Why it violates principles/best practices
- Concrete code example showing the fix
- Benefits of the refactoring

**Step 8: Write Review Report**
Save the complete review to a markdown file in `.review/` directory

## Output Format

The review should include:

### Executive Summary
- Overall code quality rating (A-F)
- Compilation status (Errors/Warnings count)
- Code cop configuration status
- AppSourceCop configuration status
- Dependency architecture assessment
- Top 3 strengths
- Top 3 improvement areas
- Recommended next actions

### Compilation & Configuration Report

#### 🔨 AL Compilation Results
- **Compilation Status**: Pass/Fail
- **Total Diagnostics**: X errors, Y warnings, Z info
- **By Code Cop**:
  - CodeCop: [issues]
  - UICop: [issues]
  - PerTenantExtensionCop: [issues]
  - AppSourceCop: [issues]
- **Critical Issues**: [List all errors]
- **Warnings Requiring Attention**: [High-priority warnings]

#### ⚙️ Project Configuration
- **Code Cops Status**:
  - CodeCop: ✅ Enabled / ❌ Disabled
  - UICop: ✅ Enabled / ❌ Disabled
  - PerTenantExtensionCop: ✅ Enabled / ❌ Disabled
  - AppSourceCop: ✅ Enabled / ❌ Disabled
- **AppSourceCop.json**: ✅ Present and valid / ⚠️ Issues found / ❌ Missing
- **Ruleset Configuration**: [Status and notes]

#### 📦 Dependency Architecture
- **Dependencies Count**: X external dependencies
- **Architecture Pattern**: ✅ Follows connector app pattern / ⚠️ Some concerns / ❌ Violates best practices
- **Issues Identified**:
  - [List dependencies that should be in connector apps]
  - [Note if app has too many responsibilities]
- **Recommendations**:
  - [Specific refactoring suggestions for dependency isolation]

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

## Output Deliverable

**IMPORTANT**: After completing the review, write the complete review report to a markdown file:

- **Filename**: `.review/[filename]-review-[YYYY-MM-DD].md`
  - Example: `.review/MyCodeunit.Codeunit-review-2025-11-07.md`
- Create the `.review` directory if it doesn't exist
- Use the Write tool to save the complete review report
- Include all sections: Executive Summary, Compilation Report, Configuration Status, Dependency Analysis, and Detailed Findings
- Format using proper markdown with headers, code blocks, and lists

This ensures:
- Reviews are tracked and versioned
- Progress can be monitored over time
- Team members can reference past reviews
- Review history is preserved in the repository

## After the Review

The code should be refactored before:
- Writing unit tests (use Quinn Tester specialist)
- Deploying to production
- Code review by team members
- Integration with other modules

**Review Tracking:**
- Compare new reviews with previous reviews in `.review` directory
- Track improvement over time
- Ensure all critical issues from previous reviews are addressed

---

**Note:** This command leverages Roger Reviewer from the BC Code Intelligence MCP. If Roger identifies issues requiring other specialists (e.g., performance issues → Dean Debug, architecture concerns → Alex Architect), he will recommend appropriate handoffs.
