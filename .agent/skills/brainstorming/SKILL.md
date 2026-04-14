---
name: brainstorming
description: Multi-perspective structured brainstorming with expert lenses. Generates 3-5 options with trade-off analysis before implementation.
allowed-tools: Read, Glob, Grep
---

# Brainstorming Skill

> "Options stress-tested by multiple perspectives, not just the first angle you think of."

## When to Use

| Trigger | Action |
|---------|--------|
| Architecture decision needed | Activate brainstorm |
| New feature design | Activate brainstorm |
| Tech stack choice | Activate brainstorm |
| Ambiguous problem statement | Ask 1 clarifying question, then brainstorm |

---

## Content Map

| File | Description | When to Read |
|------|-------------|--------------|
| `dynamic-questioning.md` | Question generation for clarifying scope | Problem is ambiguous, need to narrow scope |

---

## Related Skills

| Skill | Use For |
|-------|---------|
| `@[skills/architecture]` | Architecture decision frameworks |
| `@[skills/plan-writing]` | Breaking chosen option into tasks |

---

## Brainstorm Protocol

### PHASE 1 - Context Ingestion (Silent)

1. Read all `@file_ref` files referenced in the command
2. Read `README.md`, `TODO.md` if available in context
3. Identify: tech stack in use, existing patterns, stated constraints
4. Define the decision axis clearly: "We are deciding how to..."
5. Select 3+ expert lenses (see Lens Roster)

> Do NOT generate options yet. Only after context is fully understood.

### PHASE 2 - Option Generation with Multi-Lens Analysis

Generate **3 to 5 options**. For each option:
1. Describe the approach (architecture, data flow, key components - no code)
2. Run all selected expert lenses against it
3. Score Effort and Risk
4. State what type of project/scale/team this option suits

---

## Core Constraint: Minimum 3 Expert Lenses

Every option MUST be analyzed by at least **3 expert perspectives**.

> If fewer than 3 lenses analyze each option, the brainstorm is incomplete. Stop and complete missing lenses.

### Expert Lens Roster

Always include these 2:
- **Architect Lens** - scalability, system design, long-term structure
- **Devil's Advocate Lens** - what breaks, hidden risks, real-world failure modes

Plus 1+ **Domain Lenses** based on problem type:

| Problem Type | Domain Lens |
|--------------|-------------|
| API / Backend | Backend Specialist |
| UI / UX | Frontend Specialist |
| Data / Schema | Database Architect |
| Auth / Access | Security Auditor |
| Infra / Deploy | DevOps Engineer |
| Performance | Performance Optimizer |
| Full Stack | Pick 2 domain lenses |

---

## Output Format

For each option, follow this structure:

```markdown
### Option [X]: [Name]

> [One-line summary]

**How it works:**
[2-4 sentences. No code. Use diagrams if spatial.]

**Architect Lens**
- [Structural strength or concern]
- [Scalability characteristic]

**Devil's Advocate Lens**
- [What fails under load or edge cases]
- [Hidden operational complexity]

**[Domain Lens]**
- [Domain-specific insight]

**Pros:**
- [Concrete benefit]

**Cons:**
- [Concrete drawback]

**Effort:** Low | Medium | High
**Risk:** Low | Medium | High
**Best for:** [Team size / scale / use case]
```

After all options, include:

1. **Lens Synthesis Table** - summary comparison matrix
2. **Recommendation** - clear pick with reasoning based on lens synthesis
3. **Assumptions stated** - "This assumes [X]. If [X] changes, reconsider Option [Y]"

---

## Validation Checklist

Before presenting brainstorm:

- [ ] All `@file_ref` files read and understood
- [ ] Decision axis clearly defined
- [ ] 3-5 options generated
- [ ] Each option has 3+ expert lenses applied
- [ ] Devil's Advocate found real risks (no fake cons)
- [ ] Effort/Risk scored honestly
- [ ] Clear recommendation provided
- [ ] No code in brainstorm output
- [ ] Assumptions stated explicitly

---

## Anti-Patterns

**Don't:**
- Generate code during brainstorm phase
- Present only 1-2 options (not a brainstorm)
- Use fewer than 3 expert lenses
- Write fake cons like "requires more initial setup" without elaboration
- Give recommendations like "choose based on your needs" - be specific
- Ignore `@file_ref` context (e.g., recommend MongoDB when Prisma+Postgres already exists)
- Start generating options before reading all context files

**Do:**
- Read context first, always
- Give honest effort estimates
- Provide clear, opinionated recommendation
- Use ASCII or Mermaid diagrams for spatial architecture
- Ask ONE clarifying question if ambiguous - not a form

---

## What Happens After Brainstorm

```
/brainstorm -> User picks option -> /plan -> Implementation
```

- `/plan` takes the chosen option and breaks it into tasks, file structure, milestones
- If user wants a hybrid of two options, state it before running `/plan`
- Never skip to `/plan` without a clear option selected
