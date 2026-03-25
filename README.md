# Platonic Coding Skills

A professional collection of Agent Skills for AI-powered [Platonic Coding](https://www.xiaming.site/2026/02/06/platonic-coding/) workflow.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills Format](https://img.shields.io/badge/format-Agent%20Skills%201.0-blue)](https://agentskills.io)

## Overview

**Platonic Coding** is a coding style designed for complex projects and cross-team collaboration with AI agents. Instead of relying on prompts, vibes, or implicit assumptions, it treats specifications as abstract laws that define what can exist, what can change, and what must always hold. Agents operate inside a shared, closed spec world where meaning is explicit, violations are detectable, and evolution is traceable over time, making large systems reproducible, reviewable, and stable across teams, agents, and long development cycles.

![manifesto-infographic](./manifesto.png)

See a full description in [Manifesto](https://github.com/caesar0301/platonic-coding-skills/blob/main/MANIFESTO.md).

## Available Skill

All skills follow the [Agent Skills specification](https://agentskills.io/specification) for maximum compatibility across AI coding agents.

| Skill | Purpose | Docs |
|-------|---------|------|
| 🎯 **platonic-coding** | Intelligent orchestrator for the complete Platonic Coding workflow. Auto-detects project state and runs appropriate phases—init for new projects, recover specs from existing code, refine RFCs, implement from specs with guides and tests, or review code compliance. Single entry point for specification-driven development. | [SKILL.md](skills/platonic-coding/SKILL.md) |

## Installation

### Method 1: Claude Code CLI Marketplace (Easiest)

If using Claude Code CLI with marketplace support:

```bash
# Add the skills marketplace
claude-code marketplace add caesar0301/platonic-coding-skills
```

### Method 2: Install using npx skills (Recommended for Most)

```bash
npx skills add caesar0301/platonic-coding-skills
```

### Method 3: Clone to Skills Directory

Clone this repository to your agent's skills directory:

```bash
git clone https://github.com/caesar0301/platonic-coding-skills.git ~/.claude/skills/platonic-coding-skills
```

The unified `platonic-coding` skill provides all functionality in a single, intelligent orchestrator.

## General Workflow

Platonic Coding follows a **four-phase, closed-world workflow** with an initialization step. The unified `platonic-coding` skill intelligently orchestrates this workflow based on project state.

```
┌──────────────────────────────────────────────────────────────┐
│ Init: Project Bootstrap (INIT mode)                          │
│                                                              │
│  • Set up .platonic.yml, docs/specs/, docs/impl/, docs/drafts/
│  • For existing codebases: scan, plan, and recover           │
│    design specs as Draft RFCs                                │
│                                                              │
│  Output: Platonic Coding infrastructure (+ recovered RFCs)   │
│  Mode:   INIT (init-scaffold or init-recover)                │
└───────────────┬──────────────────────────────────────────────┘
                │
                ▼
┌──────────────────────────────────────────────────────────────┐
│ Phase 0: Conceptual Design & Requirements Elicitation        │
│                                                              │
│  • Clarify problem space, goals, constraints, and invariants │
│  • Explore domain knowledge and prior art                    │
│  • Identify core abstractions and system boundaries          │
│  • Resolve ambiguity before formalization                    │
│                                                              │
│  Output: Shared mental model, requirement notes, concepts    │
│  Mode:   WORKFLOW (Phase 0)                                  │
└───────────────┬──────────────────────────────────────────────┘
                │
                ▼
┌──────────────────────────────────────────────────────────────┐
│ Phase 1: Design Specifications (RFC World Construction)      │
│                                                              │
│  • Formalize requirements as RFCs                            │
│  • Define entities, relations, invariants, and constraints   │
│  • Establish terminology, taxonomy, and evolution rules      │
│  • Create a closed, legally-defined specification space      │
│                                                              │
│  Output: RFCs, index, history, terminology                   │
│  Mode:   WORKFLOW (Phase 1) → calls SPECS mode (specs-refine)│
└───────────────┬──────────────────────────────────────────────┘
                │
                ▼
┌──────────────────────────────────────────────────────────────┐
│ Phase 2: Implementation (Guide + Code)                       │
│                                                              │
│  • Translate RFCs into implementation-ready architecture     │
│  • Design impl guide with types, interfaces, module layout   │
│  • Generate coding plan with file-level task breakdown       │
│  • Write code strictly following guide and RFCs              │
│  • Include unit and integration tests                        │
│  • User confirmation gates after guide and coding plan       │
│                                                              │
│  Output: Implementation guide + source code + tests          │
│  Mode:   WORKFLOW (Phase 2) → calls IMPL mode (impl-full)    │
└───────────────┬──────────────────────────────────────────────┘
                │
                ▼
┌──────────────────────────────────────────────────────────────┐
│ Phase 3: Spec Compliance Review (Reality Check)              │
│                                                              │
│  • Verify code against RFCs and implementation guides        │
│  • Detect gaps, drift, and contradictions                    │
│  • Identify specs without code and code without specs        │
│  • Produce traceable compliance reports                      │
│                                                              │
│  Output: Review & compliance reports                         │
│  Mode:   WORKFLOW (Phase 3) → calls REVIEW mode              │
└──────────────────────────────────────────────────────────────┘
```

### Intelligent Auto-Detection

The skill automatically detects your project state and suggests appropriate actions:

- **No `.platonic.yml`?** → INIT mode (scaffold new or recover from existing code)
- **Has specs but no RFCs?** → WORKFLOW Phase 0-1 (design and generate RFCs)
- **Has RFCs but no implementation?** → WORKFLOW Phase 2 (implement specs)
- **Has both specs and code?** → REVIEW mode (check compliance)

You can also explicitly specify operations with flags like `--init`, `--specs-refine`, `--impl-full`, `--review`, or `--workflow`.

## Examples

The unified skill auto-detects project state and suggests appropriate actions. You can also explicitly specify operations.

### Example 1: Initialize a new project (auto-detected)

```
Use platonic-coding to set up my new project "Acme".
Language is TypeScript, framework is Next.js. Specs go in docs/specs/.
```

**Result:** Agent auto-detects no `.platonic.yml` → runs INIT mode → scaffolds infrastructure.

### Example 2: Adopt Platonic Coding for existing codebase (auto-detected)

```
Use platonic-coding to recover design specs for this existing project.
Scan the codebase and propose what RFCs to generate.
```

**Result:** Agent auto-detects no `.platonic.yml` + has code → runs INIT mode → init-recover → scans codebase, proposes modular RFC dependency graph, and (after user confirmation) generates Draft RFCs.

### Example 3: Run full workflow (auto-detected or explicit)

```
Use platonic-coding workflow to implement a "user preferences" feature.
Start at Phase 0: we need stored settings, sync with backend, and UI in settings page.
```

**Result:** Agent runs full 4-phase workflow: Phase 0 (interactive design) → Phase 1 (RFC + specs-refine) → Phase 2 (impl-full with confirmation gates) → Phase 3 (review) → FINISHED with summary.

### Example 4: Specific operation (explicit)

```
Use platonic-coding --specs-refine to validate all specifications.
```

**Result:** Agent runs specs-refine operation: validates consistency, checks taxonomy, generates history/index/namings.

### Example 5: Full implementation from spec (explicit)

```
Use platonic-coding --impl-full for RFC-0001 (Authentication)
targeting the auth module. Use TypeScript and the existing Express patterns.
```

**Result:** Agent runs impl-full: analyzes spec → creates impl guide (confirmation gate) → generates coding plan (confirmation gate) → implements code with unit and integration tests.

### Example 6: Review code against spec (auto-detected or explicit)

```
Use platonic-coding to review src/auth/ against RFC-0001.md.
```

**Result:** Agent auto-detects both specs and code → runs REVIEW mode → generates compliance report showing what's implemented, missing, and inconsistent.

### Example 7: Gap analysis (explicit)

```
Use platonic-coding --review to identify gaps between
all RFCs in docs/specs/ and the implementation in src/.
```

**Result:** Bi-directional analysis of unimplemented specs and undocumented code.

### Example 8: Start at specific phase (explicit)

```
Use platonic-coding workflow --phase 2 to implement RFC-0042.
```

**Result:** Agent starts WORKFLOW at Phase 2 → runs impl-full for RFC-0042.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**Xiaming Chen**
- Website: [https://xiaming.site/](https://xiaming.site/)
- GitHub: [@caesar0301](https://github.com/caesar0301)

---

*Built with ❤️ following the [Agent Skills](https://agentskills.io) standard*
