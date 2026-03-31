# Phase 1: RFC Specification

## Current Phase

**[Phase 1] RFC Specification**

## Objective

Produce a formal **RFC specification** (Status: Draft) and refine it using **platonic-coding SPECS mode**. Optionally use `platonic-brainstorming` for conceptual design exploration before creating the RFC.

## Process

### Optional Step 0: Platonic Brainstorming

For conceptual design exploration before RFC formalization, optionally invoke `platonic-brainstorming` following the standard integration pattern (see workflow-overview.md).

### Step 1: Create Design Draft (if not from brainstorming)

If not using `platonic-brainstorming`, or if continuing from an approved draft:

- **Check for existing draft**: If the user provides an existing design draft, use it directly
- **Interactive creation**: If no draft exists, create one through interactive dialogue covering:
  - Problem space and goals
  - Constraints and invariants
  - Domain knowledge and prior art
  - Core abstractions and system boundaries
  - Key terminology and concepts
- **Save draft**: Write to `docs/drafts/YYYY-MM-DD-<topic>-design.md` (or user-specified path)

### Step 2: Determine RFC Index

- If the user has not specified an RFC number/index:
  - Ask: "Which RFC number should this be (e.g., RFC-001)?"
  - Or suggest the next available index based on existing files in `docs/specs/`

### Step 3: Generate RFC from Design Draft

- Read the design draft
- Produce a formal RFC document that:
  - Follows the project's RFC format (see `docs/specs/rfc-standard.md` if present)
  - Includes: title, status (Draft), summary, motivation, detailed specification (entities, relations, invariants, constraints), terminology, and references
  - Preserves all material from the design draft in a structured, formal form
- Write the RFC to the specs directory using convention `RFC-NNN-<brief-semantic-name>.md`

### Step 4: Refine with SPECS Mode

- **Call platonic-coding SPECS mode** to refine the specifications
- Use the **specs-refine** operation: read `references/SPECS/specs-refine.md` and apply it to the specs directory
- This updates history, index, namings, and validates consistency and compliance

## Inputs

- **Platonic brainstorming integration** (optional): Use skill for conceptual design exploration
- **Design draft** (optional): From brainstorming output, user-provided, or created interactively
- **RFC number/index** (optional): If not specified, ask or suggest next available index

## Output

- RFC document(s) in `docs/specs/` with Status **Draft**
- Updated supporting files (history, index, namings) from refine
- Optional: Design draft in `docs/drafts/` if created during this phase

## Handoff to Phase 2

- Confirm the RFC path and identifier (e.g., RFC-001)
- Proceed to Phase 2 to create the implementation guide and code for this RFC
- Optionally use `platonic-brainstorming` again in Phase 2 for design refinement before coding

## Examples

```
# Phase 1 with brainstorming
Use platonic-coding workflow --phase 1 to design user authentication, starting with platonic-brainstorming.

# Phase 1 from existing draft
Use platonic-coding workflow --phase 1 with docs/drafts/2024-03-15-user-auth-design.md.

# Phase 1 without brainstorming (interactive creation)
Use platonic-coding workflow --phase 1 to create RFC for notification system.
```