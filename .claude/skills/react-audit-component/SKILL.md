---
description: Audit a component for accessibility, performance, and fraudoff-ui coding conventions. Produces a structured report with critical/recommended/minor findings.
argument-hint: <component-file-path>
---

Audit a fraudoff-ui component for accessibility, performance, and coding conventions.

**Arguments:** $ARGUMENTS (component file path required, e.g. "components/navigation-bar.tsx")

**Steps:**

1. Read the component file at $ARGUMENTS.

2. Also read any related files (parent layout, sub-components) that are relevant to the audit.

3. Evaluate the component across these dimensions and report findings:

### Accessibility
- All interactive elements (buttons, links, inputs) have accessible labels (`aria-label`, `aria-labelledby`, or visible text)
- Keyboard navigation works: focusable elements in logical tab order
- Color is not the sole means of conveying information
- Images have meaningful `alt` text (or `alt=""` for decorative)
- Headings are in correct hierarchical order (h1 → h2 → h3)
- Focus rings are visible (not removed with `outline-none` without a replacement)

### Performance
- `"use client"` boundary is as low in the tree as possible
- No unnecessary re-renders (functions/objects defined outside component or memoized)
- Heavy imports (Framer Motion, MUI Icons) are tree-shaken or lazy-loaded
- Images use `next/image` with proper dimensions

### Conventions (fraudoff-ui standards)
- Uses `cn()` for className merging
- No hardcoded colors — uses CSS variable-based classes
- Named export (not default export)
- Props interface follows `<ComponentName>Props` naming
- Follows the shadcn/ui component pattern if applicable

### Code Quality
- No unused imports or variables
- No `any` types without justification
- Error states handled where applicable

4. Present findings in a structured report:
```
## Audit: <ComponentName>

### Critical (must fix)
- <issue>

### Recommended (should fix)
- <issue>

### Minor (nice to have)
- <issue>

### Looks good
- <item that is well done>
```

5. Ask if the user wants any of the issues fixed now.
