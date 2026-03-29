# Phase 2: Implementation

## Current Phase

**[Phase 2] Implementation**

## Objective

Produce a **concrete implementation guide** from the Phase 1 RFC spec, then **implement the code** with unit and integration tests. Optionally use `platonic-brainstorming` for design refinement before creating the implementation guide.

## Process

### Optional Step 0: Platonic Brainstorming

For design refinement and architecture validation before coding, optionally invoke `platonic-brainstorming` following the standard integration pattern (see workflow-overview.md).

### Step 1: Identify RFC and Scope

- If not specified, ask the user which RFC (by number/index) should be implemented
- Determine target module/crate/package and language/framework (from user or existing codebase)

### Step 2: Run IMPL Mode Full Operation

- **Use platonic-coding IMPL mode** with the **impl-full** operation
- Read `references/IMPL/impl-full.md` and follow it
- The sub-workflow runs four steps:

  1. **Spec Analysis**: Read the RFC, extract all requirements, constraints, invariants
  2. **Impl Guide Design**: Create a concrete implementation guide document
     - **Confirmation gate**: Present the guide to the user and wait for approval (default behavior)
     - Save to `docs/impl/` using naming convention `IG-<number>-semantic-short-desc.md`
  3. **Coding Plan**: Break the guide into ordered tasks with file-level changes and test tasks
     - **Confirmation gate**: Present the coding plan to the user and wait for approval (default behavior)
  4. **Coding**: Implement all tasks, write unit and integration tests

- Inputs to impl-full:
  - RFC document path (e.g., `docs/specs/RFC-0001-world-view.md`)
  - Target module name
  - Language and optional framework
  - Output path for guide: default `docs/impl/`
  - Design insights from brainstorming (if used): inform guide creation

### Step 3: Verify Outputs

- Confirm the implementation guide exists in `docs/impl/`
- Confirm source code has been written following the guide
- Confirm unit and integration tests are present and passing

## Inputs

- **RFC spec**: From Phase 1 (default `docs/specs/`)
- **RFC number/index** (optional): If not specified, ask which RFC to implement
- **Target module/language/framework**: From user or inferred from codebase
- **Platonic brainstorming integration** (optional): Use for design refinement before guide creation

## Output

- **Implementation guide** document in `docs/impl/` using naming convention `IG-<number>-semantic-short-desc.md`
- **Source code** in the codebase implementing the feature
- **Unit tests** for individual components
- **Integration tests** for cross-component behavior
- Guide is concrete, language- and framework-aware, and does not contradict the RFC
- Code is traceable to the implementation guide and RFC

## Handoff to Phase 3

- Confirm which code paths and which RFC(s) / impl guide(s) should be reviewed
- Proceed to Phase 3: **Spec Compliance Review** (REVIEW mode)

## Examples

```
# Phase 2 with brainstorming for design refinement
Use platonic-coding workflow --phase 2 for RFC-0042-message-queue, use platonic-brainstorming to refine the implementation approach.

# Phase 2 standard flow
Use platonic-coding workflow --phase 2 to implement RFC-0001-user-authentication.

# Phase 2 from existing guide (impl-code operation)
Use platonic-coding impl-code from docs/impl/IG-001-user-authentication.md.
```