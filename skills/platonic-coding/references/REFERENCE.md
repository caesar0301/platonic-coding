# Platonic Coding — Complete Reference

This document provides detailed information about all operations available in the Platonic Coding skill.

## Table of Contents

1. [Operation Overview](#operation-overview)
2. [Auto-Detection Algorithm](#auto-detection-algorithm)
3. [Detailed Operation Guides](#detailed-operation-guides)
   - [INITIALIZATION](#initialization)
   - [SPECIFICATION](#specification)
   - [IMPLEMENTATION](#implementation)
   - [REVIEW](#review)
   - [WORKFLOW](#workflow)
4. [Template Variables](#template-variables)
5. [File Structure](#file-structure)
6. [Workflow Examples](#workflow-examples)
7. [Troubleshooting](#troubleshooting)

---

## Operation Overview

The skill provides **24 operations** organized into five domains:

### INITIALIZATION (6 operations)
- `init-scaffold` — Create directories, config, templates for new project
- `init-scan` — Systematically analyze existing codebase
- `init-plan-modular-specs` — Propose RFC dependency graph from scan results
- `init-recover-conceptual` — Generate conceptual design spec from code
- `init-recover-architecture` — Generate architecture design spec from code
- `init-recover-impl-interface` — Generate impl interface design spec from code

### SPECIFICATION (7 operations)
- `specs-refine` — Comprehensive validation and update of all specs
- `specs-generate-history` — Update RFC change history
- `specs-generate-index` — Update RFC index with quick links
- `specs-generate-namings` — Update terminology reference
- `specs-validate-consistency` — Check cross-references and metadata
- `specs-check-taxonomy` — Verify terminology usage across specs
- `specs-check-compliance` — Validate RFC format against standard

### IMPLEMENTATION (5 operations)
- `impl-full` — End-to-end: spec → guide → plan → code + tests
- `impl-create-guide` — Create implementation guide from RFC
- `impl-code` — Implement code from existing guide
- `impl-validate-guide` — Check guide against RFC for contradictions
- `impl-update-guide` — Update guide when RFC changes

### REVIEW (1 comprehensive operation)
- `review` — Spec compliance review with 6-step process

### WORKFLOW (4 phases)
- `workflow-phase-0` — Conceptual design and design draft
- `workflow-phase-1` — RFC generation and refinement
- `workflow-phase-2` — Implementation (guide + code + tests)
- `workflow-phase-3` — Spec compliance review

---

## Auto-Detection Algorithm

When invoked without specific instructions, the skill detects project state and suggests next action.

### Decision Tree

```
Step 1: Check for .platonic.yml
├─ NOT FOUND → INIT Mode
│  ├─ Has source code? → init-recover (generate specs from code)
│  └─ No code? → init-scaffold (create infrastructure)
│
└─ FOUND → Continue to Step 2

Step 2: Check specs directory
├─ No RFCs found
│  ├─ Has design drafts? → WORKFLOW Phase 1 (generate RFCs)
│  └─ No drafts? → WORKFLOW Phase 0 (conceptual design)
│
└─ RFCs exist → Continue to Step 3

Step 3: Check implementation guides
├─ No impl guides found → WORKFLOW Phase 2 (implement specs)
│
└─ Guides exist → Continue to Step 4

Step 4: Check codebase state
├─ Has both specs and code → REVIEW Mode (check compliance)
│
└─ Well-initialized → SPECS Mode (refine specs)
```

### Confidence Levels

- **HIGH**: Clear indication (e.g., missing `.platonic.yml`)
- **MEDIUM**: Reasonable inference (e.g., has RFCs but no guides)
- **LOW**: Suggestion based on best practices (e.g., periodic refinement)

### User Override Behavior

Users can bypass auto-detection by providing explicit flags:

```
--init              → Run init-scaffold
--init-recover      → Run init-scan + recovery operations
--specs-<operation> → Run specific specs operation
--impl-<operation>  → Run specific impl operation
--review            → Run spec compliance review
--workflow          → Run full 4-phase workflow
--phase <N>         → Start workflow at specific phase
```

---

## Detailed Operation Guides

### INITIALIZATION

All initialization operations are in `references/INIT/`.

#### init-scaffold

**Reference**: `references/INIT/init-scaffold.md`

**Purpose**: Create the Platonic Coding infrastructure for a new project.

**When to use**: New projects without existing code, or when `.platonic.yml` is missing and no source code exists.

**Inputs**:
- Project name
- Language (optional)
- Framework (optional)
- Spec directory path (default: `docs/specs/`)
- Impl directory path (default: `docs/impl/`)
- Drafts directory path (default: `docs/drafts/`)

**Output**:
- `.platonic.yml` at project root
- `docs/specs/` with RFC infrastructure and templates
- `docs/impl/` directory
- `docs/drafts/` directory
- Template files for future RFCs

**Example**:
```
Use platonic-coding init-scaffold for project "Acme" (TypeScript/Next.js).
```

---

#### init-scan

**Reference**: `references/INIT/init-scan.md`

**Purpose**: Systematically analyze an existing codebase to understand its structure and design.

**When to use**: First step of recovery mode, before planning modular specs.

**Inputs**:
- Project root path (default: current directory)

**Output**:
- Comprehensive analysis of codebase structure
- Identified components, modules, and dependencies
- List of key abstractions and patterns
- Recommended RFC split (conceptual/architecture/impl-interface)

**Example**:
```
Use platonic-coding init-scan to analyze this codebase.
```

---

#### init-plan-modular-specs

**Reference**: `references/INIT/init-plan-modular-specs.md`

**Purpose**: Propose a modular RFC dependency graph based on scan results.

**When to use**: After init-scan, before generating RFCs. User must confirm the plan.

**Inputs**:
- Scan results from init-scan
- Maximum number of RFCs (default: 5)

**Output**:
- Proposed RFC dependency graph
- List of RFCs with titles and purposes
- Dependencies between RFCs
- **User confirmation required before proceeding**

**Example**:
```
Use platonic-coding init-plan-modular-specs from the scan results.
```

---

#### init-recover-conceptual

**Reference**: `references/INIT/init-recover-conceptual.md`

**Purpose**: Generate a conceptual design RFC (RFC-0001) from codebase analysis.

**When to use**: After user confirms the RFC plan, as the first RFC to generate.

**Inputs**:
- Scan results
- RFC plan
- Project configuration

**Output**:
- `docs/specs/RFC-0001.md` (Conceptual Design, Status: Draft)
- Vision, principles, terminology, invariants

**Example**:
```
Use platonic-coding init-recover-conceptual to generate RFC-0001.
```

---

#### init-recover-architecture

**Reference**: `references/INIT/init-recover-architecture.md`

**Purpose**: Generate architecture design RFCs from codebase analysis.

**When to use**: After init-recover-conceptual, for each major subsystem.

**Inputs**:
- Scan results
- RFC plan
- Conceptual design RFC

**Output**:
- One RFC per major subsystem (RFC-0002, RFC-0003, etc.)
- Each with Status: Draft
- Components, layers, data flow, constraints

**Example**:
```
Use platonic-coding init-recover-architecture to generate RFCs for subsystems.
```

---

#### init-recover-impl-interface

**Reference**: `references/INIT/init-recover-impl-interface.md`

**Purpose**: Generate implementation interface design RFCs (optional).

**When to use**: Only when explicitly requested by user, for detailed API contracts.

**Inputs**:
- Scan results
- Architecture RFCs
- Target modules

**Output**:
- RFCs with type definitions, interface contracts, error patterns
- Status: Draft

**Example**:
```
Use platonic-coding init-recover-impl-interface for the auth module.
```

---

### SPECIFICATION

All specification operations are in `references/SPECS/`.

#### specs-refine

**Reference**: `references/SPECS/specs-refine.md`

**Purpose**: Run comprehensive validation and update of all specifications.

**When to use**: Periodically to maintain spec quality, or after making changes to RFCs.

**Operations included**:
1. Validate consistency (cross-references, metadata)
2. Check taxonomy (terminology usage)
3. Check compliance (RFC standard)
4. Generate history, index, and namings

**Inputs**:
- Specs directory path (default: from `.platonic.yml`)
- RFC files to refine (default: all)

**Output**:
- Updated `rfc-history.md`
- Updated `rfc-index.md`
- Updated `rfc-namings.md`
- Validation report with errors and warnings

**Example**:
```
Use platonic-coding specs-refine to validate and update all specs.
```

---

#### specs-generate-history

**Reference**: `references/SPECS/specs-generate-history.md`

**Purpose**: Update RFC change history file.

**When to use**: After modifying RFCs, to track changes over time.

**Inputs**:
- Specs directory path
- RFC files to process

**Output**:
- Updated `rfc-history.md` with change log

**Example**:
```
Use platonic-coding specs-generate-history after updating RFC-0003.
```

---

#### specs-generate-index

**Reference**: `references/SPECS/specs-generate-index.md`

**Purpose**: Update RFC index file with quick links.

**When to use**: After adding or removing RFCs.

**Inputs**:
- Specs directory path
- RFC files to index

**Output**:
- Updated `rfc-index.md` with links to all RFCs

**Example**:
```
Use platonic-coding specs-generate-index to update the index.
```

---

#### specs-generate-namings

**Reference**: `references/SPECS/specs-generate-namings.md`

**Purpose**: Update terminology reference file.

**When to use**: After defining new terms in RFCs.

**Inputs**:
- Specs directory path
- RFC files to scan for terminology

**Output**:
- Updated `rfc-namings.md` with terminology definitions

**Example**:
```
Use platonic-coding specs-generate-namings to update terminology.
```

---

#### specs-validate-consistency

**Reference**: `references/SPECS/specs-validate-consistency.md`

**Purpose**: Check cross-references and metadata in RFCs.

**When to use**: To ensure RFCs are internally consistent.

**Inputs**:
- Specs directory path
- RFC files to validate

**Output**:
- Validation report with errors and warnings

**Example**:
```
Use platonic-coding specs-validate-consistency to check RFC cross-references.
```

---

#### specs-check-taxonomy

**Reference**: `references/SPECS/specs-check-taxonomy.md`

**Purpose**: Verify terminology usage across specs.

**When to use**: To ensure consistent use of defined terms.

**Inputs**:
- Specs directory path
- RFC files to check

**Output**:
- Report of terminology inconsistencies

**Example**:
```
Use platonic-coding specs-check-taxonomy to verify term usage.
```

---

#### specs-check-compliance

**Reference**: `references/SPECS/specs-check-compliance.md`

**Purpose**: Validate RFC format against standard.

**When to use**: To ensure RFCs follow the standard structure.

**Inputs**:
- Specs directory path
- RFC files to check
- RFC standard file (default: `rfc-standard.md`)

**Output**:
- Compliance report with issues

**Example**:
```
Use platonic-coding specs-check-compliance to validate RFC format.
```

---

### IMPLEMENTATION

All implementation operations are in `references/IMPL/`.

#### impl-full

**Reference**: `references/IMPL/impl-full.md`

**Purpose**: End-to-end implementation from spec through code with tests.

**When to use**: Default operation for implementing an RFC.

**Sub-workflow**:
1. Spec analysis — extract requirements from RFC
2. Impl guide design — create architecture doc (**confirmation gate**)
3. Coding plan — task breakdown (**confirmation gate**)
4. Coding — implement code with unit and integration tests

**Inputs**:
- RFC number or file path
- Target module/directory
- Language and framework context
- Auto-mode flag (skip confirmations)

**Output**:
- Implementation guide in `docs/impl/`
- Source code in target directory
- Unit tests
- Integration tests

**Example**:
```
# With confirmations (default)
Use platonic-coding impl-full for RFC-0042 in the acme-queue module.

# Auto-mode (no confirmations)
Use platonic-coding impl-full for RFC-0042 without stopping for confirmation.
```

---

#### impl-create-guide

**Reference**: `references/IMPL/impl-create-guide.md`

**Purpose**: Generate implementation guide from RFC (guide only, no coding).

**When to use**: When you need the architecture design before coding.

**Inputs**:
- RFC number or file path
- Target module/directory
- Language and framework context

**Output**:
- Implementation guide in `docs/impl/`

**Example**:
```
Use platonic-coding impl-create-guide for RFC-0001, guide only, no coding.
```

---

#### impl-code

**Reference**: `references/IMPL/impl-code.md`

**Purpose**: Implement code from existing implementation guide.

**When to use**: When guide already exists and you want to code.

**Inputs**:
- Implementation guide file path
- RFC file path (for reference)
- Target directory

**Output**:
- Source code in target directory
- Unit tests
- Integration tests

**Example**:
```
Use platonic-coding impl-code from docs/impl/RFC-0001-impl.md.
```

---

#### impl-validate-guide

**Reference**: `references/IMPL/impl-validate-guide.md`

**Purpose**: Check guide against RFC for contradictions.

**When to use**: To verify an implementation guide is spec-compliant.

**Inputs**:
- Implementation guide file path
- RFC file path

**Output**:
- Validation report confirming compliance or listing contradictions

**Example**:
```
Use platonic-coding impl-validate-guide for docs/impl/queue_impl.md.
```

---

#### impl-update-guide

**Reference**: `references/IMPL/impl-update-guide.md`

**Purpose**: Update guide when RFC changes.

**When to use**: After modifying an RFC, to synchronize the implementation guide.

**Inputs**:
- Implementation guide file path
- Updated RFC file path

**Output**:
- Updated implementation guide

**Example**:
```
Use platonic-coding impl-update-guide after RFC-0042 was revised.
```

---

### REVIEW

#### review

**Reference**: `references/REVIEW/review-spec-compliance.md`

**Purpose**: Validate code implementation matches specifications.

**Default Behavior**: Generates report WITHOUT modifying code

**6-Step Review Process**:
1. Understand specifications (RFCs, impl guides)
2. Generate functionality checklist
3. Map specs to code locations
4. Review implementation for each item
5. Identify discrepancies (missing, inconsistent, partial, extra)
6. Generate prioritized report

**Inputs**:
- RFC file(s) to review against
- Code directory to review
- Review level (basic, detailed, comprehensive)

**Output**:
- Structured compliance report with:
  - Summary (consistency rate, items reviewed)
  - Critical/high/medium issues
  - Functionality checklist status
  - Recommendations (prioritized)
  - Code references

**Example**:
```
# Review specific RFC implementation
Use platonic-coding review to check src/auth/ against RFC-0001.md.

# Comprehensive review
Use platonic-coding review to audit all code against all RFCs in docs/specs/.

# Gap analysis
Use platonic-coding review to identify gaps between specs/ and src/.
```

---

### WORKFLOW

All workflow phase references are in `references/WORKFLOW/`.

#### workflow-phase-0

**Reference**: `references/WORKFLOW/workflow-phase-0.md`

**Purpose**: Conduct interactive conceptual design and create design draft.

**Activities**:
- Clarify problem space, goals, constraints
- Explore domain knowledge
- Identify core abstractions
- Resolve ambiguity
- Create design draft document

**Output**:
- Design draft in `docs/drafts/`

**Example**:
```
Use platonic-coding workflow --phase 0 to design a user authentication feature.
```

---

#### workflow-phase-1

**Reference**: `references/WORKFLOW/workflow-phase-1.md`

**Purpose**: Turn design draft into formal RFC and refine it.

**Activities**:
1. Generate RFC from Phase 0 design draft
2. Call `specs-refine` to validate and update RFC

**Output**:
- RFC(s) in `docs/specs/` with Status: Draft

**Example**:
```
# Typically runs automatically after Phase 0
# Or explicit start:
Use platonic-coding workflow --phase 1 with RFC-0042.
```

---

#### workflow-phase-2

**Reference**: `references/WORKFLOW/workflow-phase-2.md`

**Purpose**: Create implementation guide and code with tests.

**Activities**:
Call `impl-full` operation which runs:
1. Spec analysis
2. Impl guide design (**confirmation gate**)
3. Coding plan (**confirmation gate**)
4. Coding with tests

**Output**:
- Implementation guide in `docs/impl/`
- Source code with unit and integration tests

**Example**:
```
Use platonic-coding workflow --phase 2 to implement RFC-0042.
```

---

#### workflow-phase-3

**Reference**: `references/WORKFLOW/workflow-phase-3.md`

**Purpose**: Review implementation against specs and impl guides.

**Activities**:
Call `review` operation to validate code compliance

**Output**:
- Review and compliance report

**Example**:
```
# Typically runs automatically after Phase 2
# Or explicit start:
Use platonic-coding workflow --phase 3 to review RFC-0042 implementation.
```

---

## Template Variables

Templates use `{{PLACEHOLDER}}` syntax. Common variables:

### Project-Level
- `{{PROJECT_NAME}}` — Project name
- `{{LANGUAGE}}` — Programming language
- `{{FRAMEWORK}}` — Framework or stack

### RFC-Level
- `{{RFC_NUMBER}}` — RFC identifier (e.g., "0001")
- `{{RFC_TITLE}}` — RFC title
- `{{RFC_STATUS}}` — Status (Draft, Active, Deprecated, Superseded)
- `{{RFC_KIND}}` — Kind (Conceptual Design, Architecture Design, Impl Interface Design)
- `{{DATE}}` — Current date
- `{{AUTHOR}}` — Author name

### Paths
- `{{SPECS_PATH}}` — Specs directory path (from `.platonic.yml`)
- `{{IMPL_PATH}}` — Implementation guides path
- `{{DRAFTS_PATH}}` — Design drafts path

---

## File Structure

```
<project-root>/
├── .platonic.yml                   # Project config
├── docs/
│   ├── specs/                      # RFC specifications
│   │   ├── rfc-standard.md          # RFC process & conventions
│   │   ├── rfc-history.md           # Change history
│   │   ├── rfc-index.md             # Spec index
│   │   ├── rfc-namings.md           # Terminology reference
│   │   ├── RFC-0001.md              # Individual RFC
│   │   ├── RFC-0002.md
│   │   └── templates/               # Templates for future RFCs
│   │       ├── rfc-template.md
│   │       ├── conceptual-design.md
│   │       ├── architecture-design.md
│   │       └── impl-interface-design.md
│   │
│   ├── impl/                       # Implementation guides
│   │   ├── README.md
│   │   ├── RFC-0001-impl.md         # Impl guide for RFC-0001
│   │   └── RFC-0002-impl.md
│   │
│   └── drafts/                     # Phase 0 design drafts
│       ├── README.md
│       └── user-auth-design.md
│
└── <source-code>/                  # Your implementation
```

---

## Workflow Examples

### Example 1: Greenfield Project

```
# Initialize new project
Use platonic-coding to set up my new project "Acme" (TypeScript/Next.js).
→ Auto-detects no .platonic.yml → runs init-scaffold

# Start full workflow
Use platonic-coding workflow to design user authentication.
→ Phase 0: Interactive design → design draft
→ Phase 1: Generate RFC → specs-refine
→ Phase 2: impl-full (guide + code + tests)
→ Phase 3: review
→ FINISHED

# Periodic maintenance
Use platonic-coding to refine all specs.
→ Auto-detects specs exist → runs specs-refine
```

### Example 2: Existing Codebase

```
# Initialize and recover specs
Use platonic-coding to adopt Platonic Coding for this existing project.
→ Auto-detects no .platonic.yml + has code → runs init-recover
→ Scans code, plans RFCs, generates Draft RFCs

# Review current implementation
Use platonic-coding to check if code matches recovered specs.
→ Auto-detects specs + code → runs review

# Implement new feature
Use platonic-coding workflow --phase 0 to add notifications.
→ Full workflow from design through implementation
```

### Example 3: Specific Operations

```
# Just create implementation guide
Use platonic-coding impl-create-guide for RFC-0001, guide only.

# Just implement from existing guide
Use platonic-coding impl-code from docs/impl/RFC-0001-impl.md.

# Update terminology
Use platonic-coding specs-generate-namings.

# Validate guide against RFC
Use platonic-coding impl-validate-guide for docs/impl/queue_impl.md.
```

---

## Troubleshooting

### Auto-detection suggests wrong action

**Solution**: Use explicit mode flags (`--init`, `--specs`, `--impl`, `--review`, `--workflow`)

Example:
```
# Instead of relying on auto-detection
Use platonic-coding --init-recover
```

---

### Templates have wrong placeholders

**Solution**: Check `.platonic.yml` for configured paths, and ensure placeholders match actual values.

---

### Implementation guide contradicts RFC

**Solution**:
1. Run `impl-validate-guide` to identify contradictions
2. Either update the guide (`impl-update-guide`) or modify the RFC and re-validate

---

### Specs not showing in index

**Solution**:
1. Ensure RFC files follow `RFC-NNNN.md` naming convention
2. Run `specs-generate-index` to update the index

---

### Terminology not appearing in namings

**Solution**:
1. Ensure terms are defined in RFCs using consistent format
2. Run `specs-generate-namings` to extract terminology

---

### Review report has missing code references

**Solution**:
1. Ensure code follows naming conventions from specs
2. Provide more specific search terms or paths
3. Use grep/search tools to locate implementation manually

---

### Workflow stops at confirmation gate

**Expected behavior**: Default behavior pauses for user review after impl guide and coding plan.

**To skip confirmations**:
```
Use platonic-coding impl-full for RFC-0042 without confirmation.
```

---

## Summary

The Platonic Coding skill provides a unified, intelligent entry point for specification-driven development. Use auto-detection to guide next steps, or invoke specific operations explicitly with mode flags.

For detailed operation guides, see the reference files in each domain directory:
- `references/INIT/` — Initialization operations
- `references/SPECS/` — Specification management
- `references/IMPL/` — Implementation operations
- `references/REVIEW/` — Review operations
- `references/WORKFLOW/` — Workflow orchestration