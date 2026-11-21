# Component Library

**Last updated:** 2026-03-09

---

## Overview

FraudOffUI uses a layered component system:

1. **Radix UI primitives** — unstyled, accessible, behavioral foundation
2. **shadcn/ui pattern** — Radix primitives + Tailwind + `cn()` = styled, composable components
3. **Lucide React** — icon library for all icons (shadcn/ui's default)

> **Migration note:** MUI Icons Material is currently installed and used for feature card icons. All MUI icon imports are being migrated to lucide-react equivalents in Phase 3, after which the `@mui/icons-material`, `@mui/material`, `@emotion/react`, and `@emotion/styled` packages will be removed. See [package-inventory.md](../package-inventory.md) for the full removal plan.

---

## shadcn/ui Pattern

shadcn/ui is not a traditional component library dependency — it is a collection of copy-paste component templates that live directly in the codebase at `components/ui/`. This means:

- Components are fully customizable (they're your code)
- No black-box version bumps
- The `shadcn` CLI adds new components via `pnpm dlx shadcn@latest add <component>`

**Config:** [components.json](../../components.json)

### Available shadcn Components

| Component | File | Based on |
|---|---|---|
| Button | [components/ui/button.tsx](../../components/ui/button.tsx) | Radix Slot + CVA |
| Input | [components/ui/input.tsx](../../components/ui/input.tsx) | HTML input |
| Label | [components/ui/label.tsx](../../components/ui/label.tsx) | Radix Label |
| DropdownMenu | [components/ui/dropdown-menu.tsx](../../components/ui/dropdown-menu.tsx) | Radix Dropdown |
| NavigationMenu | [components/ui/navigation-menu.tsx](../../components/ui/navigation-menu.tsx) | Radix Navigation |

---

## Button Variants (CVA)

The `Button` component uses `class-variance-authority` (CVA) for variant management:

```ts
variants: {
  variant: {
    default:     "bg-primary text-primary-foreground hover:bg-primary/90",
    destructive: "bg-destructive text-destructive-foreground hover:bg-destructive/90",
    outline:     "border border-input bg-background hover:bg-accent",
    secondary:   "bg-secondary text-secondary-foreground hover:bg-secondary/80",
    ghost:       "hover:bg-accent hover:text-accent-foreground",
    link:        "text-primary underline-offset-4 hover:underline",
  },
  size: {
    default: "h-10 px-4 py-2",
    sm:      "h-9 rounded-md px-3",
    lg:      "h-11 rounded-md px-8",
    icon:    "h-10 w-10",
  }
}
```

---

## Radix UI Usage

Radix UI provides accessible, unstyled primitives. FraudOffUI uses them directly in shadcn components and in `NavigationBar`:

| Radix Package | Used in |
|---|---|
| `@radix-ui/react-dropdown-menu` | `DropdownMenu`, `ModeToggle` |
| `@radix-ui/react-navigation-menu` | `NavigationBar` |
| `@radix-ui/react-label` | `Label` |
| `@radix-ui/react-slot` | `Button` (asChild pattern) |

The `asChild` prop from Radix Slot allows rendering a component's styles on a different element (e.g. a `Button` that renders as a `Link`):
```tsx
<Button asChild>
  <Link href="/authentication">Get Started</Link>
</Button>
```

---

## Lucide React

The sole icon library for FraudOff (shadcn/ui's default). All icons use named imports, which are tree-shakeable by default.

Current icons in use:
- `Sun`, `Moon` — ModeToggle (theme switcher)
- `ChevronDown` — NavigationMenu dropdown indicator

### MUI Icons (legacy — migrating away)

The following MUI Icons are still in use for feature card icons in `CardGrid`. These will be replaced with lucide-react equivalents in Phase 3:

- `NewspaperIcon` — The Breach
- `SchoolIcon` — Phishing School
- `RateReviewIcon` — Rate-A-Risk
- `ForumIcon` — Tech Threads

After migration, remove: `@mui/icons-material`, `@mui/material`, `@emotion/react`, `@emotion/styled`.

---

## Adding a New shadcn Component

```bash
pnpm dlx shadcn@latest add <component-name>
```

This places the component file in `components/ui/` ready to customize.

---

## Related

- [styling-system.md](styling-system.md)
- [component-hierarchy.md](../flows/component-hierarchy.md)
- [ADR-002](../adr/adr-002-tailwind-shadcn.md)
