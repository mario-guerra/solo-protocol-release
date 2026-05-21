# 🌌 SOLO: Single Operator Loop Orchestration

SOLO is an industrial-grade framework for AI-first software engineering, designed to scale individual developers through rigorous orchestration and **Security-First** architecture.

[SOLO Protocol Blog Post](https://marioguerra.xyz/blog/the-solo-protocol/)

[SOLO Protocol Presentation](https://marioguerra.xyz/solo-protocol/)

## 🌟 Unified Command Architecture
This library uses a **Unified Command Architecture** that supports Antigravity, Cursor, and VSCode (GitHub Copilot). All agent templates live in the `workflows/` directory and are exposed to your IDE [...]

### 🛠 IDE Setup Guide

| IDE | Discovery Path | Setup Action |
|-----|----------------|--------------|
| **Antigravity** | `.agent/workflows/` | Copy/Symlink from `workflows/` |
| **Cursor** | `.cursor/commands/` | Copy/Symlink from `workflows/` |
| **VSCode (Copilot)** | Project Root | Use `.github/copilot-instructions.md` |

---

## 🏗 The SOLO Workflow
SOLO enforces a rigorous **"Plan First, Code Second"** loop.

### 1. Strategy & Planning
- `/prod_mgmt`: Transform vision into a high-fidelity PRD.
- `/marketing`: Define GTM and positioning.
- `/architect`: Design the system blueprint (ADD) and API contracts.
- `/security`: Design and implement the authentication and security layer.

### 2. The Critique Loop
- `/critique`: Challenge plans, find gaps, and identify anti-patterns.
- `/revise`: Update plans with industry best practices.

### 3. Execution & Quality
- `/tickets`: Decompose approved plans into actionable tickets.
- `/code`: Implement with TDD and zero-bug tolerance.
- `/review`: Conduct adversarial code audits.
- `/fix`: Remediate issues identified during review.

### 4. Forensic Investigation & Quality Assurance
- `/sherlock`: Deep-dive root cause analysis and forensic debugging.

---

## 🔍 The Forensic Investigation Loop

The **Sherlock workflow** provides systematic root-cause analysis for anomalies and bugs that survive code review. It enforces rigorous evidence-based deduction to eliminate guesswork.

### Sherlock: Forensic Root-Cause Analysis

The Sherlock agent operates as a consulting detective, following the principle: *"When you have eliminated the impossible, whatever remains, however improbable, must be the truth."*

**Key Capabilities:**
- `@investigate <symptom>`: Execute deep-dive forensic analysis into production bugs or behavioral anomalies.
- `@trace-evidence`: Perform systematic audits of logs, state transitions, and network traffic.
- `@eliminate-impossible`: Systematically validate or invalidate hypotheses through reproduction scripts and isolation tests.
- `@watson-critique`: Self-audit current deductions to identify logical gaps and missing evidence.

**Core Protocol:**
1. **Pure Observation**: Collect raw data (logs, stack traces, database snapshots, config) without premature theory formation.
2. **Multiple Hypotheses**: Document at least three potential root causes before investigation.
3. **Systematic Elimination**: Invalidate hypotheses through rigorous evidence cross-referencing.
4. **Logical Synthesis**: Explain *why* the bug exists, not just *where* it is.

**Deliverable:** A forensic case report that includes incident overview, evidence exhibits, chain of logic, and actionable prevention measures.

---

## 🤝 The Handoff Protocol
To maintain the highest fidelity, use the `@memory` command (driven by `workflows/memory.md`) to generate session handoffs. This allows your project to span multiple weeks or fresh chat sessions witho[...]

## 📜 Key Principles
1. **Plan First**: Never jump directly into implementation.
2. **Security-First**: Auth and security are designed before the first line of feature code.
3. **Trust But Verify**: Explicit approval gates between phases.
4. **Zero-Bug Tolerance**: All failure modes must be handled.
5. **API-First**: The contract is the source of truth.

---
*Built with ❤️ for advanced agentic coding workflows.*
