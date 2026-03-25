# Full Implementation (End-to-End)

Run the complete implementation sub-workflow from RFC spec to working code with tests.

## Overview

This is the **default operation** when a user asks to implement an RFC. It orchestrates the four-step sub-workflow:

```
Spec Analysis → Impl Guide → Coding Plan → Coding + Tests
                    ▲              ▲
                    │              │
              [confirm gate] [confirm gate]
```

## Prerequisites

1. **RFC exists**: The source RFC document is available (Draft or Frozen status)
2. **Scope is clear**: Target module/crate/package is specified or can be inferred
3. **Language is known**: Target programming language and framework are specified or detectable
4. **Project context**: Existing codebase architecture is understood

## Input Requirements

| Input | Required | Description |
|-------|----------|-------------|
| RFC Document | Yes | Path to source RFC specification |
| Target Module | Yes | Name of implementing module/crate/package |
| Language | Yes | Target programming language |
| Framework | No | Framework if applicable |
| Auto Mode | No | Skip confirmation gates (default: false) |
| Output Path | No | Where to save the guide (default: `docs/impl/`) |

## Sub-Workflow

### Step 1: Spec Analysis

Read the RFC document and extract:

1. **Core Concepts**: Key abstractions and entities defined
2. **Requirements**: What must be implemented (MUST, SHALL)
3. **Constraints**: Limitations and invariants
4. **Interfaces**: Expected APIs and protocols
5. **Data Structures**: Defined types and schemas
6. **Behaviors**: Expected runtime behavior
7. **Dependencies**: Related RFCs and external systems

Create a requirements checklist for traceability.

### Step 2: Impl Guide Design

Create a concrete implementation guide. Follow `create-guide.md` for the full procedure.

Key outputs:
- Module/directory structure
- Type definitions with fields
- Interface/trait definitions with signatures
- Implementation details for complex algorithms
- Error handling strategy
- Testing strategy

**Confirmation Gate**: Present the impl guide to the user and wait for confirmation before proceeding. Show a summary of:
- Module structure
- Key types and interfaces
- Major design decisions

**Skip conditions**:
- User explicitly requested auto mode
- User said "no confirmations" or equivalent

### Step 3: Coding Plan

Break the implementation guide into an ordered list of coding tasks:

1. **Task granularity**: Each task targets one file or a small group of closely related files
2. **Dependency order**: Tasks are ordered so each can build on previous ones
3. **Test pairing**: Each functional task has a corresponding test task
4. **Clear scope**: Each task describes what to create/modify and references the guide section

Use the template from `assets/coding-plan-template.md`.

**Confirmation Gate**: Present the coding plan to the user and wait for confirmation.

**Skip conditions**:
- User explicitly requested auto mode
- Trivially small scope: single-file change with < ~50 lines of implementation logic
- User said "no confirmations" or equivalent

### Step 4: Coding

Execute the coding plan task by task:

1. **Follow the guide as law**: Code is a realization of the impl guide, not a source of new requirements
2. **No speculative design**: Do not add behavior or interfaces not in the RFC or impl guide
3. **Language idioms**: Use idiomatic patterns for the target language and framework
4. **Integration**: Wire into existing code (imports, configuration, entry points) as described in the guide
5. **Unit tests**: Write unit tests for each component/module
6. **Integration tests**: Write integration tests for cross-component behavior and data flow
7. **Verify build**: Ensure the project builds and tests pass

If something is missing from the guide that is needed for implementation, document the gap and suggest a guide update rather than inventing behavior.

## Confirmation Gate Logic

The skill uses this decision process for confirmation gates:

```
Has user explicitly requested auto mode?
  → YES: Skip all gates
  → NO: Continue

Is the user instruction ambiguous about confirmations?
  → YES: Confirm (default)
  → NO: Continue

Is the scope trivially small (single file, < ~50 lines)?
  → YES: May skip coding plan gate (still confirm impl guide)
  → NO: Confirm both gates

Are there ambiguous RFC requirements with multiple valid interpretations?
  → YES: Always confirm impl guide
```

## Output

- **Implementation guide** in `docs/impl/` (e.g., `RFC-NNNN-impl.md` such as `RFC-0001-impl.md`)
- **Source code** in the codebase following the guide
- **Unit tests** for individual components
- **Integration tests** for cross-component behavior

## Example

**Input**:
- RFC: `docs/specs/RFC-0042.md` (Message Queue Protocol)
- Target Module: `acme-queue`
- Language: Rust
- Framework: Tokio

**Sub-Workflow Execution**:
1. Spec Analysis: Extract 15 requirements, 4 invariants, 6 types from RFC-0042
2. Impl Guide: Present `docs/impl/RFC-0042-impl.md` → user confirms
3. Coding Plan: 8 tasks (4 implementation + 4 test tasks) → user confirms
4. Coding: Implement all tasks, run tests, verify build

**Output**:
- `docs/impl/RFC-0042-impl.md`
- `acme-queue/src/lib.rs`, `acme-queue/src/message.rs`, etc.
- `acme-queue/tests/unit/`, `acme-queue/tests/integration/`
