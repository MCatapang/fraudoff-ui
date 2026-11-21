---
description: Close a GitHub issue with a reason comment. Use when marking an issue as completed, not planned, or duplicate.
argument-hint: <issue-number>
---

Close a GitHub issue in the fraudoff-ui repository with a reason comment.

**Arguments:** $ARGUMENTS (issue number required)

**Steps:**

1. Fetch the issue to display its title and current state:
```bash
gh issue view $ARGUMENTS
```

2. Ask the user for a closing reason if not already provided:
   - **completed** — work was done and merged
   - **not planned** — won't fix / out of scope
   - **duplicate** — duplicate of another issue (ask for the duplicate issue number)

3. Compose a closing comment based on the reason. Examples:
   - Completed: "Resolved in PR #<number>. Closing as completed."
   - Not planned: "Closing as out of scope — <brief explanation>."
   - Duplicate: "Duplicate of #<number>. Closing in favor of the original."

4. Confirm with the user before closing — this modifies a remote resource.

5. Post the comment and close:
```bash
gh issue comment $ARGUMENTS --body "<closing comment>"
gh issue close $ARGUMENTS --reason "<completed|not planned|duplicate>"
```

6. Confirm the issue is closed and output the URL.
