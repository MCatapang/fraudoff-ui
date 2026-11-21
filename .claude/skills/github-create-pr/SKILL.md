---
description: Create a pull request for the current branch. Reviews changes, drafts a summary, and opens the PR against main.
argument-hint: [pr title or related issue number]
---

Create a pull request for the current branch in the fraudoff-ui repository.

**Arguments:** $ARGUMENTS

**Steps:**

1. Run `git status` and `git log main..HEAD --oneline` to understand what is being submitted.

2. Run `git diff main...HEAD` to review all changes included in the PR.

3. Determine the base branch (default: `main`).

4. If $ARGUMENTS provides a title or issue number, use it. Otherwise derive a concise PR title from the commits (imperative, ≤70 chars).

5. Draft the PR body using this template:
```markdown
## Summary
- <bullet points describing what changed and why>

## Related Issues
Closes #<issue number if known>

## Test Plan
- [ ] <manual or automated test step>

## Screenshots (if applicable)

🤖 Generated with Claude Code
```

6. If the branch has no upstream, push it first:
```bash
git push -u origin <current-branch>
```

7. Create the PR:
```bash
gh pr create --title "<title>" --body "<formatted body>" --base main
```

8. Output the PR number and URL.
