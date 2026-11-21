---
description: Draft a new coding standard and append it to docs/standards.md. Reads existing standards first to avoid duplication.
argument-hint: [topic, e.g. "error handling" or "naming conventions"]
---

Draft a new coding standard and append it to the fraudoff-ui standards document.

**Arguments:** $ARGUMENTS (optional — topic of the standard, e.g. "error handling" or "naming conventions")

**Steps:**

1. Read the existing standards doc at `docs/standards.md` to understand current content and avoid duplication.

2. Ask the user to describe:
   - **Topic**: What area does this standard cover?
   - **Rule(s)**: What is the standard? Be specific (do/don't, examples).
   - **Rationale**: Why does this standard exist? What problem does it prevent?

3. Draft the standard in this format:
```markdown
## <Topic>

**Rule:** <concise rule statement>

**Rationale:** <why this matters>

**Do:**
```<language>
// good example
```

**Don't:**
```<language>
// bad example
```
```

4. Show the drafted standard to the user for approval.

5. After approval, append it to `docs/standards.md` under the appropriate section (or create a new section if needed).

6. Confirm the section it was added to and the line range.
