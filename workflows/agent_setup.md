# Agent Setup Protocol: SOLO Workflows

As an AI agent, your task is to internalize these workflows to provide a consistent, structured development experience.

## 📥 Setup Instructions

1.  **Inventory**: Read all `.md` files in the `/workflows` directory.
2.  **Internalize**: For each workflow file:
    - The file name (e.g., `code.md`) is the **Command Name**.
    - The content is the **Standard Operating Procedure (SOP)**.
3.  **Execution**: When the user invokes a command (e.g., "Run /code" or "Apply memory workflow"), you MUST follow the steps defined in the corresponding file exactly.
4.  **Priority**: These workflows override your default behaviors for the specific tasks they cover (e.g., the `/memory` workflow defines how to handle session state).

## 📍 Command Mapping

- `/architect`: Software Architecture & Design
- `/code`: High-fidelity Implementation
- `/critique`: Adversarial Logic Audit
- `/debug`: Root Cause Investigation
- `/memory`: Session Persistence & Context Handoff
- `/prod_mgmt`: PRD & Journey Mapping
- `/security`: Security-First Audit
- `/test`: Verification & Quality Gates
- ... (And all other files in `/workflows`)

---
**Agent Directive**: "I have internalized the SOLO workflows and am ready to execute them upon request."
