# {{PROJECT_NAME}} Design Drafts

This directory contains design drafts produced during Phase 1 (RFC Specification) of the Platonic Coding workflow.

## Purpose

Design drafts capture the conceptual design before formalization into RFC specifications. They include:

- Problem space and goals
- Principles and constraints
- Conceptual interfaces and boundaries
- Key abstractions and terminology
- Design sketches and exploration

## Workflow

```
Design Draft (this directory)
        |
        v
RFC Specification (specs/)
        |
        v
Implementation Guide (.platonic/impl/)
        |
        v
Code
```

## Creating a New Draft

Use the **platonic-coding** skill starting at Phase 1:

```
Use platonic-coding workflow to start Phase 1 for a new feature.
```

Or create a draft manually -- any markdown file in this directory works.

## After a Draft is Complete

Once a draft captures the shared understanding, continue Phase 1 to formalize it into an RFC:

```
Use platonic-coding workflow to proceed with the draft at .platonic/drafts/<name>.md
```

This converts the draft into a formal RFC specification within Phase 1.

## After a Draft is Formalized (Draft Retirement Gate)

A design draft is a **transient input** — once it has been formalized into an RFC (or an implementation guide), its content lives in the spec tree. At the **Draft Retirement Gate** the skill offers (via `ask_user`) to:

- **Remove the consumed draft file(s)** from this directory, and
- **Cleanse every reference** to those drafts across the project (RFC/IG metadata, supporting docs, indexes, other drafts, config, and source comments), repointing provenance to the new RFC/IG ID.

Removal and reference cleansing are atomic and verified (zero remaining references). You can also choose to **keep** the drafts for traceability. The gate is never auto-skipped by quick-pass routing. See `references/workflow/draft-retirement.md` for the full procedure.
