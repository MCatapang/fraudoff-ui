# Authentication Flow

**Last updated:** 2026-03-09

---

## Overview

FraudOff uses **AWS Cognito** as the identity provider. The **API is the auth gateway** — the UI and all future clients (mobile, CLI) authenticate through API endpoints, which delegate to Cognito. The UI never calls Cognito directly.

This pattern was chosen to ensure a single auth path regardless of client type. See [ADR-004](../adr/adr-004-solution-architecture.md) for the full decision rationale.

> **Note:** The `next-auth` and `@next-auth/prisma-adapter` packages are currently installed but scheduled for removal in Phase 3. They are unused — authentication will be implemented as described below.

---

## Auth Flow

```
User submits email + password
        ↓
UI calls POST /api/v1/auth/login (via API client)
        ↓
fraudoff-api validates credentials against Cognito
        ↓
Cognito issues JWT tokens (access + refresh + id)
        ↓
API returns tokens to UI
        ↓
UI stores tokens (localStorage or httpOnly cookie)
        ↓
Subsequent API calls include Authorization: Bearer <access_token>
        ↓
API validates JWT signature against Cognito JWKS
        ↓
API checks Cedar authorization for the requested resource
        ↓
Response returned
```

---

## Token Management

| Token | Purpose | Storage | Lifetime |
|---|---|---|---|
| **Access token** | Authorizes API requests | localStorage (v1) | Short-lived (~1 hour) |
| **Refresh token** | Obtains new access tokens without re-login | localStorage (v1) | Long-lived (~30 days) |
| **ID token** | Contains user profile claims (email, name) | localStorage (v1) | Short-lived (~1 hour) |

The custom Axios instance (`lib/api/custom-instance.ts`) attaches the access token to every request via an interceptor. When a token expires, the interceptor uses the refresh token to obtain a new access token transparently.

---

## Authentication Page

**Route:** `/authentication`
**File:** [app/authentication/page.tsx](../../app/authentication/page.tsx)

- Two-column layout: quote sidebar (left) + sign-in form (right)
- `UserAuthForm` renders email + password inputs
- `RandomQuote` displays a randomly selected motivational quote

---

## UserAuthForm

**File:** [components/user-auth-form.tsx](../../components/user-auth-form.tsx)

- Client component (`"use client"`)
- Accepts email + password input
- Submits credentials to `POST /api/v1/auth/login` via the API client
- Loading state with spinner animation (Framer Motion entrance)
- v1: email/password only. Google OAuth is a future addition.

---

## Protected Routes Strategy

Protected routes are enforced via Next.js `middleware.ts` at the project root. The middleware checks for a valid access token — if missing or expired, the user is redirected to `/authentication`.

```ts
// middleware.ts
import { NextRequest, NextResponse } from "next/server"

export function middleware(request: NextRequest) {
  const token = request.cookies.get("access_token")?.value
    ?? request.headers.get("authorization")?.replace("Bearer ", "")

  if (!token) {
    return NextResponse.redirect(new URL("/authentication", request.url))
  }

  // Optionally validate token expiry client-side
  // Full validation happens server-side in the API
  return NextResponse.next()
}

export const config = {
  matcher: [
    "/(app)/:path*",   // all authenticated route group pages
  ],
}
```

> **Note:** The middleware performs a lightweight client-side check (token exists). The API performs full JWT validation (signature, expiry, claims) and Cedar authorization on every request. The UI check is a UX convenience — not a security boundary.

---

## Auth Architecture Summary

| Concern | Where it lives |
|---|---|
| **Identity provider** | AWS Cognito (managed by the API) |
| **Auth endpoints** | fraudoff-api: `POST /api/v1/auth/login`, `/register`, `/refresh`, `/logout` |
| **Token storage** | UI: localStorage (v1) |
| **Token validation** | API: validates JWT against Cognito JWKS |
| **Authorization** | API: Amazon Cedar (policy-as-code) |
| **UI permissions** | `GET /api/v1/me/permissions` — short TTL, refetch-on-focus via TanStack Query |
| **Route protection** | UI: middleware.ts (lightweight); API: enforced on every request |

---

## Related

- [ADR-004](../adr/adr-004-solution-architecture.md) — solution architecture decisions (auth, authorization, deployment)
- [component-hierarchy.md](component-hierarchy.md)
- [product-vision.md](../product-vision.md) — auth requirements (section 7)
