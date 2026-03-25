# Platonic Code Review - Reference Guide

Complete guide for validating code implementation against specifications.

## Overview

**Purpose**: Validate code correctly implements specifications (NOT general code quality review)

**Default**: Generate reports WITHOUT modifying code (unless explicitly requested)

**Focus**: Spec-to-code consistency, completeness, correctness, gap identification

## The 6-Step Review Process

### Step 1: Understand Specifications

**Actions**:
1. Locate specification documents (RFCs, requirements, design docs, API specs)
2. Read and extract key requirements:
   - Features and functionality
   - Behaviors and workflows
   - Interfaces (APIs, functions, endpoints)
   - Data structures and schemas
   - Business rules and constraints
   - Error handling requirements
3. Note spec versions, dates, and dependencies

**Output**: List of testable requirements with spec references

---

### Step 2: Generate Functionality Checklist

**Format**:
```markdown
### [ID]: [Feature Name]
- **Spec**: [Document, Section]
- **Priority**: [Critical/High/Medium/Low]
- **Description**: [Clear statement]
- **Testable Criteria**:
  - [ ] Criterion 1
  - [ ] Criterion 2
- **Status**: [ ] To Review
```

**Tips**:
- Be comprehensive - include all requirements
- Make items testable and specific
- Include edge cases, error handling, validations
- Group by category/module

---

### Step 3: Map Specs to Code

**Process**:
1. Understand codebase structure
2. For each checklist item, find:
   - Primary implementation files
   - Supporting code (helpers, utilities)
   - Configuration
   - Tests
3. Document mapping:
   ```markdown
   ### [ID]: [Feature]
   - **Implementation**: src/path/file.ts:45-123
   - **Tests**: tests/path/test.ts
   - **Status**: Found / NOT FOUND
   ```

**Search Strategies**:
- Grep for spec keywords and terminology
- Look for API endpoints/routes
- Check function/class names
- Review imports and dependencies
- Examine database models

---

### Step 4: Review Implementation

**For Each Item**:
1. Read relevant code
2. Compare with spec requirements
3. Assign status:
   - ✅ **Fully Implemented** - Matches spec completely
   - ⚠️ **Partially Implemented** - Some aspects missing
   - ❌ **Not Implemented** - No implementation found
   - 🔍 **Unclear** - Found code but unclear if correct
   - ⚡ **Inconsistent** - Contradicts spec

4. Document findings with code references and evidence

**Example**:
```markdown
### AUTH-001: Email Login
- **Status**: ✅ Fully Implemented
- **Code**: src/auth/LoginController.ts:45-123
- **Evidence**:
  - Line 67: JWT generation (spec 4.3)
  - Line 89: Rate limit check (spec 5.2)
  - Line 102: Password validation (spec 3.1.4)
```

---

### Step 5: Identify Discrepancies

**Discrepancy Types**:

1. **Missing**: Spec describes, code doesn't implement
2. **Inconsistent**: Code differs from spec requirements
3. **Partial**: Some aspects implemented, others missing
4. **Extra**: Code implements features not in specs
5. **Incorrect**: Logic doesn't match spec behavior

**Documentation Format**:
```markdown
### [TYPE]: [Issue Title]
**Spec**: [Reference]
**Code**: [Location or "Not found"]
**Issue**: [Clear description]
**Impact**: [Critical/High/Medium/Low]
**Evidence**: [Code snippets or search results]
**Recommendation**: [Specific action]
```

---

### Step 6: Generate Report

**Report Structure**:

```markdown
# Spec-to-Code Review Report

## Summary
- Specs Reviewed: X
- Code Modules: Y
- Consistency Rate: Z%
- Total Items: N (✅A, ⚠️B, ❌C, 🔍D, ⚡E)

## Critical Issues
[Detailed findings with code references]

## High Priority Issues
[Detailed findings]

## Medium/Low Priority Items
[Brief list]

## Functionality Checklist Status
| Category | Total | Complete | Partial | Missing | Unclear | Inconsistent |
|----------|-------|----------|---------|---------|---------|--------------|
| [Cat]    | N     | A        | B       | C       | D       | E            |

## Category Analysis
### [Category Name]
- ✅ Fully Implemented (X items): [list]
- ⚠️ Partial (Y items): [list with issues]
- ❌ Missing (Z items): [list with impact]

## Recommendations
**Priority 1 (Critical)**:
1. [Action with effort and impact]

**Priority 2 (High)**:
1. [Action]

**Priority 3 (Medium/Low)**:
1. [Action]

## Next Steps
**Note**: No code modified by this review.

To implement fixes: [request explicitly]
To update specs: [request explicitly]
```

---

## Review Levels

