---
description: Create an Architecture Decision Record (ADR) for a technical or design decision. Auto-numbers from existing ADRs in docs/adr/.
argument-hint: [brief description of the decision]
---

Create an Architecture Decision Record (ADR) for a technical or design decision in fraudoff-ui.

**Arguments:** $ARGUMENTS (optional — brief description of the decision)

**Steps:**

1. List existing ADRs to determine the next number:
   - Check `docs/adr/` for existing files named `adr-NNN-*.md`
   - Next ADR number = highest existing number + 1

2. Ask the user for:
   - **Title**: Short description of the decision (e.g. "Use Zustand for global state")
   - **Status**: `Proposed` | `Accepted` | `Deprecated` | `Superseded by ADR-NNN`
   - **Context**: What situation or problem prompted this decision?
   - **Decision**: What was decided?
   - **Consequences**: What are the trade-offs — positive and negative?
   - **Alternatives considered**: What else was evaluated and why was it rejected?

3. Generate the ADR filename: `adr-<NNN>-<slugified-title>.md`

4. Create the file at `docs/adr/<filename>`:

```markdown
# ADR-<NNN>: <Title>

**Date:** <YYYY-MM-DD>
**Status:** <status>

## Context

<context>

## Decision

<decision>

## Consequences

### Positive
- <benefit>

### Negative / Trade-offs
- <drawback>

## Alternatives Considered

| Option | Why rejected |
|---|---|
| <alternative> | <reason> |
```

5. Output the file path and a one-line summary of the decision.
