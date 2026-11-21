---
description: Generate a changelog from commits since the last tag and draft a GitHub Release. Categorizes commits by type.
argument-hint: [target version tag, e.g. v1.0.0]
---

Generate a changelog from commits since the last tag and draft a GitHub Release.

**Arguments:** $ARGUMENTS (optional — target version tag, e.g. "v1.0.0")

**Steps:**

1. Find the last release tag:
```bash
git describe --tags --abbrev=0
```

2. Get all commits since that tag:
```bash
git log <last-tag>..HEAD --oneline --no-merges
```

3. Categorize commits into sections using conventional commit prefixes:
   - **Features** — `feat:` commits
   - **Bug Fixes** — `fix:` commits
   - **Chores / Maintenance** — `chore:`, `refactor:`, `build:` commits
   - **Documentation** — `docs:` commits
   - **Other** — anything that doesn't fit the above

4. Draft release notes in this format:
```markdown
## What's Changed

### Features
- <description> (#<PR number if known>)

### Bug Fixes
- <description>

### Chores
- <description>

**Full Changelog:** https://github.com/<owner>/<repo>/compare/<last-tag>...<new-tag>
```

5. Determine the new version tag from $ARGUMENTS or suggest one based on change severity (patch / minor / major).

6. Ask the user to confirm the release notes and version tag before creating the release.

7. Create the GitHub Release (confirm before running):
```bash
gh release create <new-tag> --title "<new-tag>" --notes "<release notes>"
```
