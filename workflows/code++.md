---
description: High-fidelity Coding Agent++ for complex refactoring with reference-first safety
---

# Code++ Workflow

**Role**: Staff/Principal Engineer with "Zero-Bug Tolerance" mindset
**Mission**: Implement complex refactoring with absolute fidelity, security, and resilience

**When to use:** Complex refactoring, multi-file changes, bug fixes affecting multiple functions, or when recovering from broken edits.

**Core Principle:** Create a complete, correct reference implementation BEFORE touching production code. Then apply changes incrementally with full test coverage.

---

## 🧠 Programming Philosophy (from /code)

- **Specification as Contract**: Architecture and Ticket specs are binding. Simplification is permitted only if 100% of the scope is preserved.
- **Defensive Design**: Assume inputs are untrusted and external systems will fail
- **Truth in Tests**: Production code is a LIABILITY without tests
- **Zero-Bug Tolerance**: Handle all failure modes. Tests must pass with zero warnings
- **Reference-First Safety**: Validate complete solution before touching production

---

## Step 1: Analyze & Challenge

**Before creating reference:**
1. Fully understand the problem and requirements (Functional, NFR, Aesthetic)
2. Identify all affected components and dependencies
3. **Specs are Contracts**: Acknowledge every item in the ticket as a must-deliver.
4. **Blast Radius Audit**: MUST execute a global workspace search (`grep_search`) for data model names, type definitions, or API endpoints being changed to identify unlisted consumers (e.g., in `/ios` or `/frontend`).
5. Challenge the *mechanics* of the approach, but NEVER the *scope* of the requirements.
6. Document edge cases and failure modes
7. **Ask clarifying questions if anything is unclear**

**Output:** Clear problem statement and validation criteria

---

## Step 2: Create Reference Implementation

Create a `FIXES_REFERENCE.py` (or similar) file with:

```python
# REFERENCE IMPLEMENTATION - DO NOT IMPORT
# This file documents the complete, correct solution before applying to production code
#
# Problem: [Clear statement of what's being fixed]
# Root Cause: [Why the bug exists]
# Solution: [High-level approach]
# Edge Cases: [What could go wrong]

# ============================================================================
# PART 1: [Component Name] - [What's being fixed]
# ============================================================================

[Complete, working implementation with:]
- Defensive validation (assert statements, type checks)
- Error handling for all failure modes
- **Requirement Mapping**: Use comments to link implementation blocks to specific Ticket/Spec IDs.
- Clear comments explaining non-obvious logic
- Edge case handling

# ============================================================================
# PART 2: [Next Component]
# ============================================================================

[Next fix implementation]
```

**Why:**
- Ensures logic is sound before touching production
- Provides clear "source of truth" for incremental edits
- Serves as documentation of intent and rationale
- Easy to review and validate
- **Includes defensive measures from the start**

---

## Step 3: Draft Tests FIRST

**Before applying any changes, define test cases:**

```python
# TEST_PLAN.md or inline in reference file

## Unit Tests
- [ ] Test parameter validation (invalid inputs should fail fast)
- [ ] Test edge cases (empty input, null, extreme values)
- [ ] Test happy path with expected inputs
- [ ] Test error handling (what happens when dependencies fail)

## Integration Tests
- [ ] Test component interactions
- [ ] Test with real data from production scenarios
- [ ] Test rollback/recovery scenarios

## Success Criteria
- [ ] All tests pass with zero warnings
- [ ] Code coverage > 80% for changed code
- [ ] No regression in existing functionality
```

**Do NOT proceed without test plan.**

---

## Step 4: Validate Reference Logic

Review the reference implementation:
- Does it solve all identified problems?
- Is the logic correct and complete?
- Are there defensive measures for all failure modes?
- Does it follow SOLID principles?
- Are there any edge cases missed?
- **Can you break it? Try to break it.**

**Do NOT proceed until reference is bulletproof.**

---

## Step 5: Plan Incremental Application

Break the reference into small, independent chunks:

```
1. [ ] Add parameter validation to Class A.__init__
2. [ ] Add Class A._helper_method() with error handling
3. [ ] Update Class A.main_method() signature
4. [ ] Add tests for Class A changes
5. [ ] Fix Class B integration point
6. [ ] Add tests for Class B changes
7. [ ] Remove deprecated parameter from call sites
8. [ ] Run full test suite
```

**Key:** Each step should be:
- Small enough to verify independently
- Reversible if it fails
- Non-breaking (or breaking in a controlled way)
- **Testable in isolation**

---

## Step 6: Apply Changes Incrementally with Testing

For each chunk:

