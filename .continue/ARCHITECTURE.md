# Continue Kit Architecture

> Comprehensive AI Agent Capability Expansion Toolkit for Continue IDE

---

## Overview

Continue Kit is a modular system for Continue IDE integration, providing:

- **11 Specialist Agents** - Role-based AI personas
- **31 Skills** - Domain-specific knowledge modules (in `.agent/skills/`)
- **3 Prompts** - Custom slash commands
- **20 Scripts** - Validation tools (in `.agent/scripts/`)

**Note:** Continue uses `.agent/skills/` for skills (shared with other AI tools).

---

## Directory Structure

```plaintext
.continue/
├── ARCHITECTURE.md              # This file
├── .continuerc.json             # Workspace-level configuration
├── agents/                      # 11 Specialist Agents
├── rules/                       # Global Rules (CONTINUE.md)
├── prompts/                     # 3 Custom Prompts (slash commands)
├── configs/                     # Config profiles (YAML)
└── assistants/                  # Assistant definitions (optional)
```

### Shared Resources (from .agent/)

```plaintext
.agent/
├── skills/                      # 31 Skills (shared across AI tools)
│   ├── api-patterns/
│   ├── architecture/
│   ├── brainstorming/
│   ├── clean-code/
│   └── ... (27 more skills)
└── scripts/                     # 20 Validation Scripts
    ├── checklist.py
    ├── verify_all.py
    └── ... (18 more scripts)
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

## Prompts (3)

Custom slash commands for Continue IDE. Invoke with `/command`.

| Command       | Description                                                   |
|---------------|---------------------------------------------------------------|
| `/brainstorm` | Multi-perspective structured brainstorming with expert lenses |
| `/create`     | Create new features                                           |
| `/plan`       | Task breakdown and implementation planning                    |

---

## Configuration

### .continuerc.json

Workspace-level configuration referencing rules, prompts, and agents.

```json
{
  "rules": ["rules/*.md"],
  "prompts": ["prompts/*.md"],
  "agents": ["agents/*.md"]
}
```

### Global Config (~/.continue/config.yaml)

User-level configuration for models, rules, and MCP servers.

```yaml
models:
  - provider: openai
    model: gpt-4
    apiKey: ${OPENAI_API_KEY}

rules:
  - uses: .continue/rules/*.md

prompts:
  - uses: .continue/prompts/*.md

mcpServers:
  context7:
    command: npx
    args: ["-y", "@upstash/context7-mcp"]
```

---

## Statistics

| Metric              | Value                                 |
|---------------------|---------------------------------------|
| **Total Agents**    | 11                                    |
| **Total Skills**    | 31 (in `.agent/skills/`)              |
| **Total Prompts**   | 3                                     |
| **Total Scripts**   | 20 (in `.agent/scripts/`)             |

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

---

## Continue vs Other AI Tools

| Feature | Continue | Claude Code | Antigravity |
|---------|----------|-------------|-------------|
| **Config** | config.yaml + .continuerc.json | settings.json | mcp_config.json |
| **Structure** | Simple (rules/, prompts/, agents/) | Complex (skills/, commands/, hooks/, etc.) | Complex (skills/, workflows/, scripts/) |
| **Skills Location** | References `.agent/skills/` | `.claude/skills/` | `.agent/skills/` |
| **IDE Integration** | VS Code, JetBrains | Terminal-based | Terminal-based |
| **Focus** | Autocomplete + Chat + Agent | Multi-agent orchestration | Multi-agent orchestration |
| **Best For** | IDE workflows, quick tasks | Complex projects, team collaboration | Complex projects, team collaboration |
