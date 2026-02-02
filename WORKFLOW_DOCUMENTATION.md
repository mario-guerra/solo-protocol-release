# AI-Assisted Development Workflow

**Author:** Mario Guerra  
**Date:** January 29, 2026  
**Based on:** Session Inspector React Frontend Migration

---

## Executive Summary

This document describes a structured workflow for AI-assisted software development using Cursor IDE with custom slash commands. The workflow emphasizes **planning before coding**, **iterative critique**, and **quality gates** before deployment. It was refined over multiple iterations of a full-stack application migration (vanilla JS → React + TypeScript frontend with FastAPI backend hardening).

---

## Core Philosophy

### 1. Plan First, Code Second
Never jump directly into implementation. Every significant piece of work goes through a planning phase that includes:
- Analysis of existing code/systems
- Explicit design decisions with trade-off documentation
- Self-critique and revision before implementation

### 2. Trust But Verify
The AI agent does the heavy lifting, but the human maintains control through:
- Explicit approval gates between phases
- Structured review processes
- Quality checks before commits

### 3. Persistent Context
Sessions can span days or weeks. Context preservation is critical:
- Session memory files capture state between conversations
- The AI can resume work without re-explaining the project

### 4. API-First Design
The API contract is the source of truth. Before implementation begins:
- Define all endpoints, request/response schemas, and error codes
- Generate types for both frontend and backend from the same specification
- Use the contract to enable independent technology choices

The `.API/` folder contains the canonical contract (OpenAPI spec, JSON schemas, TypeScript types, examples). This enables porting frontend and backend to different technologies without ambiguity about interfaces or behavior.

### 5. Test-Driven Development
Production code is a liability without tests. Quality is a prerequisite, not a phase:
- Write tests before implementation—they define "done"
- Zero-bug tolerance: all failure modes must be handled
- Tests are living documentation of expected behavior
- No code ships without passing the full test suite

TDD isn't just about catching bugs—it forces clear thinking about interfaces and edge cases before code is written.

### 6. Security-First Architecture
Security is not a feature; it is an architectural constraint. Auth and RBAC are designed before feature code:
- Zero-Trust authorization is the default
- All API endpoints must have explicit permission scopes
- `lockedfiles` protect security-critical components from AI drift
- Security Audits (`/critique` and `/review`) are mandatory gates

---

## The Command System

Custom slash commands in `.cursor/commands/` define specialized agent personas. Each command transforms the AI into a domain expert with specific responsibilities and output formats.

### Command Inventory

| Command | Persona | Purpose |
|---------|---------|---------|
| `/resume` | Context Loader | Resume from `.tmp/SESSION_MEMORY.md` |
| `/architect` | Principal Architect | Generate technical specifications |
| `/design` | UX Oracle | Design UI/UX when interface is impacted |
| `/critique` | Devil's Advocate | Challenge plans, find gaps |
| `/revise` | Senior Designer | Address critique with best practices |
| `/tickets` | Technical PM | Decompose approved specs into actionable tickets |
| `/code` | Principal Engineer | Implement with TDD, zero-bug tolerance |
| `/test` | Test Runner | Run all tests, ensure "green and clean" |
| `/review` | Senior Staff Engineer | Formal code audit with severity matrix |
| `/fix` | Staff SRE | Remediate all issues from review |
| `/memory` | Context Manager | Write session handoff document |

---

## The Workflow Loop