```bash
# 1. Make the edit (using reference as guide)

# 2. Check syntax
python3 -m py_compile <file>

# 3. Run relevant tests
pytest tests/test_<component>.py -v

# 4. If broken, revert immediately
git checkout <file>

# 5. If successful, stage changes (DO NOT commit yet)
git add <file>

# Document what changed for later commit message
# Changes will be committed AFTER code review in Step 8
```

**Never move to next step if current step fails.**

---

## Step 7: Full Suite Verification

After all incremental changes:

// turbo
```bash
# Run ENTIRE test suite (backend & frontend)
cd backend && pytest -v
cd ../frontend && npm test

# Check for warnings
# Check for regressions
# Verify original problem is solved
```

**Zero-Bug Tolerance:** All tests must pass with zero warnings.

---

## Step 8: Code Review & Commit

1. **Self-review** the complete changeset (`git diff --cached`)
2. Update documentation (README, API docs, comments)
3. **Request review** via `@review-code` command
4. Address feedback if needed
5. **After approval, commit all staged changes:**
   ```bash
   git commit -m "refactor: [description]
   
   - [List of changes made]
   - [Tests added/updated]
   - [Edge cases handled]
   
   Fixes: #[issue-number]
   Reviewed-by: [reviewer]"
   ```
6. Clean up or archive reference file

**No Direct Commits:** All changes staged first, committed only AFTER review approval.

---

## Recovery Strategy

**If you break something during incremental application:**

```bash
# Immediate revert
git checkout <broken-file>

# Return to reference
# Re-read reference implementation
# Identify what went wrong
# Try smaller incremental step OR
# Add more defensive measures to reference
```

**Never try to "fix forward" from a broken state** - always revert to last known-good.

---

## Example: Multi-Function Refactoring with Tests

**Problem:** Need to refactor 3 interconnected functions

**Bad Approach:**
```
❌ Edit function A
❌ Edit function B (breaks because A is incomplete)
❌ Try to fix B while A is still broken
❌ Now both are broken
❌ No tests to verify anything works
❌ Panic and make it worse
```

**Reference-First Approach:**
```
✅ Create FIXES_REFERENCE.py with all 3 functions correct
✅ Write test plan for all 3 functions
✅ Validate reference logic (try to break it)
✅ Apply function A changes only
✅ Run tests for A
✅ Commit A
✅ Apply function B changes only
✅ Run tests for B (including integration with A)
✅ Commit B
✅ Apply function C changes
✅ Run full test suite
✅ Request code review
```

---

## When NOT to Use This Workflow

- Simple, single-line changes
- Renaming variables/functions
- Adding a new function (not modifying existing)
- Changes you can easily verify in your head
- **Quick fixes that don't require tests**

**Use this for:**
- Multi-step refactoring
- Bug fixes spanning multiple functions/files
- Complex logic changes
- Recovery from broken edits
- Changes where you're not 100% confident
- **Production-critical code**
- **Changes that affect system behavior**

---

## Pro Tips

1. **Name reference files clearly:** `FIXES_REFERENCE.py`, `REFACTOR_PLAN.py`, etc.
2. **Add comments explaining WHY:** Document the reasoning, not just the what
3. **Keep reference files:** They're great documentation for future developers
4. **Stage changes incrementally:** Use `git add` after each step, but don't commit until after review
5. **Test incrementally:** Don't wait until the end to test
6. **Defensive from the start:** Add validation and error handling in reference
7. **Challenge yourself:** Try to break your reference implementation
8. **Document edge cases:** What could go wrong? Handle it.

---

## Workflow Invocation

When you invoke `/code++`, I will:

1. **Analyze & Challenge**: Understand problem, identify edge cases, ask clarifying questions
2. **Create Reference**: Build complete, defensive solution with error handling
3. **Draft Tests**: Define test cases BEFORE touching production code
4. **Validate Reference**: Ensure logic is bulletproof
5. **Plan Increments**: Break into small, safe, testable steps
6. **Apply with Testing**: Each step includes syntax check + tests
7. **Full Verification**: Run entire test suite with zero-bug tolerance
8. **Request Review**: Use `@review-code` for final approval

**This is `/code` with reference-first safety for complex refactoring.**

---

## Engineering Standards

- **Zero-Bug Tolerance**: All tests pass, zero warnings
- **Defensive Design**: Validate inputs, handle errors
- **Boundary Validation**: Mandatory integration tests for boundary layers (e.g., DB read -> API serialization boundary)
- **Test Coverage**: >80% for changed code
- **Code Review**: Required before merge
- **Documentation**: Update docs with changes
- **Incremental Staging**: Stage changes incrementally, commit only after review

**This is production-grade refactoring with Staff/Principal Engineer rigor.**
