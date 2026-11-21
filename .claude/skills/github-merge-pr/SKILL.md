---
description: Merge a pull request using squash strategy. Confirms mergeability and checks before proceeding — always requires user confirmation.
argument-hint: <pr-number>
---

Merge a pull request in the fraudoff-ui repository using squash strategy.

**Arguments:** $ARGUMENTS (PR number required)

**Steps:**

1. Fetch PR status to confirm it is mergeable:
```bash
gh pr view $ARGUMENTS --json number,title,state,mergeable,statusCheckRollup,reviews
```

2. Display the PR title, current state, check status, and review summary to the user.

3. Confirm with the user before proceeding — this action is irreversible.

4. Merge using squash strategy with a clean commit message:
```bash
gh pr merge $ARGUMENTS --squash --delete-branch
```
   The squash commit message should follow this format:
   ```
   <PR title> (#<PR number>)

   <one-sentence summary of changes>
   ```

5. After merge, confirm the branch was deleted and output the merge commit SHA.

6. Remind the user to pull `main` locally:
```bash
git checkout main && git pull
```
