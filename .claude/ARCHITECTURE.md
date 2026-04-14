# Claude Code Kit Architecture

> Comprehensive AI Agent Capability Expansion Toolkit

---

## Overview

Claude Code Kit is a modular system consisting of:

- **11 Specialist Agents** - Role-based AI personas
- **31 Skills** - Domain-specific knowledge modules (includes 7 nested design skills)
- **3 Workflows** - Slash command procedures
- **20 Scripts** - 4 master + 16 skill-level validation tools

---

## Directory Structure

```plaintext
.claude/
├── ARCHITECTURE.md              # This file
├── settings.json                # Permissions, hooks, model defaults, env
├── settings.local.json          # Personal overrides (gitignore)
├── .mcp.json                    # MCP server configuration
├── agents/                      # 11 Specialist Agents
├── skills/                      # 31 Skills (+ 7 nested in skills/skills/)
├── commands/                    # 3 Slash Commands
├── rules/                       # Global Rules (CLAUDE.md)
├── hooks/                       # Event-driven Scripts
├── output-styles/               # Custom output formatting
├── agent-memory/                # Memory per agent (optional)
└── worktrees/                   # Auto-created worktrees (gitignore)
```

---

## Agents (11)

Specialist AI personas for different domains.

| Agent                   | Description                                                           |
|-------------------------|-----------------------------------------------------------------------|
| `orchestrator`          | Multi-agent coordination and task orchestration                       |
| `project-planner`       | Smart project planning, task breakdown, dependency graphs             |
| `frontend-specialist`   | React/Next.js, UI components, styling, state management               |
| `backend-specialist`    | Node.js, Python, API development, database integration                |
| `database-architect`    | Schema design, query optimization, migrations, data modeling          |
| `performance-optimizer` | Performance profiling, Core Web Vitals, bundle optimization           |
| `product-manager`       | Product requirements, user stories, acceptance criteria               |
| `product-owner`         | Strategic facilitation, roadmap management, backlog prioritization    |
| `documentation-writer`  | Technical documentation (only when explicitly requested)              |
| `seo-specialist`        | SEO, GEO, Core Web Vitals, E-E-A-T, AI search visibility              |
| `code-archaeologist`    | Legacy code analysis, refactoring, understanding undocumented systems |
| `explorer-agent`        | Codebase discovery, architectural analysis, proactive research        |

---

## Skills (31)

Modular knowledge domains that agents load on-demand based on task context.

### Core Skills

| Skill                     | Description                                                                            |
|---------------------------|----------------------------------------------------------------------------------------|
| `api-patterns`            | REST, GraphQL, tRPC, auth, rate limiting, versioning                                   |
| `architecture`            | System design patterns, context discovery, trade-off analysis                          |
| `brainstorming`           | Multi-perspective structured brainstorming with expert lenses                          |
| `clean-code`              | Coding standards (global)                                                              |
| `code-review-checklist`   | Code review standards                                                                  |
| `database-design`         | Schema design, indexing, migrations, optimization, ORM selection                       |
| `documentation-templates` | Documentation formats and structures                                                   |
| `frontend-design`         | UI/UX patterns, design systems, animation, color, typography, motion                   |
| `geo-fundamentals`        | Generative Engine Optimization (GEO)                                                   |
| `i18n-localization`       | Internationalization patterns                                                          |
| `intelligent-routing`     | Request routing and dispatch                                                           |
| `lint-and-validate`       | Linting, type checking, validation                                                     |
| `mobile-design`           | Mobile UI/UX, platform-specific (iOS/Android), testing, navigation                     |
| `nextjs-react-expert`     | Next.js/React performance (Vercel rules: async, bundle, server, client, render, cache) |
| `nodejs-best-practices`   | Node.js async patterns, modules, best practices                                        |
| `performance-profiling`   | Web Vitals, Lighthouse audits, optimization                                            |
| `plan-writing`            | Task planning and breakdown                                                            |
| `seo-fundamentals`        | SEO, E-E-A-T, Core Web Vitals                                                          |
| `tailwind-patterns`       | Tailwind CSS utilities and patterns                                                    |
| `tdd-workflow`            | Test-driven development                                                                |
| `testing-patterns`        | Jest, Vitest, testing strategies                                                       |
| `vulnerability-scanner`   | Security auditing, OWASP, checklists                                                   |
| `web-design-guidelines`   | Web UI audit - accessibility, UX, performance                                          |
| `webapp-testing`          | E2E testing with Playwright                                                            |

### Nested Design Skills (skills/skills/)

Specialized design taste and visual skills.

| Skill                        | Description                         |
|------------------------------|-------------------------------------|
| `design-taste-frontend`      | Frontend design taste principles    |
| `full-output-enforcement`    | Complete output generation patterns |
| `high-end-visual-design`     | Premium visual design standards     |
| `industrial-brutalist-ui`    | Brutalist design approach           |
| `minimalist-ui`              | Minimalist design principles        |
| `redesign-existing-projects` | Project redesign strategies         |
| `stitch-design-taste`        | Unified design taste system         |

