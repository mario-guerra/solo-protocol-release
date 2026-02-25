---
description: 
---

Write a memory document to `.tmp/SESSION_MEMORY.md` for context handoff. You *must* use this exact file saved in this exact location, or your memory will be lost. This file will be your ONLY context when resuming work. 

### If `.tmp/SESSION_MEMORY.md` Already Exists
1. Read it first
2. Preserve still-relevant sections
3. Mark updated sections with `[UPDATED: YYYY-MM-DD]`
4. Move obsolete content to "## Archive" section at bottom
5. Delete archive entries older than 2 weeks

---

## Required Sections

Line budgets (approximate, ~500 total):
- Sections 1-2: ~30 lines
- Section 3: ~50 lines
- Sections 4-5: ~40 lines
- Section 6: ~150 lines (prioritize this - most valuable)
- Sections 7-8: ~50 lines
- Sections 9-12: ~80 lines
- Buffer: ~100 lines

---

### 1. Project Overview (~10 lines)
What is this codebase? Tech stack? Absolute path to project root?

### 2. Architecture (~20 lines)
- Key directories with purpose
- Entry points (main.py, App.tsx, etc.)
- Data flow between components

### 3. Completed Work (~50 lines, table format)
| Feature | Files Modified | Key Details |
|---------|----------------|-------------|
| Example | `path/to/file.py` | Brief description |

### 4. In-Progress/Blocked (~20 lines)
What's partially done? What's blocking?
- Include error messages if relevant
- Note which file/line you stopped at

### 5. Pending Work (~20 lines, prioritized)
What remains? Reference ticket files if they exist:
1. `@.v2_features/tickets/TICKET-001.md` - High priority description
2. Lower priority items...

### 6. Critical Knowledge (~150 lines)
Non-obvious discoveries that save re-investigation. Use severity markers:

**Severity Levels:**
- 🔴 **CRITICAL**: Will cause failures if forgotten
- 🟡 **IMPORTANT**: Saves significant time  
- 🟢 **HELPFUL**: Nice to know

**Categories:**
- **Gotchas**: Edge cases, things that look wrong but are correct
- **External Services**: API quirks, auth flows, data format surprises
- **Patterns**: What worked vs. what didn't (and why)
- **Data Relationships**: How IDs/keys connect across systems

Example format:
```
🔴 CRITICAL - S3 Customer ID Mismatch
The customer ID in session IDs (e.g., 000000000001002914) is PADDED.
The customer ID in AppLog CustId field (e.g., 1002914) is STRIPPED.
S3 paths use the STRIPPED version. Always get CustId from AppLog metadata.
File: backend/app/clients/audio_storage.py:57-78
```

### 7. Test Data (~30 lines)
Include timestamps - test data expires!

```
[As of YYYY-MM-DD]
Session IDs that work:
- <id> - has audio + applog data
- <id> - has applog only

Credentials file: backend/local.env (not committed)

Sample API calls:
curl -X GET "http://localhost:8000/api/..." -H "..."
```

### 8. Quick Start (~20 lines)
Exact commands to run from project root:

```bash
# Backend
cd <path>
source .venv/bin/activate
uvicorn app.main:app --reload --port 8000

# Frontend
cd <path>
npm run dev

# Verify working
curl http://localhost:8000/health
```

### 9. Git State (~15 lines)
```
Branch: <current branch>
Last commit: <hash> <message>
Uncommitted changes: <yes/no, brief description>
Remote sync: <ahead/behind/synced>
```

### 10. Related Memory Files (~15 lines)
Link to other context files:
- `.tmp/AUDIO_INTEGRATION_MEMORY.md` - S3 audio setup details
- `.tmp/OTHER_FILE.md` - Description

### 11. Resume Instructions (~25 lines)
First actions for next session:

```
1. READ FIRST:
   - <specific file to load for context>
   - <another critical file>

2. VERIFY STATE:
   - <command to check backend running>
   - <command to check frontend running>

3. CONTINUE FROM:
   - Task: <specific next task>
   - File: <file:line to start at>
   - Next step: <concrete action>
```

### 12. User Preferences (~25 lines)
Patterns established in this session:

```
Commits: Conventional commits (feat/fix/chore/docs/test)
Testing: Manual verification before commit
Python: snake_case, type hints, Pydantic models
TypeScript: camelCase, functional components, MUI
Error handling: <pattern used>
Logging: <pattern used>
Other: <any preferences expressed>
```

### 13. Lessons Learned (~20 lines)
Process improvements, debugging strategies, or workflow tweaks to carry forward.
- **Workflow**: e.g., "Always build before deploy"
- **Debugging**: e.g., "Use Sherlock mode for complex stale closures"

---

## What NOT to Include

- Standard library/framework behavior (obvious from docs)
- Code patterns obvious from reading the file
- Full file contents (use `file:line-range` references instead)
- Credentials, secrets, or PII (reference file location only)
- Speculative future work not captured in tickets
- Verbose explanations when a code snippet suffices

---

## Self-Verification Checklist

Before finalizing, verify:
- [ ] Can I restart backend/frontend with only Quick Start commands?
- [ ] Can I find the file for any feature mentioned? (paths are present and absolute)
- [ ] Are blocking issues actionable? (next step is clear)
- [ ] Is test data timestamped and likely still valid?
- [ ] Did I capture the ONE thing that would waste the most time if forgotten?
- [ ] Are Critical Knowledge items ranked by severity?
- [ ] Is Resume Instructions specific enough to start immediately?

---

## Archive

[Move obsolete sections here with date. Delete after 2 weeks.]