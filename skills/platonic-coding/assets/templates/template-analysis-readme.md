# {{PROJECT_NAME}} Codebase Analysis

This directory holds **analysis reports** produced during Platonic Coding's
recovery flow (`init-scan` and downstream `init-recover-*` operations) and
during BRAINSTORM-mode exploration of an existing codebase.

## Purpose

Analysis reports capture a structured snapshot of a codebase **before** it is
translated into RFC specifications. They are the bridge between raw source code
and formal specs:

- Project metadata (purpose, language, framework, dependencies, entry points)
- Structural analysis (modules, dependency graph, layer classification)
- Type and interface catalog (public types, cross-boundary interfaces)
- Behavioral analysis (data flows, invariants, design patterns)
- Synthesis (subsystem boundaries, API surfaces, terminology candidates)

Reports here are **inputs** to spec recovery, not specs themselves. They are
free to be messy and iterative; RFCs are the formalized output.

## Relationship to Other Platonic Directories

```
Codebase (existing source)
        |
        v
Analysis Report (this directory)        <-- structured scan findings
        |
        v
RFC Specification (.platonic/specs/)     <-- formalized specs
        |
        v
Implementation Guide (.platonic/impl/)   <-- concrete design
        |
        v
Code
```

Design drafts (exploratory design documents produced in BRAINSTORM mode before
an RFC exists) live in `.platonic/drafts/`, not here. This directory is for
**analysis of existing code**, not forward design exploration.

## File Naming

Analysis reports follow the convention:

```
YYYY-MM-DD-<topic>-analysis.md
```

- `YYYY-MM-DD`: date the scan was run
- `<topic>`: kebab-case descriptor of scope (e.g., `full-codebase`,
  `auth-subsystem`, `message-queue`)

**Examples**:

- `2026-11-11-full-codebase-analysis.md` — initial recovery scan of the whole project
- `2026-11-12-auth-subsystem-analysis.md` — focused re-scan of the auth subsystem

When a scan is re-run for the same scope, either overwrite the existing report
or append a new dated file — prefer overwriting when the report is purely
derived from code (no hand edits), and appending when the user has annotated it.

## Creating a New Analysis Report

Use the **platonic-coding** skill to scan an existing codebase:

```
Use platonic-coding to scan this codebase for spec recovery.
```

Or invoke the scan operation directly:

```
Use platonic-coding init-scan to analyze the project.
```

The scan writes its synthesis report to `.platonic/analysis/` by default, then
feeds it into `init-plan-modular-specs` to propose an RFC dependency graph.

## After an Analysis Report Is Complete

Once the scan synthesis is validated by the user, continue the recovery flow:

```
Use platonic-coding init-plan-modular-specs to propose RFCs from the analysis.
```

This consumes the analysis report and produces a modular RFC plan, which is then
confirmed and used to generate Draft RFCs via `init-recover-conceptual`,
`init-recover-architecture`, and (optionally) `init-recover-impl-interface`.
