# Platonic Coding Skills

A professional collection of Agent Skills for AI-powered [Platonic Coding](https://www.xiaming.site/2026/02/06/platonic-coding/) workflow.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills Format](https://img.shields.io/badge/format-Agent%20Skills%201.0-blue)](https://agentskills.io)

## Overview

**Platonic Coding** is a coding style designed for complex projects and cross-team collaboration with AI agents. Instead of relying on prompts, vibes, or implicit assumptions, it treats specifications as abstract laws that define what can exist, what can change, and what must always hold. Agents operate inside a shared, closed spec world where meaning is explicit, violations are detectable, and evolution is traceable over time, making large systems reproducible, reviewable, and stable across teams, agents, and long development cycles.

![manifesto-infographic](./manifesto.png)

See a full description in [Manifesto](https://github.com/caesar0301/platonic-coding/blob/main/MANIFESTO.md).

## 🔗 Combine with Superpowers for Maximum Impact

**Platonic Coding + [Superpowers](https://github.com/obra/superpowers) = Ultimate Spec-Driven Development**

For the most powerful spec-driven development experience, install both skill collections:

- **Platonic Coding**: Orchestrates the full specification-to-code lifecycle with intelligent auto-detection
- **Superpowers Brainstorming**: Provides structured design exploration with multiple approaches and trade-off analysis

**How they work together**: When you run Phase 0 of the Platonic Coding workflow, it automatically invokes the Superpower Brainstorming skill (if installed) to enhance your design process with:
- Structured requirements exploration
- 2-3 design approaches with trade-off analysis
- Incremental design validation
- Built-in spec review loops

If Superpowers isn't installed, Platonic Coding seamlessly falls back to its bundled interactive design method.

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
Init → Bootstrap infrastructure (new or recover from existing code)

Phase 0 → Conceptual Design (invoke brainstorming skill if available)
Phase 1 → RFC Specification (formalize requirements)
Phase 2 → Implementation (guide + code + tests)
Phase 3 → Spec Compliance Review (verify code against RFCs)
```

### Auto-Detection

The skill automatically detects your project state:

- **No `.platonic.yml`?** → Initialize (scaffold or recover)
- **Has specs but no RFCs?** → Run Phase 0-1
- **Has RFCs but no code?** → Run Phase 2
- **Has both specs and code?** → Run review

Override with flags: `--init`, `--workflow`, `--phase N`, `--review`

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
Phase 0: Design (uses brainstorming skill if installed) → Phase 1: RFC → Phase 2: Code + tests → Phase 3: Review.

### Implement Specific RFC
```
Use platonic-coding --impl-full for RFC-0001-user-authentication (Authentication).
```
Creates impl guide (with confirmation) → generates coding plan (with confirmation) → writes code + tests.

### Review Code Against Specs
```
Use platonic-coding to review src/auth/ against RFC-0001-user-authentication.md.
```
Generates compliance report: implemented ✅, missing ❌, inconsistent ⚠️.

## Acknowledgments

This project integrates with the [Superpowers](https://github.com/obra/superpowers) skill collection by [Will Barton](https://github.com/obra). The **Brainstorming** skill enhances Phase 0 design exploration with structured requirements gathering, multiple approach comparison, and incremental validation—making Platonic Coding even more powerful for spec-driven development.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**Xiaming Chen**
- Website: [https://xiaming.site/](https://xiaming.site/)
- GitHub: [@caesar0301](https://github.com/caesar0301)

---

*Built with ❤️ following the [Agent Skills](https://agentskills.io) standard*
