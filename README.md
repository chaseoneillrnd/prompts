# Claude AI Skills

A collection of skills for use with **Claude Code** and **Cline** to supercharge codebase analysis and implementation planning.

---

## Skills Overview

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| [`trace`](#-deep-trace) | Systematically analyze existing code | Before refactoring, debugging, or understanding unfamiliar code |
| [`technical-guide-architect`](#-technical-guide-architect) | Create agent-executable implementation guides | Before building new features or systems |

---

## 🔍 Deep-Trace

**File:** `skills/trace/SKILL.md`

Systematically map and analyze execution paths through any codebase using a 7-phase iterative framework. Produces comprehensive documentation in `ai_docs/` that makes refactoring and debugging decisions trivial.

### What It Does

Transforms "I don't understand this code" into deep, documented understanding by tracing:
- Entry points and exit points
- Happy paths, error paths, and edge cases
- Anti-patterns, bottlenecks, and architectural issues
- Visual Mermaid diagrams of execution flows

### Usage

#### Claude Code

```
@deep-trace <feature or execution path to analyze>
```

**Examples:**
```
@deep-trace the user authentication flow and identify security gaps
@deep-trace how orders are processed from cart to fulfillment
@deep-trace the WebSocket connection lifecycle and find memory leaks
```

#### Cline

Add the skill content to your system prompt or reference it in your task:

```
Using the Deep-Trace methodology, analyze <feature> through all 7 phases.
Start with Phase 1 (Lightning Overview) and pause for my approval before continuing.
```

### The 7 Phases

| Phase | Name | Output |
|-------|------|--------|
| 1 | Lightning Overview & Path Mapping | `ai_docs/notes/phase1-overview.md` |
| 2 | Path Analysis & Alternative Assessment | `ai_docs/notes/phase2-analysis.md` |
| 3 | Deep Execution Tracing | `ai_docs/notes/phase3-deep-trace.md` |
| 4 | Pattern Analysis & Issue Identification | `ai_docs/notes/phase4-issues.md` |
| 5 | Solution Synthesis & Documentation | `ai_docs/solutions/` |
| 6 | Visualization & Architecture Mapping | `ai_docs/diagrams/` |
| 7 | Final Review & Deliverable Packaging | Executive summary + roadmap |

> **Tip:** The agent pauses after each phase for your approval. You can stop early — every phase delivers standalone value.

### Output Structure

```
ai_docs/
├── notes/
│   ├── phase1-overview.md
│   ├── phase2-analysis.md
│   ├── phase3-deep-trace.md
│   └── phase4-issues.md
├── specs/
│   ├── current-implementation.md
│   └── execution-flows.md
├── solutions/
│   ├── current-solution.md
│   └── [reason]-proposed-solution.md
└── diagrams/
    └── [component]-flow.md
```

---

## 🏗️ Technical Guide Architect

**File:** `skills/technical-guide-architect/SKILL.md`

Creates comprehensive, agent-executable implementation guides through rigorous Socratic clarification followed by structured documentation. Always produces two deliverables: a Technical Implementation Guide and a Visual Architecture Companion.

### What It Does

Transforms vague requirements into precise, bite-sized prompts an AI agent can execute sequentially — with TDD instructions, acceptance criteria, and "Do NOT" guardrails in every prompt.

### Usage

#### Claude Code

```
Create a technical guide for <project description>
```

**Examples:**
```
Create a technical guide for building a REST API with authentication
Create an implementation guide for migrating from REST to GraphQL
Create a development playbook for a multi-tenant SaaS billing system
```

#### Cline

```
Using the Technical Guide Architect skill, create an implementation guide for <project>.
Begin with Phase 1 Socratic clarification — ask me the Round 1 questions before writing anything.
```

> **Important:** The skill asks 12–20 clarifying questions across 4 rounds **before** writing any documentation. Do not skip this step — the quality of the guide depends on it.

### The Clarification Rounds

| Round | Focus | Questions |
|-------|-------|-----------|
| 1 | Project Scope | End deliverable, audience, starting point, stack, timeline, compliance |
| 2 | Technical Depth | Testing philosophy, security, infrastructure, deployment, integrations |
| 3 | Quality Standards | Code quality, documentation, definition of done, architecture principles |
| 4 | Success Criteria | What success looks like, what would make the guide insufficient |

### Output Deliverables

Two files are always created:

```
[project]-technical-guide.md     ← Executable prompts with TDD, acceptance criteria, guardrails
[project]-diagrams.md            ← Mermaid diagrams: flows, architecture, ERDs, state machines
```

#### Prompt Structure (every prompt includes)

```markdown
#### Prompt N.M.P: Descriptive Title

Requirements:
- Specific, verifiable requirement

File structure:
path/
├── file1
└── file2

Write tests FIRST for:
- Test case 1

Do NOT:
- Anti-pattern to avoid

Acceptance Criteria:
- [ ] Binary pass/fail criterion
```

---

## Using Both Skills Together

These skills are designed to complement each other:

```
Existing Codebase → [trace] → Understand current state → [technical-guide-architect] → Plan improvements
New Project       →           [technical-guide-architect] → Build from scratch
```

**Recommended workflow for refactoring:**

1. Run `@deep-trace` on the area you want to change → produces `ai_docs/` analysis
2. Feed the `ai_docs/solutions/` output into a technical guide request
3. Use the generated guide to execute the refactor systematically

**Example:**
```
# Step 1
@deep-trace the payment processing module and identify coupling issues

# Step 2 (after trace completes)
Create a technical guide for refactoring the payment module based on
the analysis in ai_docs/solutions/decoupling-proposed-solution.md
```

---

## Installation

### Claude Code

Place the skill files anywhere in your project or global `~/.claude/` directory. Reference them using `@` mentions or configure them as custom skills in your Claude Code settings.

```
.claude/
└── skills/
    ├── trace/
    │   └── SKILL.md
    └── technical-guide-architect/
        └── SKILL.md
```

### Cline

Add skill content to your Cline system prompt, or reference the files directly in your task messages using Cline's file reference syntax (`@file`).

```
# In your .clinerules or system prompt:
@.claude/skills/trace/SKILL.md
@.claude/skills/technical-guide-architect/SKILL.md
```

---

## Tips

- **Trace first, plan second** — Never build on code you don't understand
- **Let the phases gate your work** — Each phase of trace produces standalone value; stop early if needed
- **Don't skip clarification** — The technical guide architect's questions prevent rework
- **Keep `ai_docs/` in version control** — The trace output is living documentation
- **Reuse trace output** — Phase 4 issues feed directly into technical guide requirements


https://claude.ai/public/artifacts/ca74314b-91d3-482e-8329-e9105058b51a
