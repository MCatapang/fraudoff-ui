---
description: Scaffold a new Next.js App Router API route handler with proper typing, error handling, and optional auth guard.
argument-hint: <route-path, e.g. "users" or "auth/session">
---

Scaffold a new Next.js App Router API route handler for fraudoff-ui.

**Arguments:** $ARGUMENTS (route path required, e.g. "users" or "auth/session")

**Steps:**

1. Determine the route path from $ARGUMENTS. The file will be created at `app/api/<path>/route.ts`.

2. Ask the user:
   - **HTTP methods** needed: GET, POST, PUT, PATCH, DELETE?
   - **Auth required?** Should the handler check for a valid session?
   - **Brief description** of what the route does

3. Scaffold `app/api/<path>/route.ts`:

```ts
import { NextRequest, NextResponse } from "next/server"
// import { getServerSession } from "next-auth"
// import { authOptions } from "@/lib/auth"

export async function GET(request: NextRequest) {
  // const session = await getServerSession(authOptions)
  // if (!session) return NextResponse.json({ error: "Unauthorized" }, { status: 401 })

  try {
    // TODO: implement
    return NextResponse.json({ data: null })
  } catch (error) {
    console.error("[GET /api/<path>]", error)
    return NextResponse.json({ error: "Internal server error" }, { status: 500 })
  }
}

// export async function POST(request: NextRequest) {
//   const body = await request.json()
//   ...
// }
```

Rules:
- Always wrap handler body in try/catch
- Return `NextResponse.json()` for all responses
- Log errors with the route path as context prefix
- Validate request body before using it (do not trust input)
- Never expose stack traces in responses

4. Show the created file and the URL it handles (e.g. `/api/users`).