---

## Commands (3)

Custom slash commands. Invoke with `/command`.

| Command       | Description                                                   |
|---------------|---------------------------------------------------------------|
| `/brainstorm` | Multi-perspective structured brainstorming with expert lenses |
| `/create`     | Create new features                                           |
| `/plan`       | Task breakdown and implementation planning                    |

---

## Skill Loading Protocol

```plaintext
User Request -> Skill Description Match -> Load SKILL.md
                                             |
                                      Read references/
                                             |
                                      Read scripts/
```

### Skill Structure

```plaintext
skill-name/
├── SKILL.md           # (Required) Metadata and instructions
├── scripts/           # (Optional) Python/Bash scripts
├── references/        # (Optional) Templates, docs
└── assets/            # (Optional) Images, logos
```

---

## Settings

### settings.json

Main configuration file for Claude Code permissions, hooks, and defaults.

| Section       | Purpose                                        |
|---------------|------------------------------------------------|
| `permissions` | Allow/deny specific tools (Bash, Write, etc.)  |
| `hooks`       | Event-driven scripts (pre/post tool use)       |
| `model`       | Default and fallback model selection           |
| `env`         | Environment variables for hooks                |
| `outputStyle` | Custom output formatting preferences           |

### .mcp.json

MCP (Model Context Protocol) server configuration for external tool integration.

| Server      | Purpose                                    |
|-------------|--------------------------------------------|
| `context7`  | Upstash context API integration            |
| `shadcn`    | shadcn/ui component generation             |

---

## Output Styles (Optional)

Custom output formatting for different response types.

```plaintext
output-styles/
├── detailed.md              # Verbose, structured responses
├── concise.md               # Minimal, direct responses
└── code-review.md           # Code review format
```

---

## Agent Memory (Optional)

Per-agent persistent memory for context retention across sessions.

```plaintext
agent-memory/
├── orchestrator/
│   └── MEMORY.md            # Orchestrator session history
├── frontend-specialist/
│   └── MEMORY.md            # Frontend patterns learned
└── backend-specialist/
    └── MEMORY.md            # Backend patterns learned
```

---

## Hooks (20)

### Master Hooks (4)

Located in `.claude/hooks/`.

| Hook                 | Purpose                                 |
|----------------------|-----------------------------------------|
| `checklist.py`       | Priority-based validation (core checks) |
| `verify_all.py`      | Comprehensive verification (full suite) |
| `auto_preview.py`    | Automatic preview generation            |
| `session_manager.py` | Session management                      |

### Skill-Level Scripts (16)

| Skill                   | Script                                             |
|-------------------------|----------------------------------------------------|
| `api-patterns`          | `api_validator.py`                                 |
| `database-design`       | `schema_validator.py`                              |
| `frontend-design`       | `accessibility_checker.py`, `ux_audit.py`          |
| `geo-fundamentals`      | `geo_checker.py`                                   |
| `i18n-localization`     | `i18n_checker.py`                                  |
| `lint-and-validate`     | `lint_runner.py`, `type_coverage.py`               |
| `mobile-design`         | `mobile_audit.py`                                  |
| `nextjs-react-expert`   | `convert_rules.py`, `react_performance_checker.py` |
| `performance-profiling` | `lighthouse_audit.py`                              |
| `seo-fundamentals`      | `seo_checker.py`                                   |
| `testing-patterns`      | `test_runner.py`                                   |
| `vulnerability-scanner` | `security_scan.py`                                 |
| `webapp-testing`        | `playwright_runner.py`                             |

### Usage

```bash
# Quick validation during development
python .claude/hooks/checklist.py .

# Full verification before deployment
python .claude/hooks/verify_all.py . --url http://localhost:3000
```

---

## Statistics

| Metric                | Value                                 |
|-----------------------|---------------------------------------|
| **Total Agents**      | 11                                    |
| **Total Skills**      | 31 (24 core + 7 nested design skills) |
| **Total Commands**    | 3                                     |
| **Total Hooks**       | 20 (4 master + 16 skill-level)        |
| **Settings Files**    | 2 (settings.json + settings.local)    |
| **MCP Servers**       | 2 (context7 + shadcn)                 |

---

## Quick Reference

| Need         | Agent                   | Skills                                                  |
|--------------|-------------------------|---------------------------------------------------------|
| Web App      | `frontend-specialist`   | frontend-design, nextjs-react-expert, tailwind-patterns |
| API          | `backend-specialist`    | api-patterns, nodejs-best-practices                     |
| Database     | `database-architect`    | database-design                                         |
| Planning     | `project-planner`       | brainstorming, plan-writing, architecture               |
| Performance  | `performance-optimizer` | performance-profiling                                   |
| Security     | -                       | vulnerability-scanner                                   |
| Testing      | -                       | testing-patterns, webapp-testing, tdd-workflow          |
| SEO          | `seo-specialist`        | seo-fundamentals, geo-fundamentals                      |
| Mobile       | -                       | mobile-design                                           |
| Code Quality | `code-archaeologist`    | clean-code, code-review-checklist                       |
