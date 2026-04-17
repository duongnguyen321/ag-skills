# Qwen Code Kit Architecture

> Comprehensive AI Agent Capability Expansion Toolkit for Qwen Code CLI

---

## Overview

Qwen Code Kit is a streamlined system providing:

- **11 Specialist Agents** - Role-based AI personas
- **31 Skills** - Domain-specific knowledge modules (shared with `.qwen/skills/`)
- **3 Commands** - Custom slash commands
- **20 Scripts** - Validation tools (in `.qwen/scripts/`)

**Note:** Qwen Code shares skills and scripts with `.qwen/` for consistency across AI tools.

---

## Directory Structure

```plaintext
.qwen/
├── ARCHITECTURE.md              # This file
├── settings.json                # Project settings (model, permissions, hooks, env)
├── agents/                      # 11 Specialist Agents
├── skills/                      # Custom skills (project-specific)
├── commands/                    # 3 Custom Commands
└── sandbox-*                    # Sandbox config (optional, Docker/SB files)
```

### Shared Resources (from .qwen/)

```plaintext
.qwen/
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

### Global Scope (~/.qwen/)

```plaintext
~/.qwen/
├── settings.json                # Global/user settings
├── QWEN.md                      # Global instructions
├── .env                         # API keys, env vars
├── trustedFolders.json          # Security rules for folders
├── commands/                    # Global custom commands
├── skills/                      # Global reusable skills
└── agents/                      # Global sub-agents
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

## Commands (3)

Custom slash commands. Invoke with `/command`.

| Command       | Description                                                   |
|---------------|---------------------------------------------------------------|
| `/brainstorm` | Multi-perspective structured brainstorming with expert lenses |
| `/create`     | Create new features                                           |
| `/plan`       | Task breakdown and implementation planning                    |

---

## Configuration

### settings.json

Project-level configuration with hierarchical loading.

```json
{
  "model": {
    "default": "qwen-coder-plus-latest",
    "fallback": "qwen-plus-latest"
  },
  "permissions": {
    "allow": ["Bash", "Read", "Write", "Edit", "Glob", "Grep", "Agent"],
    "deny": []
  },
  "context": {
    "includeDirectories": [".qwen/skills", ".qwen/agents", ".qwen/commands"]
  },
  "hooks": {
    "sessionStart": [],
    "sessionEnd": [],
    "preToolUse": [],
    "postToolUse": []
  },
  "env": {}
}
```

### QWEN.md (Project Root)

Persistent instructions auto-loaded each session. Equivalent to CLAUDE.md.

---

## Statistics

| Metric              | Value                                 |
|---------------------|---------------------------------------|
| **Total Agents**    | 11                                    |
| **Total Skills**    | 31 (in `.qwen/skills/`) + custom in `.qwen/skills/` |
| **Total Commands**  | 3                                     |
| **Total Scripts**   | 20 (in `.qwen/scripts/`)             |

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

## Configuration Hierarchy

Settings load hierarchically (highest priority first):

```
CLI args → ENV vars → Project (.qwen/settings.json) → User (~/.qwen/settings.json) → Defaults
```

> Use `.env` for API keys. Use `trustedFolders.json` for security rules.
