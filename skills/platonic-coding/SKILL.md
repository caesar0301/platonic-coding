---
name: platonic-coding
description: Intelligent orchestrator for Platonic Coding workflow. Auto-detects project state and routes to the right next step—initialize a project, run the recovery flow for existing code, formalize drafts into RFCs, refine specs, implement from guides with tests, or review code compliance. Single entry point for the complete specification-driven development lifecycle.
license: MIT
metadata:
  version: "2.2.0"
  author: "Xiaming Chen"
  category: "workflow"
  replaces:
    - platonic-init
    - platonic-specs
    - platonic-impl
    - platonic-code-review
    - platonic-workflow
  integrates:
    - platonic-brainstorming (optional)
---

# Platonic Coding

Intelligent orchestrator for the complete **specification-driven development lifecycle**. Auto-detects project state and executes the appropriate workflow phases—initialization, specification management, implementation, or review. Integrates with `platonic-brainstorming` as an optional accelerator in Phases 1 and 2 for structured design exploration and refinement.

## When to Use This Skill

Use this skill when you need to:

- **Bootstrap** a new project with Platonic Coding infrastructure
- **Adopt** Platonic Coding for an existing codebase (recover specs from code)
- **Manage** RFC specifications (validate, refine, generate indices)
- **Implement** features from RFC specs with guides and tests
- **Review** code for spec compliance
- **Run** the full workflow from design → RFC → code → review

**Keywords**: platonic coding, specs, RFC, implementation, review, workflow, spec-driven, initialization

## Intelligent Auto-Detection

When invoked without specific instructions, this skill **automatically detects** project state and suggests the next action.

### Detection Algorithm

```
1. Does .platonic.yml exist?
   → NO:  Run INIT mode
          • Has source code? → run the recovery flow (`init-scan` -> recovery operations)
          • No code? → `init-scaffold`

2. Has specs directory but no RFCs?
   → Has design drafts? → WORKFLOW Phase 1 (draft -> RFC -> `specs-refine`)
   → No drafts? → WORKFLOW Phase 1 (start from conceptual design, optionally with `platonic-brainstorming`)

3. Has RFCs but no implementation guides?
   → WORKFLOW Phase 2 (`impl-full` or `impl-create-guide`, optionally with `platonic-brainstorming` first)

4. Has RFCs and implementation guides?
   → If implementation is still in progress, resume Phase 2 (IMPL mode)
   → If implementation appears complete or the user asks for verification, run Phase 3 (REVIEW mode)

5. Has specs but the next step is ambiguous?
   → Resume from the current workflow phase or ask whether the user wants refine / implement / review
```

### User Override

Users can explicitly specify operations to bypass auto-detection. Prefer the canonical operation names directly:

- `init-scaffold`
- `init-scan`
- `init-plan-modular-specs`
- `init-recover-conceptual`
- `init-recover-architecture`
- `init-recover-impl-interface`
- `specs-refine` and other `specs-*` operations
- `impl-full` and other `impl-*` operations
- `review`
- `workflow --phase <N>`

If you support shorthand like `--init` or `--workflow`, treat it as a convenience wrapper around the canonical operation flow above.

## Core Workflow Phases

After initialization, the Platonic Coding workflow has three phases:

| Phase | Focus | Output | Platonic Brainstorming (Optional) |
|-------|-------|--------|-----------------------------------|
| **1** | RFC Specification | Design draft → RFC → `specs-refine` | ✅ Use for conceptual exploration before formalization |
| **2** | Implementation | Implementation guide + Code + Tests | ✅ Use for design refinement before coding |
| **3** | Spec Compliance Review | Review report | ❌ Not applicable |

**Phase Flow**:
- Each phase optionally integrates `platonic-brainstorming` at the beginning for structured exploration
- All phases can run sequentially (full workflow) or independently (explicit invocation)
- Phase 1 and 2 can start from scratch or resume from existing artifacts (drafts, RFCs, guides)

## Operation Modes

### INIT Mode

**Purpose**: Bootstrap Platonic Coding infrastructure

