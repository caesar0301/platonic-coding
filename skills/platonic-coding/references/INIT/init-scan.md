# Scan Project

Systematically analyze an existing codebase to understand its structure, types, interfaces, and design patterns.

## Objective

Produce structured scan results that feed into the spec recovery operations. The scan is thorough but focused — gather enough to write meaningful specs without reading every line of code.

## Inputs

| Input | Required | Source |
|-------|----------|--------|
| Project root | Yes | `.platonic.yml` or user-provided |
| Language | Yes | `.platonic.yml` or auto-detected |
| Framework | No | `.platonic.yml` or auto-detected |

## Scan Phases

Execute all 5 phases in order. Each phase builds on the previous.

---

### Phase 1: Project Discovery

**Goal**: Understand what this project is, at the highest level.

**Actions**:

1. Read the **README** (README.md, README.rst, README.txt) — extract:
   - Project purpose and goals
   - Feature descriptions
   - Architecture overview (if present)
   - Getting started / usage patterns

2. Read **build/config files** to identify:
   - Language and version
   - Framework and version
   - Dependencies (direct and dev)
   - Build targets / entry points
   - Monorepo structure (workspaces, packages)

3. Read **existing documentation** (docs/, doc/, wiki/):
   - Architecture diagrams or descriptions
   - API documentation
   - Design decisions or ADRs

4. Map **top-level directory structure**:
   - Source directories
   - Test directories
   - Config directories
   - Documentation directories

**Build file patterns by language**:

| Language | Build Files |
|----------|-------------|
| Rust | `Cargo.toml`, workspace `Cargo.toml` |
| TypeScript/JS | `package.json`, `tsconfig.json`, monorepo root |
| Python | `pyproject.toml`, `setup.py`, `setup.cfg`, `requirements.txt` |
| Go | `go.mod`, `go.sum` |
| Java | `pom.xml`, `build.gradle`, `build.gradle.kts` |
| C# | `*.csproj`, `*.sln` |

**Output**: Project metadata record (name, language, framework, dependencies, entry points, existing docs summary).

---

### Phase 2: Structural Analysis

**Goal**: Map the module/package/crate boundaries and dependency graph.

**Actions**:

1. **Identify modules**: List all top-level source modules/packages/crates
   - Rust: crates in workspace or `src/` modules
   - TypeScript: packages in monorepo or `src/` directories
   - Python: packages (directories with `__init__.py`) or modules
   - Go: packages under `cmd/`, `internal/`, `pkg/`
   - Java: packages under `src/main/java/`

2. **Build dependency graph**: For each module, identify which other modules it imports
   - Rust: `Cargo.toml` dependencies between workspace crates, `use` statements
   - TypeScript: `import` statements, `package.json` workspace dependencies
   - Python: `import` statements, `setup.py` / `pyproject.toml` dependencies
   - Go: `import` statements
   - Java: `import` statements, Maven/Gradle dependencies

3. **Detect layers**: From the dependency graph, identify:
   - Foundation modules (depended on by many, depend on few)
   - Middle-tier modules (both depend and are depended on)
   - Leaf modules (depend on many, depended on by few)
   - Utility modules (depended on by many, small scope)

4. **Detect configuration patterns**:
   - Environment variables
   - Config files (YAML, TOML, JSON)
   - Feature flags or build-time configuration

**Output**: Module list with dependency graph, layer classification, config patterns.

---

### Phase 3: Type and Interface Analysis

**Goal**: Catalog the public types and interfaces that define the system's API surface.

**Actions**:

1. **Catalog public types** for each module:
   - Structs/classes with public fields
   - Enums/unions/discriminated types
   - Type aliases
   - Constants and configuration types
   - Focus on types that cross module boundaries (used by other modules)

2. **Catalog public interfaces** for each module:
   - Traits/interfaces/abstract classes
   - Exported function signatures
   - Public methods on key types
   - Event/message types
   - Focus on interfaces used across module boundaries

