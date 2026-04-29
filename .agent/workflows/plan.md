---
description: Create project plan using project-planner agent. Enables Socratic Gate for risk blind-spot clearance. Output must meet 4 strict criteria.
usage: /plan [Issue | Feature | Architecture decision]
---

# /plan - Project Planning Mode

$ARGUMENTS

---

## Purpose

`/plan` activates **PROJECT PLANNING MODE** - a structured breakdown of work into clear tasks with verification criteria. No code writing - only plan file generation.

---

## Critical Rules

1. **NO CODE WRITING** - This command creates plan file only
2. **Use project-planner agent** - Follow its protocol
3. **Socratic Gate** - Ask clarifying questions before planning to clear risk blind spots
4. **Dynamic Naming** - Plan file named based on task

---

## Execution

### Step 1: Context Check

Follow `project-planner.md` Phase -1 (Context Check):
- Read relevant project files
- Understand existing patterns
- Identify constraints

### Step 2: Socratic Gate

Follow `project-planner.md` Phase 0 (Socratic Gate):
- Ask questions to clear risk blind spots
- Confirm understanding before planning
- Do NOT proceed until user confirms

### Step 3: Create Plan File

Generate `plans/PLAN-{index}-{task-slug}.md` with task breakdown.

**Naming Rules:**
1. Extract 2-3 key words from request
2. Lowercase, hyphen-separated
3. Max 30 characters
4. Example: "e-commerce cart" - `PLAN-ecommerce-cart.md`

### Step 4: Mandatory Verification

The plan file MUST explicitly satisfy these 4 criteria before proceeding:

| # | Criterion | Description |
|---|-----------|-------------|
| 1 | Clear business logic | Business logic written explicitly and unambiguously |
| 2 | Sequential todo design | Technical todos listed in correct sequential order |
| 3 | Linked source files | Source code files referenced with accurate paths |
| 4 | Manual test checklist | Test tasks defined with manual testing checklist |

**Verification Checklist in Plan File:**

```markdown
- [ ] Business logic clearly written?
- [ ] Technical todos listed sequentially?
- [ ] Source code files referenced accurately?
- [ ] Manual test checklist defined?
```

---

## Expected Output

| Deliverable | Location |
|-------------|----------|
| Project Plan | `plans/PLAN-{index}-{task-slug}.md` |
| Task Breakdown | Inside plan file |
| Agent Assignments | Inside plan file |
| Verification Checklist | Phase X in plan file |

---

## After Planning

Tell user:
```
[OK] Plan created: plans/PLAN-{index}-slug.md

Next steps:
- Review the plan
- Run /create to start implementation
- Or modify plan manually
```

---

## Naming Examples

| Request | Plan File |
|---------|-----------|
| `/plan e-commerce site with cart` | `plans/PLAN-001-ecommerce-cart.md` |
| `/plan mobile app for fitness` | `plans/PLAN-002-fitness-app.md` |
| `/plan add dark mode feature` | `plans/PLAN-003-dark-mode.md` |
| `/plan fix authentication bug` | `plans/PLAN-004-auth-fix.md` |
| `/plan SaaS dashboard` | `plans/PLAN-005-saas-dashboard.md` |
---

## Usage

```
/plan e-commerce site with cart
/plan mobile app for fitness tracking
/plan SaaS dashboard with analytics
```
