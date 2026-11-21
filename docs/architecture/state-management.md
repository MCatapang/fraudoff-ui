# State Management

**Last updated:** 2026-02-25

---

## Philosophy

FraudOffUI follows a **minimal state** principle: use the simplest mechanism that solves the problem. Avoid introducing global state management until local state is genuinely insufficient.

---

## Current State Mechanisms

### 1. React `useState` / `useEffect` (local component state)

Used in client components for self-contained interactivity:

| Component | State |
|---|---|
| `UserAuthForm` | `isLoading: boolean` (sign-in loading spinner) |
| `RandomQuote` | `quote` (randomly selected on mount) |
| `ModeToggle` | Delegates to `useTheme()` — no own state |

### 2. next-themes (`useTheme`)

Global theme state (dark/light/system). Persisted in a cookie. Provided by `ThemeProvider` at the root layout.

```tsx
const { theme, setTheme } = useTheme()
```

### 3. Static / Server-rendered data

The majority of content is static. Feature card data (`LinkCardParams[]`) is defined as a constant in `card-grid.tsx` and rendered on the server with no state needed.

---

## Current State Inventory

| State | Scope | Mechanism | Location |
|---|---|---|---|
| Theme (dark/light) | Global | next-themes | ThemeProvider → all components |
| Form loading | Local | useState | UserAuthForm |
| Selected quote | Local | useState | RandomQuote |

---

## When to Add New State

Follow this decision tree:

```
Does the state need to be shared across routes?
  → Yes: Use URL state (searchParams / nuqs — Phase 3) or Zustand (Phase 3)
  → No:
    Does it need to persist across page reloads?
      → Yes: Use cookie or localStorage (with caution)
      → No:
        Is it just for one component?
          → Yes: useState
          → No: Lift state to the nearest common parent
```

---

## Planned State Additions (Phase 3)

### Zustand — Global client state

Add Zustand when `useState` prop-drilling becomes impractical across 3+ component levels.

**Example use cases:**
- User session data surfaced to many UI components
- Notification/toast queue
- Feature-level UI state (quiz progress, lesson completion)

```ts
// lib/stores/user-store.ts
import { create } from "zustand"

interface UserStore {
  user: User | null
  setUser: (user: User | null) => void
}

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}))
```

### nuqs — URL query parameter state

For state that should be shareable via URL (search filters, pagination, active tabs):

```tsx
import { useQueryState } from "nuqs"

const [category, setCategory] = useQueryState("category")
```

---

## Server State

Server Components can pass data as props to Client Components. For data that needs to refresh, use Route Handlers + SWR or React Query (to be evaluated in Phase 3 if needed).

---

## Related

- [data-fetching.md](../flows/data-fetching.md)
- [architecture/routing.md](routing.md)