```
┌─────────────────────────────────────────────────────────────┐
│                     PLANNING PHASE                          │
│  /resume → /architect → [/design] → /critique ⇄ /revise     │
│                                                → /tickets   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (explicit approval)
┌─────────────────────────────────────────────────────────────┐
│                   IMPLEMENTATION PHASE                      │
│              /code → "continue" → "continue"                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (implementation complete)
┌─────────────────────────────────────────────────────────────┐
│                      QUALITY PHASE                          │
│                    /review → /fix                           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (all issues resolved)
┌─────────────────────────────────────────────────────────────┐
│                      RELEASE PHASE                          │
│            commit → tag → /memory → (deploy)                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (deployment)
┌─────────────────────────────────────────────────────────────┐
│                       DEPLOY PHASE                          │
│             deploy-container.sh → verification              │
└─────────────────────────────────────────────────────────────┘
                              │
                    (verification failed?)
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                       DEBUG PHASE                           │
│           /sherlock → [architect|critique] → (loop)         │
└─────────────────────────────────────────────────────────────┘
```│            commit → tag → /memory → (deploy)                │
└─────────────────────────────────────────────────────────────┘
```

---

## Detailed Phase Breakdown

### Phase 1: Resume & Orient

**Command:** `/resume`

Start every session by loading context from the session memory file. The AI reads `.tmp/SESSION_MEMORY.md` and orients itself to:
- Project state (what's done, what's pending)
- Critical knowledge (gotchas, patterns, edge cases)
- User preferences (coding style, commit format)

**Example session start:**
```
User: /resume
AI: Good to see you again, Mario! I'm Archie — I'll be picking up where we left off.
    Based on the session memory, the FastAPI migration is complete and deployed.
    The next task is the Next.js Frontend Migration...
```

### Phase 2: Architect & Plan

**Command:** `/architect`

For significant work, generate a technical specification before implementation. The architect persona produces structured documents with:
- Executive summary and scope
- Visual diagrams (ASCII C4 format)
- Component specifications with interfaces
- API contracts
- Security and resilience considerations
- Phased implementation plan

**Key output:** A comprehensive plan that answers "what" and "why" — not "how."

### Phase 2b: Design the Experience (When UI is Impacted)

**Command:** `/design`

When the feature involves user interface changes, invoke the UX Oracle after architecture but before critique. This phase produces an "Experience Manifesto" that goes beyond standard UI patterns.

**The Oracle Protocol (Five-Pass Synthesis):**
1. **JTBD Deep-Dive** — Identify the core transformation the user is hiring the product for
2. **Shadow Data Extraction** — Find latent data the user hasn't connected
3. **Second-Order Prediction** — Anticipate the next friction point
4. **Cognitive Load Mapping** — Identify decision paralysis moments
5. **Pattern Rebellion** — Challenge standard UI patterns that are insufficient

**Key outputs:**
- The "Unspoken" Job (emotional/functional transformation)
- Information Architecture ("The Wisdom Pipeline")
- Novel Interaction Model (zero-UI proposals, spatial canvas)
- The "MVP of Delight" (the surprising feature)

**Anti-Mediocrity Guardrails:**
- If it looks like a standard SaaS app, start over
- High-density views must feel like a "Command Center," not "Visual Noise"
- Action-gating: Don't touch code until the user says "Manifest"

**When to use:** Any feature that changes what users see or interact with. Skip for pure backend/API work.

### Phase 3: Critique the Plan

**Command:** `/critique`

Before generating tickets, turn the AI against its own plan. The critique phase identifies:
- Gaps and unstated assumptions
- Brittle design elements
- Code smells (even in the plan)
- Missing error handling
- Security concerns
- Performance issues
- Anti-patterns (see below)

**Anti-Patterns to Flag:**
| Anti-Pattern | Description |
|--------------|-------------|
| **Gold-Plating** | Building features "just in case" (YAGNI violation) |
| **Abstractions for One** | Generic interfaces for a single implementation |
| **Inner Platform Effect** | Re-implementing framework/OS features |
| **Locked File Modification** | Changes to protected `.lockedfiles` |
| **Vague Errors** | Catch-all blocks without specific logging |
| **Bloat** | DRY/SOLID violations — but also "Over-DRYing" (complex abstractions to avoid 3 lines of repetition) |

**Example critique output:**
```markdown
### Gap 1: No Error Boundary
The plan doesn't mention React Error Boundaries. A JavaScript error in any
component could crash the entire app.

