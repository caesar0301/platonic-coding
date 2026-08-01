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

Inside a Claude Code session ([docs](https://code.claude.com/docs/en/discover-plugins)):

```text
/plugin marketplace add caesar0301/platonic-coding
/plugin install platonic-coding@platonic-coding
/reload-plugins
```

Or from the shell (non-interactive; user scope by default):

```bash
claude plugin marketplace add caesar0301/platonic-coding
claude plugin install platonic-coding@platonic-coding
```

### Method 2: Install using npx skills (Recommended for Most)

```bash
npx skills add caesar0301/platonic-coding
```

### Method 3: ClawHub

- 📦 ClawHub: `clawhub install platonic-coding`

## Built-in Design Exploration (BRAINSTORM Mode)

**Platonic Coding now includes structured design exploration as a built-in mode** — no separate skill required. The former `platonic-brainstorming` skill has been merged into `platonic-coding` as **BRAINSTORM mode**.

- **Phase 1 (RFC Specification)**: BRAINSTORM mode helps explore requirements, compare approaches, and validate design before RFC formalization
- **Phase 2 (Implementation)**: BRAINSTORM mode helps refine architecture decisions and validate implementation approach against RFC constraints

It activates automatically when your request signals exploration (e.g., "brainstorm", "discuss", "investigate", "deep analysis"), or when you invoke it explicitly. After a design draft is approved, the skill presents alternative paths to continue: pause at each phase gate, quick-pass multiple phases for a fast fix, or update vs. create an RFC/IG.

## Available Skills

All skills follow the [Agent Skills specification](https://agentskills.io/specification) for maximum compatibility across AI coding agents.

| Skill | Purpose | Docs |
|-------|---------|------|
| 🎯 **platonic-coding** | Intelligent orchestrator for the complete Platonic Coding workflow. Auto-detects intent and project state, then routes to the right next step: brainstorm a design, initialize a project, run recovery for existing code, formalize drafts into RFCs, refine specs, implement from guides with tests, or review code compliance. | [SKILL.md](skills/platonic-coding/SKILL.md) |

## General Workflow

Platonic Coding follows a **three-phase workflow** (after initialization) with intelligent auto-detection:

```
Init    → Bootstrap infrastructure (new project or recovery flow for existing code)
Phase 1 → RFC Specification (optional BRAINSTORM mode → design draft → RFC → specs-refine)
Phase 2 → Implementation (optional BRAINSTORM mode → impl-full: guide + code + tests)
Phase 3 → Spec Compliance Review (verify code against RFCs and guides)
```

Each phase optionally enters BRAINSTORM mode for structured design exploration and validation.

### Auto-Detection

The skill automatically detects your **intent** and **project state**, then suggests the next step:

- **Request mentions brainstorm / discuss / investigate / explore / deep analysis?** → Enter BRAINSTORM mode (design exploration first), then hand off to Phase 1 or 2
- **No `.platonic.yml`?** → Initialize (`init-scaffold`) or start the recovery flow (`init-scan` → recovery operations)
- **Has design drafts but no RFCs?** → Run Phase 1 (RFC Specification)
- **Has RFCs but no implementation guides?** → Run Phase 2 (Implementation)
- **Has RFCs and implementation guides?** → Resume Phase 2 implementation or run Phase 3 review, depending on completeness
- **State is ambiguous?** → Resume the current phase or ask whether you want refine / implement / review

Prefer canonical operation names when overriding auto-detection: `brainstorm`, `init-scaffold`, `init-scan`, `specs-refine`, `impl-full`, `review`, `workflow --phase <N>`

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

### Brainstorm a Design
```
Use platonic-coding to brainstorm the message queue design.
```
Intent keyword detected → enters BRAINSTORM mode → collaborative dialogue → approved design draft → presents post-draft routing (pause / quick-pass / update or create RFC / update or create IG).

### Run Full Workflow (Phase 1-3)
```
Use platonic-coding workflow to implement user preferences.
```
Phase 1: RFC Specification (optional BRAINSTORM mode for conceptual design → RFC → specs-refine) → Phase 2: Implementation (optional BRAINSTORM mode → impl-full) → Phase 3: Review.

### Implement Specific RFC
```
Use platonic-coding impl-full for RFC-001-user-authentication (Authentication).
```
Creates impl guide (with confirmation) → generates coding plan (with confirmation) → writes code + tests.

### Review Code Against Specs
```
Use platonic-coding to review src/auth/ against RFC-001-user-authentication.md.
```
Generates compliance report: implemented ✅, missing ❌, inconsistent ⚠️.

## Acknowledgments

BRAINSTORM mode (the built-in design exploration capability, formerly the standalone `platonic-brainstorming` skill) is adapted from the upstream [Superpowers](https://github.com/obra/superpowers) skill collection by [Will Barton](https://github.com/obra). It enhances design exploration in Platonic Coding Phases 1 and 2 with structured requirements gathering, multiple approach comparison, and incremental validation at key decision points.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**Xiaming Chen**
- Website: [https://xiaming.site/](https://xiaming.site/)
- GitHub: [@caesar0301](https://github.com/caesar0301)

---

*Built with ❤️ following the [Agent Skills](https://agentskills.io) standard*
