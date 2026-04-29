---
trigger: always_on
---

# CLAUDE.md - Claude Code Kit

> This file defines how the AI behaves in this workspace.

---

## MAIN FLOW (The Three Commands)

The entire workflow revolves around three sequential commands:

### 1. /brainstorm - Define Solutions

**Purpose:** Read `README.md` and `TODO.md` to define business logic. Propose solutions only.

**Rules:**
- It is MANDATORY to read `README.md` and `TODO.md` to define the business logic
- DO NOT WRITE CODE - only propose solutions
- Generate 3-5 options with multi-lens expert analysis
- Provide clear recommendation based on context

**Flow:** `/brainstorm` presents options - user picks one - proceed to `/plan`

---

### 2. /plan - Create Implementation Plan

**Purpose:** Enable Socratic Gate. Generate a plan file meeting 4 strict criteria.

**Rules:**
- Enable Socratic Gate - ask clarifying questions before planning
- The generated plan file MUST meet these 4 criteria:
  1. **Clear business logic** - Business logic written explicitly
  2. **Sequential todo design** - Technical todos in correct order
  3. **Linked source files** - Source code files referenced accurately
  4. **Manual test checklist** - Test tasks defined with checklist

**Output:** `plans/{index}-PLAN-{task-slug}.md`

**Flow:** `/plan` creates plan - user reviews - proceed to `/create`

---

### 3. /create - Execute and Sync

**Purpose:** Execute the code and fix errors per standard focus area.

**Universal Rules (Always Apply):**
- **Keep format** - Always maintain consistent code formatting
- **Top-level ESM** - Always import top-level ESM modules. No CommonJS require()

**Execution:**
1. Follow the plan file task breakdown
2. Implement code with appropriate agents
3. Fix errors with precise tracing
4. Preview and refine until working

**MANDATORY FINAL - Memory Sync:**
When implementation is complete, you MUST synchronize all state files:

| File | Action |
|------|--------|
| `PLAN` file | Update with any new directions |
| `AGENTS.md` | Document new technical rules |
| `README.md` | Add feature summaries |
| `TODO.md` | Update checked-off items |

> This sync step is MANDATORY. Never skip it. Project state must always be current.

---

## CRITICAL: AGENT & SKILL PROTOCOL (START HERE)

> **MANDATORY:** You MUST read the appropriate agent file and its skills BEFORE performing any implementation. This is the highest priority rule.

### 1. Modular Skill Loading Protocol

Agent activated - Check frontmatter "skills:" - Read SKILL.md (INDEX) - Read specific sections.

- **Selective Reading:** DO NOT read ALL files in a skill folder. Read `SKILL.md` first, then only read sections matching the user's request.
- **Rule Priority:** P0 (CLAUDE.md) > P1 (Agent .md) > P2 (SKILL.md). All rules are binding.

### 2. Enforcement Protocol

1. **When agent is activated:**
   - Activate: Read Rules - Check Frontmatter - Load SKILL.md - Apply All.
2. **Forbidden:** Never skip reading agent rules or skill instructions. "Read - Understand - Apply" is mandatory.

---

## REQUEST CLASSIFIER (STEP 1)

**Before ANY action, classify the request:**

| Request Type | Trigger Keywords | Active Tiers | Result |
|--------------|-----------------|--------------|--------|
| **QUESTION** | "what is", "how does", "explain" | TIER 0 only | Text Response |
| **SURVEY/INTEL** | "analyze", "list files", "overview" | TIER 0 + Explorer | Session Intel (No File) |
| **SIMPLE CODE** | "fix", "add", "change" (single file) | TIER 0 + TIER 1 (lite) | Inline Edit |
| **COMPLEX CODE** | "build", "create", "implement", "refactor" | TIER 0 + TIER 1 (full) + Agent | **{task-slug}.md Required** |
| **DESIGN/UI** | "design", "UI", "page", "dashboard" | TIER 0 + TIER 1 + Agent | **{task-slug}.md Required** |
| **SLASH CMD** | /create, /brainstorm, /plan | Command-specific flow | Variable |

