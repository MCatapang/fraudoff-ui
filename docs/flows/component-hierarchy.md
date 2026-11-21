# Component Hierarchy

**Last updated:** 2026-02-25

---

## Overview

This document describes the top-level layout → page → section → component tree for FraudOffUI.

---

## Root Layout

**File:** [app/layout.tsx](../../app/layout.tsx)

```
<html lang="en">
  <body>
    <ThemeProvider>          ← next-themes; applies dark/light class
      <NavigationBar />      ← fixed top nav (always rendered)
      {children}             ← page content slot
    </ThemeProvider>
  </body>
</html>
```

The root layout is a **Server Component** that renders on every page. `ThemeProvider` and `NavigationBar` are mounted once and persist across client-side navigations.

---

## Home Page

**File:** [app/page.tsx](../../app/page.tsx)

```
<main> (bg-cover background image, flex col)
  <ItemCarousel />           ← scrolling threat label marquee
  <MainBanner />             ← hero section with headline
  <CardGrid />               ← 4 feature link cards
</main>
```

All three components on the home page are **Server Components** (no `"use client"`).

---

## Authentication Page

**File:** [app/authentication/page.tsx](../../app/authentication/page.tsx)

```
<div> (two-column grid)
  <aside>                    ← hidden on mobile
    <RandomQuote />          ← "use client", random quote
  </aside>
  <section>
    <UserAuthForm />         ← "use client", email + GitHub sign-in
  </section>
</div>
```

---

## Component Tree Detail

### NavigationBar → [components/navigation-bar.tsx](../../components/navigation-bar.tsx)
```
NavigationBar ("use client")
├── next/image (logo)
├── Link (logo text, href="/")
├── Link ("Get Started", href="/authentication")
├── NavigationMenuPrimitive.Root
│   └── NavigationMenuPrimitive.List
│       └── NavigationMenuPrimitive.Item (Docs)
│           ├── NavigationMenuPrimitive.Trigger
│           └── NavigationMenuPrimitive.Content
│               └── ListItem × 3 (Manuals, License, API)
└── ModeToggle
    └── DropdownMenu (Radix UI)
        ├── DropdownMenuTrigger (Sun/Moon icon button)
        └── DropdownMenuContent
            ├── DropdownMenuItem (Light)
            ├── DropdownMenuItem (Dark)
            └── DropdownMenuItem (System)
```

### ItemCarousel → [components/item-carousel.tsx](../../components/item-carousel.tsx)
```
ItemCarousel ("use client")
└── div.group (overflow-hidden, pause-on-hover)
    └── div.animate-slide (infinite scroll)
        └── span × N (threat labels, strikethrough text)
```

### MainBanner → [components/main-banner.tsx](../../components/main-banner.tsx)
```
MainBanner (Server Component)
└── section
    └── h1 (large gradient headline text)
```

### CardGrid → [components/card-grid.tsx](../../components/card-grid.tsx)
```
CardGrid (Server Component)
└── div.grid (2-col mobile, 4-col desktop)
    └── LinkCard × 4
        └── a (external link)
            ├── icon (MUI Icon)
            ├── h3 (title)
            └── p (description)
```

---

## Client Component Boundaries

| Component | Reason for `"use client"` |
|---|---|
| `NavigationBar` | Uses Radix menu state; renders `ModeToggle` |
| `ModeToggle` | Uses `useTheme()` hook |
| `ItemCarousel` | Carousel pause-on-hover behavior |
| `ThemeProvider` | Wraps next-themes provider (needs client context) |
| `UserAuthForm` | Form state, loading state, Framer Motion |
| `RandomQuote` | Uses `useState` for random selection |
| `PageTransition` | Framer Motion exit animations require client |

---

## Related

- [navigation-flow.md](navigation-flow.md)
- [authentication-flow.md](authentication-flow.md)
- [architecture/component-library.md](../architecture/component-library.md)
