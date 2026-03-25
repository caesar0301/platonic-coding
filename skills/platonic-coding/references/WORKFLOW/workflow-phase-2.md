# Phase 2: Implementation (Guide + Code)

## Current Phase

**[Phase 2] Implementation (Guide + Code)**

## Objective

Produce a **concrete implementation guide** from the Phase 1 RFC spec, then **implement the code** with unit and integration tests. This phase uses the **platonic-coding IMPL mode** which runs a four-step sub-workflow with user confirmation gates.

## Inputs

- **RFC spec**: From Phase 1 (default `docs/specs/`, filename convention `RFC-NNNN.md`, e.g. `RFC-0001.md`).
- **RFC number/index** (optional): If the user has not specified for which RFC to implement, ask (e.g., "For which RFC should I create the implementation?").
- **Target module/language/framework**: From user or inferred from codebase.

## Process

### Step 1: Identify RFC and Scope

- If not specified, ask the user which RFC (by number/index) should be implemented.
- Determine target module/crate/package and language/framework (from user or existing codebase).

### Step 2: Run IMPL Mode Full Operation

- **Use platonic-coding IMPL mode** with the **impl-full** operation.
- Read `references/IMPL/impl-full.md` and follow it.
- The sub-workflow runs four steps:

  1. **Spec Analysis**: Read the RFC, extract all requirements, constraints, invariants
  2. **Impl Guide Design**: Create a concrete implementation guide document
     - **Confirmation gate**: Present the guide to the user and wait for approval (default behavior)
     - Save to `docs/impl/` (e.g., `docs/impl/RFC-0001-impl.md`)
  3. **Coding Plan**: Break the guide into ordered tasks with file-level changes and test tasks
     - **Confirmation gate**: Present the coding plan to the user and wait for approval (default behavior)
  4. **Coding**: Implement all tasks, write unit and integration tests

- Inputs to impl-full:
  - RFC document path (e.g., `docs/specs/RFC-0001.md`)
  - Target module name
  - Language and optional framework
  - Output path for guide: default `docs/impl/`

### Step 3: Verify Outputs

- Confirm the implementation guide exists in `docs/impl/`
- Confirm source code has been written following the guide
- Confirm unit and integration tests are present and passing

## Output

- **Implementation guide** document in `docs/impl/` (e.g., `RFC-0001-impl.md`)
- **Source code** in the codebase implementing the feature
- **Unit tests** for individual components
- **Integration tests** for cross-component behavior
- Guide is concrete, language- and framework-aware, and does not contradict the RFC
- Code is traceable to the implementation guide and RFC

## Default Location

- Implementation guides: `docs/impl/`
- Source code: existing codebase directories

## Handoff to Phase 3

- Confirm which code paths and which RFC(s) / impl guide(s) should be reviewed.
- Proceed to Phase 3: **REVIEW mode** for code vs specs and impl guides.
