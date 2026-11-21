# Package Inventory

> Curated inventory of all dependencies in `package.json`. Documents why each package exists, whether it should stay, and planned changes. Update this document when packages are intentionally added, removed, or replaced.
>
> For upgrade targets, see Phase 3 of the [modernization plan](modernization-plan.md).

**Last updated:** 2026-03-08

---

## Production Dependencies

### Core Framework

| Package | Version | Purpose | Status |
|---|---|---|---|
| `next` | 14.0.1 | Next.js App Router — framework for the web app | Keep — upgrade to 15.x in Phase 3 |
| `react` | ^18.3.1 | React UI library | Keep — upgrade to 19.x with Next.js 15 |
| `react-dom` | ^18.3.1 | React DOM renderer | Keep — upgrade with React |
| `typescript` | ^5.4.5 | TypeScript compiler | Keep — minor bump in Phase 3 |

### Styling & Theming

| Package | Version | Purpose | Status |
|---|---|---|---|
| `tailwindcss` | ^3.4.3 | Utility-first CSS framework | Keep — upgrade to v4 in Phase 3 |
| `tailwind-merge` | ^2.3.0 | Resolves conflicting Tailwind classes in `cn()` utility | Keep |
| `tailwindcss-animate` | ^1.0.7 | Animation utilities for Tailwind (used by shadcn/ui) | Keep |
| `clsx` | ^2.1.1 | Conditional className utility (used in `cn()` with tailwind-merge) | Keep |
| `class-variance-authority` | ^0.7.0 | Variant-based component styling (used by shadcn/ui) | Keep |
| `autoprefixer` | ^10.4.19 | PostCSS plugin for vendor prefixes | Keep |
| `postcss` | ^8.4.38 | CSS processing pipeline (required by Tailwind) | Keep |
| `next-themes` | ^0.2.1 | Dark/light theme switching via `class` strategy | Keep |

### UI Components

| Package | Version | Purpose | Status |
|---|---|---|---|
| `@radix-ui/react-dropdown-menu` | ^2.0.6 | Accessible dropdown menu primitive (shadcn/ui) | Keep |
| `@radix-ui/react-label` | ^2.0.2 | Accessible label primitive (shadcn/ui) | Keep |
| `@radix-ui/react-navigation-menu` | ^1.1.4 | Accessible navigation menu primitive (shadcn/ui) | Keep |
| `@radix-ui/react-slot` | ^1.0.2 | Slot composition primitive (shadcn/ui) | Keep |
| `lucide-react` | ^0.292.0 | Icon library — shadcn/ui's default. Will be the sole icon library after MUI removal. | Keep — upgrade to latest; migrate all MUI Icon imports to lucide-react equivalents |

### MUI (Material UI) — REMOVING in Phase 3

| Package | Version | Purpose | Status |
|---|---|---|---|
| `@mui/icons-material` | ^5.15.18 | Material Design icons | **Remove** — migrate all icon imports to lucide-react |
| `@mui/material` | ^5.15.18 | Full Material UI component library | **Remove** — unused; FraudOff uses shadcn/ui |
| `@emotion/react` | ^11.11.4 | CSS-in-JS runtime (required by @mui/material) | **Remove** — only needed by MUI |
| `@emotion/styled` | ^11.11.5 | Styled API for Emotion (required by @mui/material) | **Remove** — only needed by MUI |

### Animation

| Package | Version | Purpose | Status |
|---|---|---|---|
| `framer-motion` | ^10.18.0 | Animation library for React (MainBanner, page transitions) | Keep — upgrade to `motion` v11+ (same library, rebranded, smaller package) |

### Auth — REMOVING in Phase 3

| Package | Version | Purpose | Status |
|---|---|---|---|
| `next-auth` | ^4.24.7 | Authentication for Next.js | **Remove** — redundant with Cognito auth via the API; UI will store JWT tokens directly |
| `@next-auth/prisma-adapter` | ^1.0.7 | Prisma adapter for next-auth | **Remove** — leftover; no Prisma setup exists |

### HTTP & Data

| Package | Version | Purpose | Status |
|---|---|---|---|
| `axios` | ^1.7.2 | HTTP client for API calls | Keep for now — will be wrapped by orval's custom instance. Could be replaced by native `fetch` later. |
| `sharp` | ^0.32.6 | Image processing (used by Next.js for `next/image` optimization) | Keep |

### Miscellaneous

| Package | Version | Purpose | Status |
|---|---|---|---|
| `glob` | ^10.4.1 | File pattern matching | **Remove** — not a UI dependency; leftover |

---

## Dev Dependencies

| Package | Version | Purpose | Status |
|---|---|---|---|
| `@eslint/js` | ^9.3.0 | ESLint JavaScript config | Keep — replace with Biome in Phase 3 |
| `eslint` | ^9.3.0 | JavaScript/TypeScript linter | Keep — replace with Biome in Phase 3 |
| `@types/node` | ^20.12.12 | TypeScript types for Node.js | Keep |
| `@types/react` | ^18.3.3 | TypeScript types for React | Keep — upgrade with React |
| `@types/react-dom` | ^18.3.0 | TypeScript types for React DOM | Keep — upgrade with React DOM |

---

## Planned Additions (Phase 3)

| Package | Purpose | Priority |
|---|---|---|
| `@tanstack/react-query` | Server state management (caching, refetch, polling, optimistic updates) | High |
| `orval` (dev) | OpenAPI codegen — generates typed TanStack Query hooks | High |
| `vitest` (dev) | Unit + component testing | High |
| `@playwright/test` (dev) | E2E testing | High |
| `zustand` | Global client state (only when needed) | Medium |
| `nuqs` | URL query parameter state management | Medium |
| `@biomejs/biome` (dev) | Linter + formatter replacing ESLint + Prettier | Medium |
| `msw` (dev) | Mock Service Worker for testing and Demo Bank | Low |

---

## Confirmed Removals (Phase 3)

Packages to remove during Phase 3, in order:

| Step | Package(s) | Action |
|---|---|---|
| 1 | `glob` | Remove immediately — no code depends on it |
| 2 | `next-auth`, `@next-auth/prisma-adapter` | Remove together — replace with direct JWT token storage |
| 3 | `@mui/icons-material` → `lucide-react` | Migrate all icon imports, then remove |
| 4 | `@mui/material`, `@emotion/react`, `@emotion/styled` | Remove after all MUI icon imports are migrated |
| 5 | `eslint`, `@eslint/js` | Remove after Biome is configured |