**Operations** (see `references/INIT/`):
- `init-scaffold`: Create `.platonic.yml`, directories, templates
- `init-scan`: Analyze existing codebase (recovery mode)
- `init-plan-modular-specs`: Propose RFC dependency graph
- `init-recover-conceptual`: Generate conceptual design RFC
- `init-recover-architecture`: Generate architecture design RFCs
- `init-recover-impl-interface`: Generate impl interface RFCs (optional)

**Auto-detection**: Runs when `.platonic.yml` is missing

**Examples**:
```
# Auto-detect: new vs existing project
Use platonic-coding to set up my new project.

# Explicit: existing codebase recovery flow
Use platonic-coding init-scan, then recover the core specs for this codebase.

# Explicit: greenfield scaffold
Use platonic-coding init-scaffold for project "Acme" (TypeScript/Next.js).
```

**Reference**: See `references/REFERENCE.md` → INITIALIZATION section

---

### SPECS Mode

**Purpose**: Manage RFC specifications

**Operations** (see `references/SPECS/`):
- `specs-refine`: Comprehensive validation and update
- `specs-generate-history`: Update RFC change history
- `specs-generate-index`: Update RFC index
- `specs-generate-namings`: Update terminology reference
- `specs-validate-consistency`: Check cross-references and metadata
- `specs-check-taxonomy`: Verify terminology usage
- `specs-check-compliance`: Validate against RFC standard

**Auto-detection**: Suggested when specs exist but need validation

**Examples**:
```
# Auto-detect: refine all specs
Use platonic-coding to validate and update all specifications.

# Explicit: specific operation
Use platonic-coding specs-generate-index to update the RFC index.

# Explicit: comprehensive refinement
Use platonic-coding specs-refine to run all validation and generation operations.
```

**Reference**: See `references/REFERENCE.md` → SPECIFICATION section

---

### IMPL Mode

**Purpose**: Translate RFCs into implementation guides and code

**Operations** (see `references/IMPL/`):
- `impl-full`: End-to-end: spec → guide → plan → code + tests (default)
- `impl-create-guide`: Generate implementation guide from RFC
- `impl-code`: Implement code from existing guide
- `impl-validate-guide`: Check guide against RFC for contradictions
- `impl-update-guide`: Update guide when RFC changes

**Auto-detection**: Suggested when an RFC is ready for implementation and no implementation guide exists for that target

**Confirmation Gates**: By default, pauses after impl guide and coding plan for user confirmation. Can be overridden with "no confirmations" or auto-mode.

**Examples**:
```
# Auto-detect: implement from RFC
Use platonic-coding to implement RFC-042-message-queue (Message Queue) in the acme-queue module.

# Explicit: create guide only
Use platonic-coding impl-create-guide for RFC-001-user-authentication, guide only, no coding.

# Explicit: implement from existing guide
Use platonic-coding impl-code from docs/impl/IG-001-user-authentication.md.

# Auto-mode: no confirmations
Use platonic-coding impl-full for RFC-003-notification-routing without stopping for confirmation.
```

**Reference**: See `references/REFERENCE.md` → IMPLEMENTATION section

---

### REVIEW Mode

**Purpose**: Validate code implementation against specifications

**Default Behavior**: Generates compliance report WITHOUT modifying code

**Review Process**:
1. Understand specifications (RFCs, impl guides)
2. Generate functionality checklist
3. Map specs to code locations
4. Review implementation for each item
5. Identify discrepancies (missing, inconsistent, partial, extra)
6. Generate prioritized report with recommendations

**Auto-detection**: Suggested when the relevant RFC and implementation appear complete and the user wants compliance verification

**Examples**:
```
# Auto-detect: review specific RFC implementation
Use platonic-coding to review src/auth/ against RFC-001-user-authentication.md.

# Explicit: comprehensive review
Use platonic-coding review to audit all code against all RFCs in docs/specs/.

# Explicit: gap analysis
Use platonic-coding review to identify gaps between specs/ and src/.
```

**Reference**: See `references/REFERENCE.md` → REVIEW section

---

### WORKFLOW Mode

**Purpose**: Orchestrate the full 3-phase workflow from RFC specification to review

**Phases**:

**Phase 1: RFC Specification**
- Optional: `platonic-brainstorming` for conceptual design (see workflow-overview.md)
- Create design draft → generate RFC → run `specs-refine`