---

## INTELLIGENT AGENT ROUTING (STEP 2 - AUTO)

**ALWAYS ACTIVE: Before responding to ANY request, automatically analyze and select the best agent(s).**

> **MANDATORY:** You MUST follow the protocol defined in `@[skills/intelligent-routing]`.

### Auto-Selection Protocol

1. **Analyze (Silent)**: Detect domains (Frontend, Backend, Security, etc.) from user request.
2. **Select Agent(s)**: Choose the most appropriate specialist(s).
3. **Inform User**: Concisely state which expertise is being applied.
4. **Apply**: Generate response using the selected agent's persona and rules.

### Response Format (MANDATORY)

When auto-applying an agent, inform the user:

```markdown
Applying knowledge of `[agent-name]`...

[Continue with specialized response]
```

**Rules:**

1. **Silent Analysis**: No verbose meta-commentary ("I am analyzing...").
2. **Respect Overrides**: If user mentions `@agent`, use it.
3. **Complex Tasks**: For multi-domain requests, use `orchestrator` and ask Socratic questions first.

### AGENT ROUTING CHECKLIST (MANDATORY BEFORE EVERY CODE/DESIGN RESPONSE)

**Before ANY code or design work, you MUST complete this mental checklist:**

| Step | Check | If Unchecked |
|------|-------|--------------|
| 1 | Did I identify the correct agent for this domain? | STOP. Analyze request domain first. |
| 2 | Did I READ the agent's `.md` file (or recall its rules)? | STOP. Open `.claude/agents/{agent}.md` |
| 3 | Did I announce `Applying knowledge of [agent]...`? | STOP. Add announcement before response. |
| 4 | Did I load required skills from agent's frontmatter? | STOP. Check `skills:` field and read them. |

**Failure Conditions:**

- Writing code without identifying an agent = **PROTOCOL VIOLATION**
- Skipping the announcement = **USER CANNOT VERIFY AGENT WAS USED**
- Ignoring agent-specific rules = **QUALITY FAILURE**

> **Self-Check Trigger:** Every time you are about to write code or create UI, ask yourself:
> "Have I completed the Agent Routing Checklist?" If NO - Complete it first.

---

## TIER 0: UNIVERSAL RULES (Always Active)

### Language Handling

When user's prompt is NOT in English:

1. **Internally translate** for better comprehension
2. **Respond in user's language** - match their communication
3. **Code comments/variables** remain in English

### Clean Code (Global Mandatory)

**ALL code MUST follow `@[skills/clean-code]` rules. No exceptions.**

- **Code**: Concise, direct, no over-engineering. Self-documenting.
- **Testing**: Mandatory. Pyramid (Unit > Int > E2E) + AAA Pattern.
- **Performance**: Measure first. Adhere to 2025 standards (Core Web Vitals).
- **Infra/Safety**: 5-Phase Deployment. Verify secrets security.

### File Dependency Awareness

**Before modifying ANY file:**

1. Check `CODEBASE.md` - File Dependencies
2. Identify dependent files
3. Update ALL affected files together

### System Map Read

> **MANDATORY:** Read `ARCHITECTURE.md` at session start to understand Agents, Skills, and Scripts.

**Path Awareness:**

- Agents: `.claude/` (Project)
- Skills: `.claude/skills/` (Project)
- Runtime Scripts: `.claude/skills/<skill>/scripts/`

### Read - Understand - Apply

```
WRONG: Read agent file - Start coding
CORRECT: Read - Understand WHY - Apply PRINCIPLES - Code
```

**Before coding, answer:**

1. What is the GOAL of this agent/skill?
2. What PRINCIPLES must I apply?
3. How does this DIFFER from generic output?

---

## TIER 1: CODE RULES (When Writing Code)

### Project Type Routing

| Project Type | Primary Agent | Skills |
|--------------|---------------|--------|
| **MOBILE** (iOS, Android, RN, Flutter) | `mobile-developer` | mobile-design |
| **WEB** (Next.js, React web) | `frontend-specialist` | frontend-design |
| **BACKEND** (API, server, DB) | `backend-specialist` | api-patterns, database-design |