### Gap 2: No Request Cancellation
API calls don't mention AbortController. Rapid navigation could cause
race conditions and state updates on unmounted components.

### Anti-Pattern: Abstraction for One
The proposed "BaseAPIClient" class wraps fetch() but only has one
implementation. This adds indirection without value — use fetch directly.
```

### Phase 4: Revise with Best Practices

**Command:** `/revise`

Address every critique item with industry best practices. The revision phase:
- Does NOT write implementation code
- Specifies patterns and approaches
- Explains trade-offs between alternatives
- Updates the architectural plan with new requirements

**Key instruction:** *"No half-assed solutions. Don't be lazy, I want your absolute best effort."*

### The Critique-Revise Loop

Phases 3 and 4 often iterate multiple times before the plan is ready for tickets:

```
/architect → /critique → /revise → /critique → /revise → (approved) → /tickets
```

Each iteration strengthens the plan:
- **First critique:** Catches obvious gaps and missing error handling
- **First revision:** Adds patterns and approaches
- **Second critique:** Challenges the proposed solutions, finds edge cases
- **Second revision:** Refines approaches, addresses edge cases
- **Continue until:** No significant gaps remain

**When to stop iterating:** The plan is ready for tickets when critique produces only minor or stylistic concerns, not structural issues.

### Phase 5: Decompose into Tickets

**Command:** `/tickets`

Only after the plan has survived critique-revise cycles, convert it into prioritized, actionable tickets. Each ticket is:
- Self-contained (can be implemented independently)
- Vertically sliced (API + Frontend together)
- Prioritized (P0 = critical path, P1 = core, P2 = polish)
- Testable (includes acceptance criteria)

**Example ticket structure:**
```markdown
## P0-001: Project Scaffold
**Priority:** P0 (Critical Path) | **Effort:** 2 hours

### Objective
Initialize the Vite + React + TypeScript project with tooling.

### Technical Requirements
- Vite 5.x with React 18+ plugin
- TypeScript 5.x strict mode
- ESLint + Prettier configuration
- Path aliases (@/ → src/)

### Acceptance Criteria
- [ ] `npm run dev` starts development server
- [ ] `npm run build` produces production bundle
- [ ] `npm run test` runs test suite
```

### Phase 6: Implement

**Command:** `/code`

With an approved, critiqued, and revised plan, implementation begins. The coding agent follows a strict TDD discipline:

**The TDD Process:**
1. **Analyze & Challenge** — Study specs, identify gaps, ask questions before starting
2. **Draft Tests First** — Define test cases that validate all requirements BEFORE writing implementation
3. **Build Resilient Code** — Clean, SOLID, idiomatic code with defensive design
4. **Full Suite Verification** — Run ENTIRE test suite (backend + frontend), must pass with zero warnings

**Core Principles:**
- **Mandatory TDD**: No production code without unit, integration, or E2E tests
- **Zero-Bug Tolerance**: Handle all failure modes; tests must pass with zero warnings
- **Truth in Tests**: "Production code is a LIABILITY without tests"
- **Security First**: OWASP Top 10 and zero-trust principles

**Standalone Test Verification:** `/test`

At any point, run `/test` to verify all tests are "green and clean":
- Runs all frontend and backend tests
- Any failures must be fixed properly (not hacked around)
- Tests that no longer reflect project state should be updated, not deleted

**Continuation pattern:** For multi-ticket implementations, use simple "continue" messages:
```
User: /code
AI: [Implements P0-001 scaffold]
    Ready for P0-002?

User: continue

AI: [Implements P0-002]
    Ready for P1-001?

User: finish all the tickets, don't stop till the code work is done.

AI: [Implements all remaining tickets in sequence]
```

### Phase 7: Review

**Command:** `/review`

After implementation is complete, conduct a formal code review. The review produces a structured report:

```markdown
# Code Review Report