**Phase 2: Implementation**
- Optional: `platonic-brainstorming` for design refinement (see workflow-overview.md)
- Call `impl-full` (guide → plan → code + tests) or `impl-code` from existing guide

**Phase 3: Spec Compliance Review**
- Call `review` to validate implementation against specs
- Generate compliance report

**FINISHED**: Summary and next steps

**Phase Visibility**: Always show current phase at each step (see workflow-overview.md for details)

### Process Flow

For detailed workflow diagram and process flow, see `references/WORKFLOW/workflow-overview.md`.

**Auto-detection**: Suggested when project is initialized and ready for new features

**Platonic Brainstorming Integration**: Optional in Phases 1 and 2 (see workflow-overview.md for pattern)

**Platonic Brainstorming Integration**:
- Phase 1: Use before RFC formalization to explore requirements, alternatives, and design trade-offs
- Phase 2: Use before implementation to refine architecture decisions and validate approach
- Phase 3: Not applicable (review phase is purely analytical)

**Examples**:
```
# Run full workflow from Phase 1
Use platonic-coding workflow to implement a user preferences feature.

# Start at Phase 1 with brainstorming
Use platonic-coding workflow to design and implement a user authentication system, starting with platonic-brainstorming.

# Start at specific phase
Use platonic-coding workflow --phase 2 to implement RFC-042-message-queue.

# Resume workflow (auto-detected)
Use platonic-coding to continue from where we left off.

# Phase 2 with design refinement
Use platonic-coding workflow --phase 2 for RFC-042, use platonic-brainstorming to refine the implementation approach first.
```

**Reference**: See `references/REFERENCE.md` → WORKFLOW section

## Default Paths

| Artifact | Default Path | Naming Convention | Configurable in .platonic.yml |
|----------|--------------|-------------------|-------------------------------|
| Design drafts | `docs/drafts/` | `YYYY-MM-DD-<topic>-design.md` | Yes |
| RFC specs | `docs/specs/` | `RFC-<NNN>-<brief-semantic-name>.md` (e.g., `RFC-001-world-view.md`) | Yes |
| Implementation guides | `docs/impl/` | `IG-<number>-semantic-short-desc.md` (e.g., `IG-053-cli-command-nesting.md`) | Yes |

## Templates

All templates are provided in the `assets/` directory:

- **Project scaffolding**: `assets/templates/`
- **RFC templates**: `assets/specs/`
- **Implementation templates**: `assets/implementation/`
- **Review templates**: `assets/review/`

Templates use `{{PLACEHOLDER}}` syntax. See individual reference files for details.

## Best Practices

1. **Trust auto-detection**: Let the skill suggest next steps based on project state
2. **Override when needed**: Use explicit operation names or workflow phase selectors when auto-detection is not the right fit
3. **Review generated artifacts**: All generated RFCs and guides are Draft—review before use
4. **Run refine regularly**: Keep specs validated and indices updated
5. **Use confirmation gates**: Default behavior pauses for review—don't skip unless confident
6. **Report-only by default**: Review mode generates reports, modify code only when explicitly requested

## Dependencies

- Read/write access to project directories
- Read access to `.platonic.yml` for configuration
- Understanding of target language and framework
- Ability to scan and read source code files (for recovery and review)

## Integration Example

Complete workflow from greenfield to reviewed implementation:

```
# Day 1: Initialize
Use platonic-coding to set up my new project "Acme" (TypeScript/Next.js).

# Day 2: Start workflow (Phase 1)
Use platonic-coding workflow to implement a user authentication feature.
→ Phase 1: RFC Specification (design draft → RFC → specs-refine)
→ Phase 2: Implementation (impl-full: guide → code → tests)
→ Phase 3: Spec Compliance Review (review report)

# Day 2: Alternative with brainstorming
Use platonic-coding workflow to design a notification system, starting each phase with platonic-brainstorming.
→ Phase 1: platonic-brainstorming → design draft → RFC → specs-refine
→ Phase 2: platonic-brainstorming (design refinement) → impl-full
→ Phase 3: review

# Day 3: Maintenance
Use platonic-coding to refine all specs and validate consistency.

# Day 4: New feature
Use platonic-coding workflow --phase 1 to add a notification system.
```

See `references/REFERENCE.md` for detailed operation guides.