### Level 1: Basic Compliance (5-10 min)
- Major features only (3-5 key items)
- Quick presence/absence check
- Brief summary report

### Level 2: Detailed Verification (30-60 min)
- All specified features
- Implementation correctness check
- Detailed status for each item
- Comprehensive report

### Level 3: Comprehensive Audit (2+ hours)
- Deep implementation analysis
- Algorithm correctness
- Performance validation
- Security review
- Test coverage mapping
- Bi-directional analysis (spec→code, code→spec)

---

## Common Patterns

### Pattern 1: Missing Feature
```
Spec: Feature X required
Code: No implementation found
Status: ❌ Missing
Impact: [Assess based on feature criticality]
```

### Pattern 2: Inconsistent Implementation
```
Spec: Lock after 5 consecutive failures
Code: Locks after 5 failures in 24h (different logic)
Status: ⚡ Inconsistent
Impact: Incorrect behavior
```

### Pattern 3: Partial Implementation
```
Spec: OAuth with Google, GitHub, Facebook
Code: Only Google and GitHub
Status: ⚠️ Partial (Facebook missing)
Impact: Incomplete feature
```

### Pattern 4: Extra Implementation
```
Spec: No mention of feature
Code: Fully implemented feature
Status: 🔍 Undocumented
Action: Create spec or remove code
```

---

## Example: Simple Review

**Spec** (RFC-005, Section 2.1):
```
GET /api/users/:id returns user name, email, registrationDate
```

**Review**:
1. **Find Code**: `src/api/users.controller.ts:45-52`
2. **Check Implementation**:
   ```typescript
   return res.json({
     name: user.name,
     email: user.email,
     // registrationDate MISSING
   });
   ```
3. **Status**: ⚠️ Partial - Missing registrationDate
4. **Report**:
   ```markdown
   ### Finding: Missing Registration Date
   **Severity**: Medium
   **Spec**: RFC-005, Section 2.1
   **Code**: src/api/users.controller.ts:49
   **Issue**: Response missing required `registrationDate` field
   **Recommendation**: Add `registrationDate: user.createdAt` to response
   ```

---

## Bi-Directional Analysis

**Spec → Code** (Unimplemented):
- List features in specs not found in code
- Impact assessment
- Priority recommendations

**Code → Spec** (Undocumented):
- List features in code without specs
- Recommendation: Document or remove
- Assess if intentional or oversight

**Benefits**:
- Identifies gaps in both directions
- Finds deprecated code still active
- Ensures complete documentation

---

## Best Practices

### Do's ✅
- Read specs thoroughly before reviewing
- Be systematic - follow 6-step process
- Provide exact file paths and line numbers
- Be objective - report facts with evidence
- Prioritize findings clearly
- Default to report-only (no code changes)

### Don'ts ❌
- Don't assume or guess
- Don't skip documentation
- Don't modify code without explicit permission
- Don't make code quality judgments (focus on spec compliance only)
- Don't overwhelm with too much detail - prioritize

---

## Validation Checklist

Quick reference for thorough review:

**Spec Coverage**:
- [ ] All features in spec are checked
- [ ] All APIs/interfaces verified
- [ ] All data structures validated
- [ ] All behaviors confirmed

**Consistency**:
- [ ] Function signatures match
- [ ] Data models match
- [ ] Error handling matches
- [ ] Workflows match

**Gaps**:
- [ ] Missing features identified
- [ ] Extra features identified
- [ ] Deprecated features noted

**Report Quality**:
- [ ] Findings have code references
- [ ] Severity assigned
- [ ] Recommendations actionable
- [ ] No assumptions made

---

## Integration with platonic-coding

**Workflow**:
1. Use `platonic-coding SPECS` mode to maintain RFCs
2. Use `platonic-coding REVIEW` mode to validate code against RFCs
3. Review report identifies gaps/inconsistencies
4. User decides: update specs or fix code
5. Use `platonic-coding SPECS` mode to update spec history

**Example**:
```
1. Refine specs: "Use platonic-coding --specs-refine"
2. Review code: "Use platonic-coding --review to validate src/ against specs/"
3. Get report with findings
4. Fix code or update specs as appropriate
5. Update spec history with changes
```

---

## Report Templates

See `assets/review/` directory for:
- `review-checklist.md` - Comprehensive checklist
- `pr-review-template.md` - Structured report template

---

## Summary

This skill validates spec-to-code consistency through systematic 6-step process:

1. Understand specs
2. Generate checklist
3. Map to code
4. Review implementation
5. Identify discrepancies
6. Generate report (no code changes)

**Remember**: Default is report-only. Code modifications require explicit user consent.

For detailed examples and edge cases, see SKILL.md.