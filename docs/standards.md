# FraudOffUI — Coding Standards

Living document. Use `/create-standard` to add new standards.

**Last updated:** 2026-02-25

---

## Styling

### Use `cn()` for all className composition

**Rule:** Always use the `cn()` utility from `@/lib/utils` when combining Tailwind classes. Never use string concatenation or template literals for className.

**Rationale:** `cn()` (clsx + tailwind-merge) resolves Tailwind class conflicts correctly. String concatenation can produce duplicate or conflicting classes.

**Do:**
```tsx
<div className={cn("rounded-lg bg-card p-4", isActive && "ring-2 ring-primary", className)} />
```

**Don't:**
```tsx
<div className={`rounded-lg bg-card p-4 ${isActive ? "ring-2 ring-primary" : ""}`} />
```

---

### No hardcoded colors

**Rule:** Never hardcode color values. Always use CSS variable-based Tailwind classes.

**Rationale:** The app supports dark and light modes. CSS variable-backed classes adapt automatically. Hardcoded colors break one or both themes.

**Do:**
```tsx
<p className="text-foreground bg-background" />
<span className="text-muted-foreground" />
```

**Don't:**
```tsx
<p className="text-gray-900 bg-white" />
<p style={{ color: "#111827" }} />
```

---

## Components

### Named exports only

**Rule:** All components use named exports. No default exports.

**Rationale:** Named exports make refactoring and auto-import more reliable. They also make the import path unambiguous.

**Do:**
```tsx
export function FeatureCard({ ... }: FeatureCardProps) { ... }
```

**Don't:**
```tsx
export default function FeatureCard() { ... }
```

---

### Props interface naming

**Rule:** Props interface must be named `<ComponentName>Props`.

**Do:**
```tsx
interface NavigationBarProps {
  className?: string
}

export function NavigationBar({ className }: NavigationBarProps) { ... }
```

---

### Minimize `"use client"` boundaries

**Rule:** Components are Server Components by default. Add `"use client"` only when the component uses hooks, browser APIs, or event handlers directly.

**Rationale:** Server Components improve initial load performance and reduce JavaScript bundle size.

**Do:** Place interactive logic in a small leaf component. Keep parent components as Server Components.

**Don't:** Add `"use client"` to a page or layout just because one nested element needs it.

---

## TypeScript

### No `any` without justification

**Rule:** Never use `any` as a type. Use `unknown` for truly unknown values, or a proper type/interface.

**Do:**
```ts
function parseResponse(raw: unknown): ApiResponse { ... }
```

**Don't:**
```ts
function parseResponse(raw: any): any { ... }
```

---

## API Routes

### Always wrap handlers in try/catch

**Rule:** Every route handler body must be wrapped in a try/catch. Return a structured JSON error on failure.

**Do:**
```ts
export async function GET(request: NextRequest) {
  try {
    return NextResponse.json({ data: result })
  } catch (error) {
    console.error("[GET /api/example]", error)
    return NextResponse.json({ error: "Internal server error" }, { status: 500 })
  }
}
```

---

### Validate input at the boundary

**Rule:** Validate and sanitize all request body/query parameter data before using it. Never trust external input.

**Rationale:** Prevents injection attacks and unexpected runtime errors from malformed data.

---

## Git

### Commit message format

**Rule:** Use imperative mood, present tense. ≤72 characters on the first line.

**Do:**
```
Add phishing school lesson page
Fix navigation dropdown on mobile
Chore: upgrade Tailwind to v4
```

**Don't:**
```
added phishing school
fixed the dropdown bug that was causing issues on mobile devices when the screen was smaller
```