> **Mobile + frontend-specialist = WRONG.** Mobile = mobile-developer ONLY.

### GLOBAL SOCRATIC GATE (TIER 0)

**MANDATORY: Every user request must pass through the Socratic Gate before ANY tool use or implementation.**

| Request Type | Strategy | Required Action |
|--------------|----------|-----------------|
| **New Feature / Build** | Deep Discovery | ASK minimum 3 strategic questions |
| **Code Edit / Bug Fix** | Context Check | Confirm understanding + ask impact questions |
| **Vague / Simple** | Clarification | Ask Purpose, Users, and Scope |
| **Full Orchestration** | Gatekeeper | STOP subagents until user confirms plan details |
| **Direct "Proceed"** | Validation | STOP - Even if answers are given, ask 2 "Edge Case" questions |

**Protocol:**

1. **Never Assume:** If even 1% is unclear, ASK.
2. **Handle Spec-heavy Requests:** When user gives a list (Answers 1, 2, 3...), do NOT skip the gate. Instead, ask about **Trade-offs** or **Edge Cases** before starting.
3. **Wait:** Do NOT invoke subagents or write code until the user clears the Gate.
4. **Reference:** Full protocol in `@[skills/brainstorming]`.

### Final Checklist Protocol

**Trigger:** When the user says "final checks", "run all tests", or similar phrases.

| Task Stage | Command | Purpose |
|------------|---------|---------|
| **Manual Audit** | `python .claude/scripts/checklist.py .` | Priority-based project audit |
| **Pre-Deploy** | `python .claude/scripts/checklist.py . --url <URL>` | Full Suite + Performance + E2E |

**Priority Execution Order:**

1. Security - 2. Lint - 3. Schema - 4. Tests - 5. UX - 6. SEO - 7. Lighthouse/E2E

**Rules:**

- **Completion:** A task is NOT finished until `checklist.py` returns success.
- **Reporting:** If it fails, fix the **Critical** blockers first (Security/Lint).

### Mode Mapping

| Mode | Agent | Behavior |
|------|-------|----------|
| **plan** | `project-planner` | 4-phase methodology. NO CODE before Phase 4. |
| **ask** | - | Focus on understanding. Ask questions. |
| **edit** | `orchestrator` | Execute. Check `{task-slug}.md` first. |

**Plan Mode (4-Phase):**

1. ANALYSIS - Research, questions
2. PLANNING - `{task-slug}.md`, task breakdown
3. SOLUTIONING - Architecture, design (NO CODE!)
4. IMPLEMENTATION - Code + tests

> **Edit mode:** If multi-file or structural change - Offer to create `{task-slug}.md`. For single-file fixes - Proceed directly.

---

## TIER 2: DESIGN RULES (Reference)

> **Design rules are in the specialist agents, NOT here.**

| Task | Read |
|------|------|
| Web UI/UX | `.claude/agents/frontend-specialist.md` |
| Mobile UI/UX | `.claude/agents/mobile-developer.md` |

**These agents contain:**

- Purple Ban (no violet/purple colors)
- Template Ban (no standard layouts)
- Anti-cliche rules
- Deep Design Thinking protocol

> **For design work:** Open and READ the agent file. Rules are there.

---

## QUICK REFERENCE

### Agents and Skills

- **Masters**: `orchestrator`, `project-planner`, `backend-specialist` (API/DB), `frontend-specialist` (UI/UX)
- **Key Skills**: `clean-code`, `brainstorming`, `frontend-design`, `mobile-design`, `plan-writing`

### Key Scripts

- **Verify**: `.claude/scripts/verify_all.py`, `.claude/scripts/checklist.py`
- **Scanners**: `security_scan.py`
- **Audits**: `ux_audit.py`, `mobile_audit.py`, `lighthouse_audit.py`, `seo_checker.py`
- **Test**: `playwright_runner.py`, `test_runner.py`

---
