# Navigation Flow

**Last updated:** 2026-02-25

---

## Overview

Navigation is handled by the `NavigationBar` component, which is rendered in the root layout and appears on every page. It is a fixed top bar with a logo, primary links, a docs dropdown, and the theme toggle.

---

## NavigationBar Structure

**File:** [components/navigation-bar.tsx](../../components/navigation-bar.tsx)
**Rendered in:** [app/layout.tsx](../../app/layout.tsx)

```
NavigationBar (fixed, top-0, z-10)
├── Logo (fraudofflogo.png + "FraudOff" gradient text)
├── "Get Started" link → /authentication
├── Docs dropdown (NavigationMenuPrimitive from Radix UI)
│   ├── Manuals → #
│   ├── License → #
│   └── API → #
└── ModeToggle (theme switcher)
```

---

## Component Details

### Logo
- Image: `/public/fraudofflogo.png` (via `next/image`)
- Text: "FraudOff" with pink/purple gradient (`bg-gradient-to-r from-pink-500 to-purple-600`)
- Both image and text link to `/`

### Navigation Links
- **Get Started**: Links to `/authentication`
- All links use Next.js `<Link>` for client-side navigation

### Docs Dropdown
- Built with `NavigationMenuPrimitive` from `@radix-ui/react-navigation-menu`
- Trigger shows "Documentation" with an animated chevron
- Opens a positioned content panel with `ListItem` entries
- Each `ListItem` uses a `forwardRef` pattern with title and description

### ModeToggle
- Renders a dropdown button (Sun or Moon icon depending on current theme)
- Opens a small dropdown with Light / Dark / System options
- Uses `useTheme()` from next-themes

---

## Client Component Boundary

`NavigationBar` is a `"use client"` component because:
- `ModeToggle` uses `useTheme()` (hook)
- Radix `NavigationMenuPrimitive` uses internal state for open/close behavior

---

## Responsive Behavior

The navigation bar is not currently collapsing to a mobile hamburger menu. On small screens:
- Items may overflow horizontally
- **Phase 4** will add a responsive mobile navigation pattern

---

## Adding New Navigation Links

To add a new top-level link, edit the `NavigationBar` component and add a `<NavigationMenuItem>` with a `<Link>` child inside the `<NavigationMenuList>`.

To add a new item to the Docs dropdown, add a `<ListItem>` inside the `NavigationMenuContent`.

---

## Related

- [component-hierarchy.md](component-hierarchy.md)
- [authentication-flow.md](authentication-flow.md)
