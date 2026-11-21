# Routing

**Last updated:** 2026-02-25

---

## Router

FraudOffUI uses **Next.js App Router** (introduced in Next.js 13, stabilized in 13.4). All routes live under the `/app` directory.

---

## Current Route Map

```
/app
├── layout.tsx                # Root layout (ThemeProvider, NavigationBar)
├── page.tsx                  # / (home — ItemCarousel, MainBanner, CardGrid)
└── authentication/
    └── page.tsx              # /authentication (sign-in/sign-up)
```

---

## Planned Route Structure (Phase 4)

When feature pages and authentication are implemented, routes will be organized into **route groups** to apply different layouts:

```
/app
├── layout.tsx                 # Root layout (always applied)
├── (marketing)/               # Route group: public pages, no auth required
│   ├── page.tsx               # /
│   ├── authentication/
│   │   └── page.tsx           # /authentication
│   └── layout.tsx             # Marketing layout (if different from root)
└── (app)/                     # Route group: authenticated feature pages
    ├── layout.tsx             # App layout (auth-gated via middleware)
    ├── the-breach/
    │   ├── page.tsx           # /the-breach
    │   ├── loading.tsx
    │   └── error.tsx
    ├── phishing-school/
    │   └── page.tsx           # /phishing-school
    ├── rate-a-risk/
    │   └── page.tsx           # /rate-a-risk
    └── tech-threads/
        └── page.tsx           # /tech-threads
```

Route groups (`(name)`) are directory names in parentheses. They group routes for shared layouts/middleware without affecting the URL path.

---

## Special Files

| File | Purpose |
|---|---|
| `layout.tsx` | Shared UI and context for a route and its children |
| `page.tsx` | The page component (renders at the route URL) |
| `loading.tsx` | Shown while the page loads (React Suspense fallback) |
| `error.tsx` | Error boundary for the route (`"use client"` required) |
| `not-found.tsx` | Rendered when `notFound()` is thrown |

---

## API Routes

API routes live under `app/api/` and use the filename `route.ts`:

```
/app/api
└── auth/
    └── [...nextauth]/
        └── route.ts           # next-auth handler (to be created)
```

---

## Middleware

Authentication enforcement will be handled by `middleware.ts` at the project root (not yet created). It uses `withAuth` from next-auth to redirect unauthenticated users to `/authentication`.

---

## Navigation

All in-app navigation uses `next/link` (`<Link>`). External links use `<a target="_blank" rel="noopener noreferrer">`.

---

## Related

- [authentication-flow.md](../flows/authentication-flow.md)
- [ADR-001](../adr/adr-001-nextjs-app-router.md)
