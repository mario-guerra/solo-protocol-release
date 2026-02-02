# GitHub Copilot Instructions

This project follows the **SOLO** AI-assisted development workflow.

## 🤖 Available Commands (Prompt Library)
The project-wide prompt library is located in the `workflows/` directory. When completing tasks, refer to these templates as the primary source of truth for personas and standards:

- **Strategy**: `workflows/prd.md`, `workflows/marketing.md`, `workflows/growth.md`
- **Design**: `workflows/architect.md`, `workflows/ui-design.md`, `workflows/ui-feature.md`
- **Execution**: `workflows/tickets.md`, `workflows/code.md`, `workflows/payments.md`
- **Quality**: `workflows/review.md`, `workflows/fix.md`, `workflows/obs.md`, `workflows/hardening.md`
- **Orchestration**: `workflows/memory.md`, `workflows/resume.md`

## 📜 Core Standards
- **Plan First**: Always request or generate a plan before writing code.
- **TDD First**: Write tests alongside or before implementation.
- **Zero-Bug Mindset**: Handle all edge cases and failure modes.
- **API-First**: Respect the common API contract for all clients.

## 📂 Session Persistence
Use `workflows/memory.md` to generate or read session context from `.tmp/SESSION_MEMORY.md` to maintain continuity between tasks.
