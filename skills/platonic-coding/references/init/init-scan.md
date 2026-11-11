# Scan Project

Systematically analyze an existing codebase for spec recovery.

## Inputs

| Input | Source |
|-------|--------|
| Project root | User-provided or current directory |
| Language | Auto-detected from build files (or `.platonic.yml` if present) |
| Framework | Auto-detected from dependencies (or `.platonic.yml` if present) |
| Specs path | `.platonic.yml` if present, else default `.platonic/specs` |
| Impl path | `.platonic.yml` if present, else default `.platonic/impl` |
| Drafts path | `.platonic.yml` if present, else default `.platonic/drafts` |
| Analysis path | `.platonic.yml` if present, else default `.platonic/analysis` |

**Note**: `.platonic.yml` is optional. All inputs auto-detect or use defaults when it is absent.

## Scan Phases (5 phases, in order)

### Phase 1: Project Discovery
Read README, build files, existing docs. Extract: purpose, language, framework, dependencies, entry points, directory structure.

**Build files**: `Cargo.toml` (Rust), `package.json` (JS/TS), `pyproject.toml` (Python), `go.mod` (Go), `pom.xml`/`build.gradle` (Java).

**Output**: Project metadata.

### Phase 2: Structural Analysis
Identify modules, build dependency graph, detect layers (foundation/middle/leaf), find config patterns.

**Output**: Module list with dependency graph, layer classification.

### Phase 3: Type and Interface Analysis
Catalog public types (structs, enums, type aliases) and interfaces (traits, exported functions, public methods). Focus on cross-module boundaries. Extract naming conventions.

**Output**: Type catalog, interface catalog, shared types, naming conventions.

### Phase 4: Behavioral Analysis
Trace data flow from entry points. Extract invariants (assertions, validation, immutability). Identify design patterns (repository, factory, observer, pipeline, strategy, middleware). Note error handling.

**Output**: Data flows, invariants, patterns, error handling.

### Phase 5: Synthesis
Group findings into conceptual clusters. Identify subsystem boundaries and API surfaces. Prepare summary: system overview, subsystems, patterns, data flows, terminology. **Present to user for validation**, then write the synthesis to the analysis report at `<analysis-path>/YYYY-MM-DD-<topic>-analysis.md` (see Output below).

Namespace inputs: flag whether the project shows ≥ 3 independent services (distinct manifests/deployment units) or enough expected specs to warrant namespaced mode — `init-plan-modular-specs` uses this to decide and to draft namespace/prefix candidates.

## Scan Guidelines

- Focus on public API surfaces, not implementation details
- Read module entry points (lib.rs, index.ts, __init__.py)
- Skim implementation files for patterns
- Prioritize cross-module interactions
- Time-box: thorough but not exhaustive

## Output

Persist the scan synthesis as a structured analysis report so downstream operations (`init-plan-modular-specs`, `init-recover-*`) and the user can reference it without re-scanning:

1. **Write the analysis report** to `<analysis-path>/YYYY-MM-DD-<topic>-analysis.md` (default `.platonic/analysis/YYYY-MM-DD-<topic>-analysis.md`). Use today's date and a short kebab-case topic derived from the project name or primary subsystem (e.g., `2026-11-11-acme-codebase-analysis.md`). Create the `<analysis-path>/` directory if it does not already exist.
2. **Report structure** — organize the synthesis into these sections:
   - **Project Metadata**: name, language, framework, dependencies, entry points, directory structure (from Phase 1)
   - **Structural Analysis**: module list with dependency graph, layer classification (from Phase 2)
   - **Type & Interface Analysis**: type catalog, interface catalog, shared types, naming conventions (from Phase 3)
   - **Behavioral Analysis**: data flows, invariants, patterns, error handling (from Phase 4)
   - **Synthesis**: system overview, subsystem boundaries, API surfaces, terminology, namespace candidates (from Phase 5)
3. **Present to user for validation** (Phase 5 already does this interactively); after the user confirms or adjusts, write the final report.

The report is an **input** to spec recovery, not a spec itself. It lives under `.platonic/analysis/` alongside design drafts (`.platonic/drafts/`) — analysis captures the existing code, drafts explore forward designs.

Downstream operations read this persisted report instead of re-running the scan.
