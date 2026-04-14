---
description: Multi-perspective structured brainstorming with orchestrated expert lenses. Explores options deeply before any implementation.
usage: /brainstorm @file_ref1, @file_ref2. [Issue | Feature | Business logic | Architecture question]
---

# /brainstorm - Orchestrated Idea Exploration

$ARGUMENTS

---

## Purpose

`/brainstorm` activates **ORCHESTRATED BRAINSTORM MODE** - a structured, multi-lens exploration of a problem before any
code is written.

It merges two disciplines:

- **Brainstorm DNA** - Options, tradeoffs, effort, honest cons, clear recommendation
- **Orchestration DNA** - Minimum 3 expert perspectives analyze each option independently, then synthesize

The result: options that are stress-tested from multiple angles, not just the first angle you think of.

---

## Usage

```
/brainstorm @file_ref1, @file_ref2. Issue or feature or architecture question
```

**Examples:**

```
/brainstorm @README.md, @schema.prisma. Design the notification system
/brainstorm @TODO.md. Authentication strategy for multi-tenant SaaS
/brainstorm @api-spec.md, @ERD.png. Caching layer for high-read endpoints
/brainstorm State management approach for the checkout flow
/brainstorm Real-time collaboration: WebSocket vs SSE vs polling
```

`@file_ref` is optional - attach any relevant context files, schemas, specs, or READMEs.

---

## Pre-Flight Checklist

Before generating options, the agent MUST resolve:

| Check                                           | Action                                                                                              |
|-------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Are `@file_ref` files attached?                 | Read and extract relevant constraints, data models, existing decisions                              |
| Is there a `README.md` or `TODO.md` in context? | Read for business logic and product goals                                                           |
| Is the problem statement ambiguous?             | Ask **one clarifying question** before proceeding - not more                                        |
| Is scope too broad?                             | Narrow to a single decision axis (e.g., "pick the storage strategy", not "design the whole system") |

---

## Core Constraint: Minimum 3 Expert Lenses

Every option is reviewed by at least **3 expert perspectives** before being presented.

> If fewer than 3 lenses analyze each option - the brainstorm is incomplete. Stop and complete missing lenses.

### Expert Lens Roster

Select lenses relevant to the domain. At minimum, always include:

- **Architect Lens** - scalability, system design, long-term structure
- **Devil's Advocate Lens** - what breaks this, hidden risks, real-world failure modes
- One **Domain Lens** based on problem type:

| Problem Type   | Domain Lens           |
|----------------|-----------------------|
| API / Backend  | Backend Specialist    |
| UI / UX        | Frontend Specialist   |
| Data / Schema  | Database Architect    |
| Auth / Access  | Security Auditor      |
| Infra / Deploy | DevOps Engineer       |
| Performance    | Performance Optimizer |
| Full Stack     | Pick 2 domain lenses  |

---

## Strict 2-Phase Execution

### PHASE 1 - Context Ingestion (Silent, No Output Yet)

1. Read all `@file_ref` files referenced in the command
2. Read `README.md`, `TODO.md` if available in context
3. Identify: tech stack in use, existing patterns, stated constraints
4. Define the decision axis clearly (one sentence: "We are deciding how to...")
5. Select which 3+ expert lenses apply

> Do NOT generate options yet. Only after context is fully understood.

### PHASE 2 - Option Generation with Multi-Lens Analysis

Generate **3 to 5 options**. For each option:

1. Describe the approach (architecture, data flow, key components - no code)
2. Run all selected expert lenses against it
3. Score Effort and Risk
4. State what type of project/scale/team this option is best suited for

---

## Output Format

```markdown
## Brainstorm: [Topic]

### Context

[1-3 sentences: what problem we're solving, who the user is, key constraints from @file_refs]

**Decision axis:** We are deciding how to [X].

**Lenses applied:** Architect - Devil's Advocate - [Domain Lens 1] - [Domain Lens 2 if applicable]

---

### Option A: [Name - e.g. "Event-Driven Queue"]

> [One-line summary of the approach]

**How it works:**
[2-4 sentences describing the architecture or approach. No code. Use diagrams if spatial.]

**Architect Lens**

- [Structural strength or concern]
- [Scalability characteristic]
- [Long-term maintainability note]

**Devil's Advocate Lens**

- [What fails under load or edge cases]
- [Hidden operational complexity]
- [Team/org risk]

**[Domain Lens]**

- [Domain-specific insight - e.g. DB index implications, auth flow complexity, bundle size impact]

**Pros:**

- [Concrete benefit]
- [Concrete benefit]

**Cons:**

- [Concrete drawback]
- [Concrete drawback]

**Effort:** Low | Medium | High
**Risk:** Low | Medium | High
**Best for:** [Team size / scale / use case this shines in]

---

### Option B: [Name]

[Same structure as Option A]

---

### Option C: [Name]

[Same structure as Option A]

---

## Lens Synthesis

| Criteria         | Option A              | Option B | Option C |
| ---------------- | --------------------- | -------- | -------- |
| Architect        | pass / warning / fail |          |          |
| Devil's Advocate |                       |          |          |
| Domain Lens      |                       |          |          |
| Effort           | Low / Med / High      |          |          |
| Risk             | Low / Med / High      |          |          |

---

## Recommendation

**Option [X]** - because [1-2 sentences of reasoning based on the lens synthesis, not opinion].

> This recommendation
> assumes: [stated constraint from context, e.g. "team of 3, early-stage product, PostgreSQL already in stack"].
> If [assumption] changes - reconsider **Option [Y]** instead.

---

**Next step:** Review the options above as Architect.

- Pick one to move forward - `/plan`
- Want deeper exploration of any option - reply with the option letter
- Want a hybrid of two options - describe the combination
```

---

## Key Principles

| Principle               | Rule                                                               |
|-------------------------|--------------------------------------------------------------------|
| **No code**             | This phase is 100% ideas and architecture. No implementation.      |
| **No sycophancy**       | Devil's Advocate lens MUST find real risks. No fake cons.          |
| **Context-first**       | Always read @file_refs before generating anything.                 |
| **Honest effort**       | Don't underestimate. "Low effort" means genuinely low.             |
| **Opinionated rec**     | Give a clear recommendation. "It depends" is not a recommendation. |
| **Defer final call**    | User is the Architect. Recommendation is input, not decree.        |
| **Visual when spatial** | Use ASCII or Mermaid diagrams for architecture flows, not prose.   |
| **One clarifying Q**    | If ambiguous, ask ONE question - not a form.                       |

---

## What Happens After /brainstorm

```
/brainstorm - User picks option - /plan - Implementation
```

- `/plan` takes the chosen option and breaks it into tasks, file structure, and milestones
- If the chosen option needs a hybrid adjustment, state it before running `/plan`
- Never skip to `/plan` without a clear option selected

---

## Anti-Patterns (Never Do These)

- Generating code during brainstorm phase
- Only presenting 1 or 2 options ("Option A or B" is not a brainstorm)
- Running fewer than 3 expert lenses
- Fake cons like "requires more initial setup" with no elaboration
- Recommendations that say "choose based on your needs" - be specific
- Ignoring `@file_ref` context (e.g., recommending MongoDB when Prisma+Postgres is already in the schema)
- Starting options before reading all context files
