# Data Fetching

**Last updated:** 2026-02-25

---

## Overview

FraudOffUI currently renders primarily static content with no live data fetching. This document describes the current state and the intended patterns for when data fetching is needed.

---

## Current State

| Page / Component | Data source | Method |
|---|---|---|
| Home page (`app/page.tsx`) | Static constants in `card-grid.tsx` | None — hardcoded |
| `ItemCarousel` | Hardcoded array of threat label strings | None |
| `RandomQuote` | Hardcoded array of 4 quotes | None |
| Authentication page | Form submission (not yet wired) | None |

No API routes exist yet. Axios is installed but not used.

---

## Intended Data Fetching Patterns

### Server Components (preferred)

For data that does not need to be interactive, fetch in a Server Component using `async/await`. This runs on the server, keeps secrets out of the client bundle, and avoids waterfall requests.

```tsx
// app/the-breach/page.tsx
async function getBreachArticles() {
  const res = await fetch("https://api.example.com/articles", {
    next: { revalidate: 3600 }, // ISR: revalidate every hour
  })
  if (!res.ok) throw new Error("Failed to fetch articles")
  return res.json()
}

export default async function TheBreachPage() {
  const articles = await getBreachArticles()
  return <ArticleList articles={articles} />
}
```

### API Routes

For client-initiated mutations (form submissions, user actions), use Next.js API routes at `app/api/*/route.ts`. API routes use Axios on the client side for consistency.

```ts
// Client component
import axios from "axios"

const { data } = await axios.post("/api/risk-assessment", { answers })
```

### Server Actions (future)

When Next.js 15 is adopted (Phase 3), prefer **Server Actions** over API routes for form mutations. They reduce boilerplate and eliminate the need for a separate fetch call.

---

## Environment Variables

- Secrets (API keys, database URLs) must live in `.env.local` and **never** in client-side code
- Variables prefixed with `NEXT_PUBLIC_` are exposed to the browser — only use this for non-sensitive configuration
- The Phase 4 security audit will verify no secrets leak client-side

---

## Caching Strategy

| Pattern | When to use |
|---|---|
| `cache: "force-cache"` (default) | Static data that rarely changes |
| `next: { revalidate: N }` | Data that changes on a schedule (ISR) |
| `cache: "no-store"` | Real-time data (user-specific, always fresh) |

---

## Related

- [architecture/state-management.md](../architecture/state-management.md)
- [architecture/routing.md](../architecture/routing.md)
