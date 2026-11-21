---
description: Scaffold a new shadcn/ui-compatible React component with cn(), TypeScript props interface, and named export.
argument-hint: <ComponentName>
---

Scaffold a new shadcn/ui-compatible React component for fraudoff-ui.

**Arguments:** $ARGUMENTS (component name required, e.g. "FeatureCard" or "UserAvatar")

**Steps:**

1. Determine the component name from $ARGUMENTS. Convert to PascalCase if needed.

2. Ask the user:
   - **Location**: `components/ui/` (primitive/reusable) or `components/` (page-level/feature)?
   - **Client component?**: Does it need interactivity (`"use client"`)? Default: no.
   - **Props**: What props should it accept?

3. Scaffold the file at the chosen path (e.g. `components/ui/feature-card.tsx`):

```tsx
// "use client"  ← include only if interactive

import { cn } from "@/lib/utils"

interface <ComponentName>Props {
  className?: string
  // <additional props>
}

export function <ComponentName>({ className, ...props }: <ComponentName>Props) {
  return (
    <div className={cn("<base-tailwind-classes>", className)}>
      {/* component content */}
    </div>
  )
}
```

Rules:
- Use `cn()` for all className merging
- No hardcoded colors — use CSS variable-based Tailwind classes (e.g. `bg-background`, `text-foreground`)
- Dark mode via `dark:` variants
- Export as named export (not default)
- Props interface named `<ComponentName>Props`

4. After creating the file, show the full content and the import path (`@/components/...`).
