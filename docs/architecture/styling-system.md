# Styling System

**Last updated:** 2026-02-25

---

## Approach

FraudOffUI uses **utility-first Tailwind CSS** with a CSS variable theming layer. All colors are defined as HSL CSS variables, which Tailwind resolves at runtime. This enables dark/light mode switching without JavaScript class swapping on individual elements.

---

## File Locations

| File | Purpose |
|---|---|
| [styles/globals.css](../../styles/globals.css) | CSS variables, Tailwind directives, base styles |
| [tailwind.config.ts](../../tailwind.config.ts) | Theme extensions, animations, plugins |
| [lib/utils.ts](../../lib/utils.ts) | `cn()` utility (clsx + tailwind-merge) |

---

## CSS Variable System

Variables are defined in HSL format (without the `hsl()` wrapper) so Tailwind can compose them with opacity modifiers:

```css
:root {
  --background: 0 0% 100%;       /* white */
  --foreground: 222.2 84% 4.9%;  /* near-black */
  --primary: 222.2 47.4% 11.2%;
  --secondary: 210 40% 96.1%;
  --accent: 330 60% 50%;         /* pink */
  --muted: 210 40% 96.1%;
  --card: 0 0% 100%;
  --border: 214.3 31.8% 91.4%;
  --input: 214.3 31.8% 91.4%;
  --ring: 222.2 84% 4.9%;
  --radius: 0.5rem;
}

.dark {
  --background: 224 71.4% 4.1%;  /* near-black blue */
  --foreground: 210 20% 98%;     /* near-white */
  --primary: 210 20% 98%;
  /* ... dark overrides ... */
}
```

Tailwind maps these via `tailwind.config.ts`:
```ts
colors: {
  background: "hsl(var(--background))",
  foreground: "hsl(var(--foreground))",
  // ...
}
```

---

## Dark Mode Strategy

- **Provider**: `next-themes` (see [theme-flow.md](../flows/theme-flow.md))
- **Mechanism**: `class` strategy — next-themes adds `class="dark"` to `<html>`
- **Tailwind**: `darkMode: "class"` in `tailwind.config.ts`
- **Usage**: Use `dark:` variants for overrides: `bg-white dark:bg-slate-900`

For most colors, dark mode is handled automatically via CSS variables — no `dark:` variant needed.

---

## Brand Palette

| Token | Value | Usage |
|---|---|---|
| `--accent` | HSL 330 60% 50% (pink) | Primary brand color, gradient start |
| Purple | ~HSL 270 60% 50% | Gradient end |
| Blue | various | Secondary accent |

Gradient usage:
```tsx
// Standard brand gradient
className="bg-gradient-to-r from-pink-500 to-purple-600 bg-clip-text text-transparent"
```

---

## Background Images

Defined in `tailwind.config.ts` as custom utility classes:

```ts
backgroundImage: {
  "background-dark": "url('/background-dark.webp')",
  "background-light": "url('/background-light.webp')",
}
```

Usage:
```tsx
<main className="bg-background-light dark:bg-background-dark bg-cover bg-center" />
```

---

## Animations

Defined in `tailwind.config.ts`:

| Name | Used by | Description |
|---|---|---|
| `animate-slide` | `ItemCarousel` | 10s infinite linear horizontal scroll |
| `animate-accordion-down` | shadcn Accordion | Expand animation |
| `animate-accordion-up` | shadcn Accordion | Collapse animation |

---

## `cn()` Utility

```ts
// lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

Always use `cn()` when conditionally applying classes or accepting a `className` prop. It prevents Tailwind class conflicts.

---

## Border Radius

Defined via CSS variable `--radius` (0.5rem default). Tailwind extensions:
```ts
borderRadius: {
  lg: "var(--radius)",
  md: "calc(var(--radius) - 2px)",
  sm: "calc(var(--radius) - 4px)",
}
```

---

## Related

- [theme-flow.md](../flows/theme-flow.md)
- [ADR-002](../adr/adr-002-tailwind-shadcn.md)
