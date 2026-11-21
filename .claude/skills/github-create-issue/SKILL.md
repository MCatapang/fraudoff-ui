---
description: Create a new GitHub issue for the fraudoff-ui repository with a structured description and label.
argument-hint: [issue title or description]
---

Create a new GitHub issue for the fraudoff-ui repository.

**Arguments:** $ARGUMENTS

**Steps:**
1. If $ARGUMENTS is provided, use it as the basis for the issue title and description. Otherwise ask the user for:
   - **Title** — concise, imperative (e.g. "Add dark mode to settings page")
   - **Description** — what needs to be done and why; include acceptance criteria if applicable
   - **Label** — choose from: `bug`, `feature`, `chore`, `docs`, `security`, `ux`, `blocked`

2. Format the issue body as:
```
## Description
<what and why>

## Acceptance Criteria
- [ ] <criterion>

## Notes
<any additional context>
```

3. Run:
```bash
gh issue create --title "<title>" --body "<formatted body>" --label "<label>"
```

4. Output the created issue number and URL.