## Summary of Findings
| Severity | Count |
|----------|-------|
| Critical | 0     |
| High     | 1     |
| Medium   | 3     |
| Low      | 4     |

## High Severity Issues
### H-1: Inconsistent Tenant Enforcement
**File:** routers/sessions.py
**Description:** Sessions router lacks tenant validation present in AI router.
**Recommendation:** Add `enforce_tenant_allowed` dependency to router.

## Health Score: 8.5/10
```

### Phase 8: Fix

**Command:** `/fix`

Remediate all issues from the review. The fix agent:
- Checks for `.lockedfiles` before modifying anything
- Fixes issues systematically by severity
- Runs full test suite after fixes
- Produces a remediation summary

**Output:** Confirmation that all issues are resolved, with verification steps.

### Phase 9: Release

After fixes pass review, prepare for release:

1. **Commit** with conventional commit message
2. **Tag** the release with annotated tag including release notes
3. **Save context** with `/memory` for future sessions
4. **Deploy** (separate from commit — allows review before production)

**Example release sequence:**
```
User: commit your code and tag the tip for release. Do not deploy yet.

AI: [Runs pre-commit hooks]
    [Creates commit: feat: add React frontend with security hardening]
    [Creates tag: v2.0.0]
    Done. Tomorrow's deployment: ./deploy-container.sh v2.0.0
```

### Phase 10: The Debug Loop (Sherlock Protocol)

**Command:** `/sherlock`

Even with rigorous orchestration, runtime issues occur. The Debug Loop provides a systematic protocol for Root Cause Analysis (RCA) when verification fails.

**The Protocol:**
1.  **Isolate:** Create a minimal reproduction case.
2.  **Investigate:** Use `/sherlock` to trace logs, state, and dependencies.
3.  **Triage:**
    *   **Minor Fix:** Direct remediation via `/fix` → `/review` → `/test`.
    *   **Major Flaw:** Escalation back to planning via `/architect` → `/critique`.

**Branching Logic:**
```
            ┌→ [Minor] → /critique → /revise → /fix → /test
/sherlock ──┤
            └→ [Major] → /architect → /critique → /revise → ...
