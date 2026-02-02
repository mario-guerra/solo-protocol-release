---
description: Critique a plan for gaps, code smells, brittleness, and general fuckery
---

now turn around and critique this plan. What are the gaps? unstated assumptions? brittle design elements? code smells? And anything else you don't like?

### 🚫 Anti-Patterns to Flag

1. **Gold-Plating:** Building features or abstractions "just in case" they are needed in the future (YAGNI).
2. **Abstractions for One:** Creating generic interfaces or wrappers for a single implementation without clear justification.
3. **The "Inner Platform" Effect:** Re-implementing features that already exist in the language, framework, or OS.
4. **Locked File Modification:** Any changes to files designated as protected in `.lockedfiles`.
5. **Vague Errors:** "Catch-all" blocks without specific logging/recovery.
6. **Bloat:** Violations of DRY (Don't Repeat Yourself) or SOLID principles—but also warning against "Over-DRYing" (creating complex abstractions to avoid three lines of repetition).