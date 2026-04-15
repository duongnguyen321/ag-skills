---
description: Execute code implementation according to plan. Fix errors per standard focus area. Mandatory final sync of PLAN, AGENTS.md, README.md, and TODO.md.
usage: /create [Plan file reference or feature description]
---

# /create - Create Application

$ARGUMENTS

---

## Purpose

`/create` activates **IMPLEMENTATION MODE** - executing code according to the plan, fixing errors per standard focus area, and synchronizing all memory files upon completion.

---

## Universal Rules (Always Apply)

| Rule | Requirement |
|------|-------------|
| **Keep format** | Always maintain consistent code formatting throughout |
| **Top-level ESM** | Always import top-level ESM modules. No CommonJS require() |

---

## Execution Steps

### Step 1: Request Analysis

- Understand what the user wants
- If information is missing, ask clarifying questions
- Check if a plan file exists and follow it

### Step 2: Project Planning (If No Plan Exists)

- Use `project-planner` agent for task breakdown
- Determine tech stack
- Plan file structure
- Create plan file

### Step 3: Application Building (After Approval)

- Orchestrate with appropriate agents:
  - `database-architect` - Schema design
  - `backend-specialist` - API development
  - `frontend-specialist` - UI implementation
- Follow the plan file task breakdown
- Apply agent-specific rules and skills

### Step 4: Preview and Refine

- Start with `auto_preview.py` when complete
- Precise error tracing and fixing if there are bugs (focus on exact lines/flows)
- Present URL to user
- Iterate until working

### Step 5: Post-Creation Memory Sync (MANDATORY)

When implementation is complete, you MUST update all static memory files to prevent memory loss:

| File | Action |
|------|--------|
| `docs/PLAN-{task-slug}.md` | Update with any new directions or changes |
| `README.md` | Add feature summaries and current state |
| `AGENTS.md` | Document any new technical rules or decisions |
| `TODO.md` | Update checked-off items for completed work |

> This step is MANDATORY. Never skip it. The project state must always be synchronized.

---

## Usage Examples

```
/create blog site
/create e-commerce app with product listing and cart
/create todo app
/create Instagram clone
/create crm system with customer management
```

---

## Before Starting

If request is unclear, ask:
- What type of application?
- What are the basic features?
- Who will use it?

Use defaults, add details later.

---

## Completion Checklist

- [ ] Code implemented per plan
- [ ] Errors fixed with precise tracing
- [ ] Application running and previewable
- [ ] `PLAN` file updated
- [ ] `README.md` synchronized
- [ ] `AGENTS.md` synchronized
- [ ] `TODO.md` synchronized