```

**Key Rule:** Never apply a "blind fix". If `/sherlock` cannot explain *why* it broke, do not run `/fix`.

### Protocol Theory: The "/sherlock" Directive

> "Why cast the AI as **Sherlock Holmes**? Because adopting a persona intentionally *unrelated* to software development acts as a cognitive power up."

It forces the model to abandon its training to apply "standard fixes" and instead unlocks a **relentless, deductive mode of reasoning** that standard engineering prompts cannot reach.

When the investigation hits a dead end, the "Detective" doesn't just give up—he instruments the crime scene. He suspects every variable, refuses to guess, and **enlists Watson's help (the User)** when standard deductive processes yield no answers and more data is required.

**"It is a capital mistake to theorize before one has data."**

---

## Key Principles in Practice

### 1. Explicit Approval Gates

The workflow has natural pause points where the human reviews AI output:
- After planning: "Does this plan make sense?"
- After critique: "Did we catch everything?"
- After revision: "Are these solutions appropriate?"
- After implementation: "Is this ready for review?"
- After review: "Should we fix all these issues?"
- After fix: "Is this ready to ship?"

### 2. Context as Contract

The `.tmp/SESSION_MEMORY.md` file is a contract between sessions. It contains:
- Project overview and architecture
- Completed and pending work
- Critical knowledge (the "gotchas")
- Quick start commands
- Resume instructions

This allows multi-day or multi-week projects without context loss.

### 3. Quality Over Speed

The workflow prioritizes shipping correct code over shipping fast:
- Every implementation gets reviewed
- Every review finding gets addressed
- Tests must pass before commits
- Pre-commit hooks enforce formatting

### 4. Command Specialization

Each command invokes a different "expert persona" with specific:
- Focus areas (security, performance, UX)
- Output formats (specs, tickets, reports)
- Quality standards (no placeholders, complete implementations)

---

## Adapting the Workflow

### For Smaller Tasks
Skip `/architect` and `/tickets` for simple bug fixes. Use:
```
/resume → /code → /review → /fix → commit
```

### For Backend-Only Work
Skip `/design` when no UI is affected:
```
/resume → /architect → /critique ⇄ /revise → /tickets → /code → ...
```

### For UI-Heavy Features
Include `/design` after architecture:
```
/resume → /architect → /design → /critique ⇄ /revise → /tickets → /code → ...
```

### For Exploratory Work
Replace `/code` with `/architect` variations:
```
/resume → /architect → /critique → /revise → (decision point)
```

### For Debugging
Use specialized debugging commands:
```
/sherlock → (investigation) → /fix → /review
```


---

## Protected Files: `.lockedfiles`

The `.lockedfiles` mechanism prevents AI agents from modifying code that has been vetted and should remain stable.

### What It Is

A `.lockedfiles` file in the project root contains a list of file paths (one per line) that should not be modified by AI agents without explicit human approval.

```
# .lockedfiles example
src/core/auth.py
src/api/contracts.ts
migrations/*.sql
```

### How It Works

1. **Before any write operation**, agents check if the target file matches a pattern in `.lockedfiles`
2. **If matched**, the agent **must pause** and request a "Manual Override" from the user
3. **The user explicitly approves** the modification, or the agent skips that change
4. **Override is logged** in the remediation summary for audit trail

### When to Lock Files

| Lock When... | Example |
|--------------|---------|
| Security-critical code | Authentication, authorization, encryption |
| API contracts | Public interfaces that external systems depend on |
| Database migrations | Schema changes that can't be easily rolled back |
| Configuration | Production environment configs |
| "Known good" implementations | Code that took significant effort to get right |

### Commands That Check `.lockedfiles`

- **`/critique`** — Flags if the plan would modify locked files
- **`/review`** — Warns if locked files were modified
- **`/fix`** — Pauses before modifying locked files, requires override

### Why This Matters

Without this protection, an AI agent optimizing for "clean code" might refactor a subtle security implementation, or an agent fixing a bug might inadvertently break a carefully-tuned algorithm. The lock file creates a human checkpoint for high-risk changes.

---

## Command File Structure

Commands are stored in `.cursor/commands/` as markdown files. Each file:
1. Defines a persona with role and focus
2. Lists operational sub-commands
3. Specifies output formats and schemas
4. Includes anti-patterns to avoid

Example structure:
```
.cursor/commands/
├── architect.md     # Principal Architect persona
├── code.md          # Principal Engineer persona
├── critique.md      # Devil's Advocate persona
├── fix.md           # Senior Staff SRE persona
├── memory.md        # Context Manager persona
├── resume.md        # Context Loader (simple)
├── review.md        # Senior Staff Engineer persona
├── revise.md        # Senior Designer persona
└── tickets.md       # Technical PM persona
```

---

## Results

This workflow produced:
- **80 files** committed in a single feature branch
- **10,931 lines** of new/modified code
- **73 tests** (53 backend + 20 frontend), all passing
- **Zero TODOs** or placeholders in production code
- **Annotated release tag** with comprehensive release notes

The entire React frontend migration, including security hardening of the FastAPI backend, was completed in a single extended session with multiple `/review` → `/fix` cycles until the health score reached 9.0/10.

---

## Conclusion

This workflow treats AI as a highly capable but supervised team member. The human provides:
- Strategic direction
- Approval at key gates
- Final accountability

The AI provides:
- Deep technical analysis
- Consistent implementation quality
- Structured documentation
- Self-critique and revision

The result is production-grade code with thorough documentation, comprehensive tests, and a clear audit trail of design decisions.
