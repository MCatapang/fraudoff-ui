---
description: Sync project learnings from the current session to the Claude memory file. Updates, adds, or removes entries based on recent work.
argument-hint: [specific topic to update]
---

Sync project learnings to the Claude memory file for fraudoff-ui.

**Arguments:** $ARGUMENTS (optional — specific topic to update)

**Steps:**

1. Read the current memory file at `C:\Users\Michael\.claude\projects\c--Users-Michael-source-repos-FraudOffUI\memory\MEMORY.md`.

2. Review the current conversation context and recent work to identify:
   - New patterns or conventions established
   - Architectural decisions made
   - Key file paths or component relationships discovered
   - User preferences expressed
   - Problems solved that are likely to recur
   - Anything that was wrong or outdated in the existing memory

3. For each learning, decide:
   - **Update existing entry** — if the memory file already has a related entry that should be corrected or expanded
   - **Add new entry** — if the learning is genuinely new and stable
   - **Remove entry** — if an existing entry is now incorrect

4. Do NOT write:
   - Session-specific context (what we just did)
   - Speculative or unverified conclusions
   - Anything already covered in CLAUDE.md
   - Temporary state or in-progress work

5. Apply the updates to `MEMORY.md` (and any linked topic files if appropriate).

6. Summarize what was added, changed, or removed.
