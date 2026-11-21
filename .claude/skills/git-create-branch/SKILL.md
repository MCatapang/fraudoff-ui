---
description: Create a new git branch following the fraudoff-ui branch naming convention (feature/, fix/, chore/, release/).
argument-hint: [branch description or purpose]
---

Create a new git branch following the fraudoff-ui branch naming convention.

**Arguments:** $ARGUMENTS (optional — description of the branch purpose)

**Branch Naming Convention:**
```
feature/<short-description>   # new features
fix/<short-description>        # bug fixes
chore/<short-description>      # tooling, deps, docs
release/<version>              # release prep
```

Rules:
- Use lowercase and hyphens only (no spaces, underscores, or slashes beyond the prefix)
- Keep `<short-description>` to 2–4 words
- Examples: `feature/phishing-school-page`, `fix/nav-dropdown-mobile`, `chore/upgrade-tailwind`

**Steps:**

1. If $ARGUMENTS is provided, infer the branch type and description from it. Otherwise ask the user:
   - **Type**: feature / fix / chore / release
   - **Description**: 2–4 word summary (will be slugified automatically)

2. Slugify the description: lowercase, replace spaces with hyphens, strip special characters.

3. Show the proposed branch name and confirm with the user.

4. Create and switch to the branch:
```bash
git checkout -b <type>/<slugified-description>
```

5. Confirm the active branch with `git status`.
