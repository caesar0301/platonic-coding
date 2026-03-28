# Platonic Coding Skills

A professional collection of Agent Skills for AI-powered [Platonic Coding](https://www.xiaming.site/2026/02/06/platonic-coding/) workflow.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills Format](https://img.shields.io/badge/format-Agent%20Skills%201.0-blue)](https://agentskills.io)

## Overview

**Platonic Coding** is a coding style designed for complex projects and cross-team collaboration with AI agents. Instead of relying on prompts, vibes, or implicit assumptions, it treats specifications as abstract laws that define what can exist, what can change, and what must always hold. Agents operate inside a shared, closed spec world where meaning is explicit, violations are detectable, and evolution is traceable over time, making large systems reproducible, reviewable, and stable across teams, agents, and long development cycles.

![manifesto-infographic](./manifesto.png)

See a full description in [Manifesto](https://github.com/caesar0301/platonic-coding/blob/main/MANIFESTO.md).

## Complementary Skills

**Platonic Coding + Platonic Brainstorming = Stronger Phase 0 Design**

For the best in-repo spec-driven development experience, use both skills together:

- **Platonic Coding**: The canonical orchestrator for the full design → RFC → implementation → review lifecycle
- **Platonic Brainstorming**: An optional Phase 0 companion for structured design exploration before RFC formalization

**How they work together**: During Phase 0, `platonic-coding` can invoke `platonic-brainstorming` when it is available and you want the structured design flow. That adds:
- Structured requirements exploration
- 2-3 design approaches with trade-off analysis
- Incremental design validation before final design approval
- A clean handoff into Phase 1 RFC formalization

If `platonic-brainstorming` is not available or you do not want the extra structure, Platonic Coding falls back to its bundled interactive design method.

## Available Skills

All skills follow the [Agent Skills specification](https://agentskills.io/specification) for maximum compatibility across AI coding agents.

| Skill | Purpose | Docs |
|-------|---------|------|
| 🎯 **platonic-coding** | Intelligent orchestrator for the complete Platonic Coding workflow. Auto-detects project state and routes to the right next step: initialize a project, run recovery for existing code, formalize drafts into RFCs, refine specs, implement from guides with tests, or review code compliance. | [SKILL.md](skills/platonic-coding/SKILL.md) |
| 🧠 **platonic-brainstorming** | Optional Phase 0 design companion for Platonic Coding. Guides collaborative exploration, compares approaches, validates the design with the user, and hands off to Phase 1 RFC formalization. | [SKILL.md](skills/platonic-brainstorming/SKILL.md) |

## Installation

### Method 1: Claude Code CLI Marketplace (Easiest)

If using Claude Code CLI with marketplace support:

```bash
# Add the skills marketplace
claude-code marketplace add caesar0301/platonic-coding
```

### Method 2: Install using npx skills (Recommended for Most)

```bash
npx skills add caesar0301/platonic-coding
```

### Method 3: Clone to Skills Directory

Clone this repository to your agent's skills directory:

```bash
git clone https://github.com/caesar0301/platonic-coding.git ~/.claude/skills/platonic-coding
```

The unified `platonic-coding` skill provides all functionality in a single, intelligent orchestrator.

## General Workflow

Platonic Coding follows a **four-phase workflow** with intelligent auto-detection:

```
Init    → Bootstrap infrastructure (new project or recovery flow for existing code)
Phase 0 → Conceptual Design (optionally use `platonic-brainstorming`)
Phase 1 → RFC Specification (`design draft -> RFC -> specs-refine`)
Phase 2 → Implementation (guide + code + tests)
Phase 3 → Spec Compliance Review (verify code against RFCs and guides)
```

### Auto-Detection

The skill automatically detects your project state and suggests the next step:

- **No `.platonic.yml`?** → Initialize (`init-scaffold`) or start the recovery flow (`init-scan` → recovery operations)
- **Has design drafts but no RFCs?** → Run Phase 1
- **Has RFCs but no implementation guides?** → Run Phase 2
- **Has RFCs and implementation guides?** → Resume implementation or run review, depending on whether the target looks complete
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

### Run Full Workflow (Phase 0-3)
```
Use platonic-coding workflow to implement user preferences.
```
Phase 0: Design (uses `platonic-brainstorming` if available and desired) → Phase 1: RFC formalization + `specs-refine` → Phase 2: Code + tests → Phase 3: Review.

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

`platonic-brainstorming` is adapted from the upstream [Superpowers](https://github.com/obra/superpowers) skill collection by [Will Barton](https://github.com/obra). It enhances Phase 0 design exploration with structured requirements gathering, multiple approach comparison, and incremental validation while aligning the handoff to Platonic Coding Phase 1 RFC formalization.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**Xiaming Chen**
- Website: [https://xiaming.site/](https://xiaming.site/)
- GitHub: [@caesar0301](https://github.com/caesar0301)

---

*Built with ❤️ following the [Agent Skills](https://agentskills.io) standard*
