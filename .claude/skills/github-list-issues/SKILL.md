---
description: List open GitHub issues, optionally filtered by label or milestone. Groups results by label for a quick overview.
argument-hint: [label or milestone name]
---

List open GitHub issues in the fraudoff-ui repository.

**Arguments:** $ARGUMENTS (optional — label name or milestone to filter by)

**Steps:**

1. List open issues. Apply filters from $ARGUMENTS if provided:

   No filter:
   ```bash
   gh issue list --state open --limit 50
   ```

   Filter by label:
   ```bash
   gh issue list --state open --label "$ARGUMENTS" --limit 50
   ```

   Filter by milestone:
   ```bash
   gh issue list --state open --milestone "$ARGUMENTS" --limit 50
   ```

2. Group and display the results by label:
   - `bug` issues
   - `feature` issues
   - `chore` / `docs` issues
   - `blocked` issues
   - Unlabeled

3. For each issue show: number, title, label(s), and age (days since opened).

4. Offer to open a specific issue for full details:
```bash
gh issue view <number>
```
