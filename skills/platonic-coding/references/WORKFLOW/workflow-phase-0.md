# Phase 0: Conceptual Design & Design Draft

## Current Phase

**[Phase 0] Conceptual Design & Design Draft**

## Objective

Produce a **design draft** that captures the conceptual design: principles, constraints, conceptual interfaces, design art, and related ideas. This is the input for Phase 1 (RFC formalization).

## Method

### Primary: Superpower Brainstorming Integration

**Check for installed brainstorming skill** and use it if available:

1. **Detect brainstorming skill**: Use Skill tool with `skill: "brainstorming"` to check if the official Superpower Brainstorming skill is installed.
2. **Invoke brainstorming**: If available, use the Skill tool to invoke the brainstorming skill:
   - The skill will guide the collaborative dialogue
   - Follow the brainstorming checklist: explore context, ask clarifying questions, propose approaches, present design
   - The skill will produce a design spec at `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
   - Copy/move the design spec to the Platonic Coding draft location (`docs/drafts/`)

3. **Benefits of brainstorming skill**:
   - Structured exploration of requirements and constraints
   - Multiple approach comparison with trade-offs
   - Incremental validation through section-by-section presentation
   - Built-in design document review process

### Fallback: Bundled Interactive Method

If the brainstorming skill is **not installed**, use the bundled interactive approach:

- **Interactive chat** with the user
- Use **optional items** to clarify:
  - Problem space and goals
  - Constraints and invariants
  - Domain knowledge and prior art
  - Core abstractions and system boundaries
  - Ambiguities to resolve before formalization

## Output

A **design draft** document that includes:

- Shared understanding of the problem and goals
- Principles and constraints
- Conceptual interfaces and boundaries
- Key abstractions and terminology
- Any design art or sketches (described or linked)

## Location

- **Default**: Save the design draft under `docs/drafts/`.
- **From brainstorming**: If using the brainstorming skill, it saves to `docs/superpowers/specs/` — copy to `docs/drafts/` for Platonic Coding workflow continuity.
- The user may provide an existing design draft from another location; in that case, use it as the Phase 1 input and do not require creating a new one in Phase 0.

## Optional Items (Examples for Fallback Mode)

Use as needed to structure the conversation:

- Short checklist of topics to cover (goals, constraints, interfaces, prior art)
- Yes/No or multiple-choice questions for key decisions
- Suggested sections for the draft (Overview, Principles, Constraints, Conceptual Interfaces, Open Questions)

## Handoff to Phase 1

Before leaving Phase 0:

1. Confirm the design draft is complete enough for RFC formalization.
2. Confirm the path to the design draft (e.g., `docs/drafts/<name>.md` or user-provided path).
3. Proceed to Phase 1 with this draft as the sole design input.
