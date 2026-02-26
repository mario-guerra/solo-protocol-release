---
description: Critique a plan for gaps, code smells, brittleness, and general fuckery
---

## Phase 0: Pre-Flight Verification (before writing a single critique point)

**Do not critique claims you have not verified.** For every claim in the plan that references a type, function signature, runtime behavior, field nullability, import, or framework mechanic:

1. **Read the relevant source file** to confirm the claim is accurate
2. **Grep for imports and call sites** — confirm referenced functions/symbols are actually importable at the call location
3. **Trace every data flow end-to-end**: for each producer→transformer→consumer chain the plan touches, follow the data through every hop to its final destination and assert the type contract holds at each boundary
4. **Check `.lockedfiles`** — verify no proposed change touches a protected file without explicit approval noted in the plan

Only after completing these checks, proceed to critique.

---

## Critique the Plan

now turn around and critique this plan. What are the gaps? unstated assumptions? brittle design elements? code smells? And anything else you don't like?

### 🚫 Anti-Patterns to Flag

1. **Gold-Plating:** Building features or abstractions "just in case" they are needed in the future (YAGNI).
2. **Abstractions for One:** Creating generic interfaces or wrappers for a single implementation without clear justification.
3. **The "Inner Platform" Effect:** Re-implementing features that already exist in the language, framework, or OS.
4. **Locked File Modification:** Any changes to files designated as protected in `.lockedfiles`.
5. **Vague Errors:** "Catch-all" blocks without specific logging/recovery.
6. **Bloat:** Violations of DRY (Don't Repeat Yourself) or SOLID principles—but also warning against "Over-DRYing" (creating complex abstractions to avoid three lines of repetition).

### 🔍 Verification Checklist (flag any unverified claim)

- **Type signatures:** Every field type, nullability, and default value referenced in the plan has been read from source — not recalled from memory.
- **Imports:** Every symbol the plan says to import or use at a call site actually exists and is importable there. Confirmed by grep.
- **Framework mechanics:** Any claim about how a framework behaves (e.g., dependency injection, background task scheduling, exception propagation) is verified against the framework's actual behavior, not assumed.
- **Data flow completeness:** For every fix that moves data from A to B, the full chain A→...→B has been traced to confirm the data is never silently dropped before it reaches its intended consumer.
- **Test assertions match implementation:** Every test assertion in the plan reflects the actual expected value given the proposed implementation — not the value before the last revision.