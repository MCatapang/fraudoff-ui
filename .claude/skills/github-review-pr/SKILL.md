---
description: Review a pull request — fetches diff and comments, analyzes for bugs and quality issues, and optionally posts the review to GitHub.
argument-hint: <pr-number>
---

Review a pull request in the fraudoff-ui repository.

**Arguments:** $ARGUMENTS (PR number required)

**Steps:**

1. Fetch the PR details:
```bash
gh pr view $ARGUMENTS --json number,title,body,author,baseRefName,headRefName,additions,deletions,changedFiles,state,labels
```

2. Fetch the diff:
```bash
gh pr diff $ARGUMENTS
```

3. Fetch any existing review comments:
```bash
gh api repos/:owner/:repo/pulls/$ARGUMENTS/comments
```

4. Analyze the PR and produce a structured review covering:
   - **Summary**: What does this PR do? What is its stated purpose?
   - **Changes overview**: Files changed, net lines added/removed
   - **Concerns / issues found**: Bugs, missing error handling, accessibility gaps, security risks, performance problems, missing tests
   - **Suggestions**: Improvements that are not blocking but worth considering
   - **Verdict**: one of `APPROVE`, `REQUEST_CHANGES`, or `COMMENT`

5. Ask the user whether to post the review as a comment on GitHub or keep it local.

6. If posting:
```bash
gh pr review $ARGUMENTS --comment --body "<review body>"
```
