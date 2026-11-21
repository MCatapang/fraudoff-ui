---
description: Scaffold a new Next.js App Router page with page.tsx, loading.tsx, and error.tsx files.
argument-hint: <route-path, e.g. "the-breach" or "phishing-school/lesson">
---

Scaffold a new Next.js App Router page with layout, loading, and error files for fraudoff-ui.

**Arguments:** $ARGUMENTS (route path required, e.g. "the-breach" or "phishing-school/lesson")

**Steps:**

1. Determine the route path from $ARGUMENTS. Strip leading slash if present.

2. Ask the user:
   - **Route group**: Should it live under `(marketing)` (public) or `(app)` (authenticated)? If no route groups exist yet, use `app/<path>`.
   - **Page title** for metadata
   - **Does it need authentication?** (will add a note but not implement middleware yet)

3. Scaffold these files under `app/<path>/`:

**page.tsx:**
```tsx
import type { Metadata } from "next"

export const metadata: Metadata = {
  title: "<Page Title> | FraudOff",
  description: "<brief description>",
}

export default function <PageName>Page() {
  return (
    <main className="flex min-h-screen flex-col items-center">
      <h1 className="text-4xl font-bold"><Page Title></h1>
    </main>
  )
}
```

**loading.tsx:**
```tsx
export default function Loading() {
  return (
    <div className="flex min-h-screen items-center justify-center">
      <div className="h-8 w-8 animate-spin rounded-full border-4 border-primary border-t-transparent" />
    </div>
  )
}
```

**error.tsx:**
```tsx
"use client"

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div className="flex min-h-screen flex-col items-center justify-center gap-4">
      <h2 className="text-2xl font-semibold">Something went wrong</h2>
      <button onClick={reset} className="underline">Try again</button>
    </div>
  )
}
```

4. After scaffolding, show the files created and the route URL (e.g. `/the-breach`).