3. **Identify shared types**: Types that appear in multiple modules' APIs
   - These often represent core domain concepts
   - They signal architectural boundaries

4. **Extract naming conventions**:
   - Common prefixes/suffixes (e.g., `*Service`, `*Repository`, `*Handler`)
   - Method naming patterns (e.g., `get_*`, `create_*`, `on_*`, `handle_*`)
   - Module naming patterns

**Language-specific signals**:

| Language | Public types | Public interfaces |
|----------|-------------|-------------------|
| Rust | `pub struct`, `pub enum`, `pub type` | `pub trait`, `pub fn`, `pub async fn` |
| TypeScript | `export interface`, `export type`, `export class` | `export function`, `export abstract class` |
| Python | Classes in `__init__.py` or `__all__` | ABC subclasses, Protocol classes, public functions |
| Go | Capitalized types | Capitalized functions, interface types |
| Java | `public class`, `public enum` | `public interface`, `public abstract class` |

**Output**: Type catalog, interface catalog, shared types list, naming conventions.

---

### Phase 4: Behavioral Analysis

**Goal**: Understand how data flows through the system and what invariants are enforced.

**Actions**:

1. **Trace data flow**: Starting from entry points (main, API handlers, CLI commands), follow how data moves:
   - What gets created/received at the entry?
   - What transformations happen?
   - Where does data end up (storage, network, output)?
   - What are the main pipelines/flows?

2. **Extract invariants**: Look for enforced rules:
   - Assertions and debug assertions
   - Validation at module boundaries
   - Error types that prevent invalid states
   - Immutability patterns (const, readonly, final)
   - Access control patterns (pub/private boundaries)

3. **Identify design patterns**:
   - Repository pattern (data access abstraction)
   - Factory pattern (object creation)
   - Observer/event pattern (publish/subscribe)
   - Pipeline/chain pattern (data transformation)
   - Strategy pattern (pluggable algorithms)
   - Middleware pattern (request/response processing)

4. **Note error handling strategies**:
   - Custom error types and hierarchies
   - Error propagation patterns (Result, try/catch, Either)
   - Recovery strategies (retry, fallback, graceful degradation)

**Output**: Data flow descriptions, invariant list, design pattern catalog, error handling summary.

---

### Phase 5: Synthesis

**Goal**: Organize all findings into a coherent picture and prepare for spec recovery.

**Actions**:

1. **Group findings into conceptual clusters**: What is this system "about"?
   - Core domain concepts (from shared types and naming)
   - System principles (from invariants and patterns)
   - Design philosophy (from architectural choices)

2. **Identify subsystem boundaries**: Where are the natural divisions?
   - Modules with strong internal cohesion and loose external coupling
   - Clear dependency direction between groups
   - Distinct data flow paths

3. **Identify API boundary surfaces**: Where do subsystems interact?
   - Cross-module interfaces and shared types
   - Event/message contracts
   - Configuration and integration points

4. **Prepare scan summary**: A concise document with:
   - System overview (1-2 paragraphs from README + discoveries)
   - Subsystem list with responsibilities
   - Key design patterns and principles
   - Main data flows
   - Core terminology

5. **Present to user**: Show the synthesis and ask:
   - "Does this accurately represent your system?"
   - "Are there subsystem boundaries I missed or got wrong?"
   - "Any important concepts or principles I should capture?"

**Output**: Scan synthesis document (in-memory) ready for spec planning.

## Scan Depth Guidelines

- **Focus on public API surfaces**, not implementation details
- **Read module entry points** (lib.rs, index.ts, __init__.py, package-level files)
- **Skim implementation files** for patterns, don't read every line
- **Prioritize cross-module interactions** over internal module logic
- **Time-box**: Aim for thorough but not exhaustive — the specs are Draft and will be refined

## Notes

- The scan produces in-memory results, not written files
- All findings feed into the plan-modular-specs and recover-* operations
- If the project is very large, focus on the most architecturally significant modules
- Always present the synthesis to the user for validation before proceeding
