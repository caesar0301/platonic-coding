# Platonic Coding Skills

A professional collection of Agent Skills for AI-powered [Platonic Coding](https://www.xiaming.site/2026/02/06/platonic-coding/) workflow.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills Format](https://img.shields.io/badge/format-Agent%20Skills%201.0-blue)](https://agentskills.io)

## Overview

**Platonic Coding** is a coding style designed for complex projects and cross-team collaboration with AI agents. Instead of relying on prompts, vibes, or implicit assumptions, it treats specifications as abstract laws that define what can exist, what can change, and what must always hold. Agents operate inside a shared, closed spec world where meaning is explicit, violations are detectable, and evolution is traceable over time, making large systems reproducible, reviewable, and stable across teams, agents, and long development cycles.

![manifesto-infographic](./manifesto.png)

See a full description in [Manifesto](https://github.com/caesar0301/platonic-coding/blob/main/MANIFESTO.md).


## Installation

### Method 1: Claude Code Marketplace (Easiest)

If using Claude Code CLI with marketplace support:

```bash
claude-code marketplace add caesar0301/platonic-coding
```

### Method 2: Install using npx skills (Recommended for Most)

```bash
npx skills add caesar0301/platonic-coding
```

### Method 3: ClawHub

- 📦 ClawHub: `clawhub install platonic-coding`
- 📦 ClawHub: `clawhub install platonic-brainstorming`

## Complementary Skills

**Platonic Coding + Platonic Brainstorming = Stronger Design Process**

For the best in-repo spec-driven development experience, use both skills together:

- **Platonic Coding**: The canonical orchestrator for the full RFC → implementation → review lifecycle
- **Platonic Brainstorming**: An optional design companion for structured exploration before RFC formalization (Phase 1) and design refinement before implementation (Phase 2)

**How they work together**:
- **Phase 1 (RFC Specification)**: `platonic-brainstorming` helps explore requirements, compare approaches, and validate design before RFC formalization
- **Phase 2 (Implementation)**: `platonic-brainstorming` helps refine architecture decisions and validate implementation approach against RFC constraints

This adds structured exploration, multiple approaches with trade-off analysis, and incremental validation at key decision points.

If `platonic-brainstorming` is not available or you prefer lightweight interaction, Platonic Coding uses bundled interactive methods for design creation and refinement.

## Available Skills

All skills follow the [Agent Skills specification](https://agentskills.io/specification) for maximum compatibility across AI coding agents.

| Skill | Purpose | Docs |
|-------|---------|------|
| 🎯 **platonic-coding** | Intelligent orchestrator for the complete Platonic Coding workflow. Auto-detects project state and routes to the right next step: initialize a project, run recovery for existing code, formalize drafts into RFCs, refine specs, implement from guides with tests, or review code compliance. | [SKILL.md](skills/platonic-coding/SKILL.md) |
| 🧠 **platonic-brainstorming** | Optional design companion for Platonic Coding Phases 1 and 2. Guides collaborative exploration, compares approaches, validates designs, and integrates seamlessly with RFC specification and implementation workflows. | [SKILL.md](skills/platonic-brainstorming/SKILL.md) |

## General Workflow

Platonic Coding follows a **three-phase workflow** (after initialization) with intelligent auto-detection:

```
Init    → Bootstrap infrastructure (new project or recovery flow for existing code)
Phase 1 → RFC Specification (optional brainstorming → design draft → RFC → specs-refine)
Phase 2 → Implementation (optional brainstorming → impl-full: guide + code + tests)
Phase 3 → Spec Compliance Review (verify code against RFCs and guides)
```

Each phase optionally integrates `platonic-brainstorming` for structured design exploration and validation.

### Auto-Detection

The skill automatically detects your project state and suggests the next step:

- **No `.platonic.yml`?** → Initialize (`init-scaffold`) or start the recovery flow (`init-scan` → recovery operations)
- **Has design drafts but no RFCs?** → Run Phase 1 (RFC Specification)
- **Has RFCs but no implementation guides?** → Run Phase 2 (Implementation)
- **Has RFCs and implementation guides?** → Resume Phase 2 implementation or run Phase 3 review, depending on completeness
- **State is ambiguous?** → Resume the current phase or ask whether you want refine / implement / review

Prefer canonical operation names when overriding auto-detection: `init-scaffold`, `init-scan`, `specs-refine`, `impl-full`, `review`, `workflow --phase <N>`

## Examples

### Initialize New Project
```
Use platonic-coding to set up my new project "Acme" (TypeScript/Next.js).
```
Auto-detects missing infrastructure → scaffolds `.platonic.yml`, directories, templates.

### Recover Specs from Existing Code
```
Use platonic-coding to recover design specs for this codebase.
```
Auto-detects existing code → scans → proposes RFC dependency graph → generates Draft RFCs.

### Run Full Workflow (Phase 1-3)
```
Use platonic-coding workflow to implement user preferences.
```
Phase 1: RFC Specification (optional `platonic-brainstorming` for conceptual design → RFC → specs-refine) → Phase 2: Implementation (optional brainstorming → impl-full) → Phase 3: Review.

### Implement Specific RFC
```
Use platonic-coding impl-full for RFC-0001-user-authentication (Authentication).
```
Creates impl guide (with confirmation) → generates coding plan (with confirmation) → writes code + tests.

### Review Code Against Specs
```
Use platonic-coding to review src/auth/ against RFC-0001-user-authentication.md.
```
Generates compliance report: implemented ✅, missing ❌, inconsistent ⚠️.

## Acknowledgments

`platonic-brainstorming` is adapted from the upstream [Superpowers](https://github.com/obra/superpowers) skill collection by [Will Barton](https://github.com/obra). It enhances design exploration in Platonic Coding Phases 1 and 2 with structured requirements gathering, multiple approach comparison, and incremental validation at key decision points.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**Xiaming Chen**
- Website: [https://xiaming.site/](https://xiaming.site/)
- GitHub: [@caesar0301](https://github.com/caesar0301)

---

*Built with ❤️ following the [Agent Skills](https://agentskills.io) standard*
