# Theme Flow

**Last updated:** 2026-02-25

---

## Overview

FraudOffUI uses **next-themes** for dark/light mode. Theme state is stored in a cookie and applied as a CSS class on `<html>`. Tailwind's `dark:` variants respond to this class.

---

## Initialization Sequence

```
1. app/layout.tsx renders <ThemeProvider>
2. next-themes reads theme from cookie (or localStorage)
3. Applies "dark" or "light" class to <html>
4. CSS variables defined in globals.css resolve based on the class
5. Tailwind dark: variants activate when class="dark" is present
```

---

## ThemeProvider Configuration

**File:** [components/theme-provider.tsx](../../components/theme-provider.tsx)
**Used in:** [app/layout.tsx](../../app/layout.tsx)

```tsx
<ThemeProvider
  attribute="class"       // applies "dark"/"light" as a class
  defaultTheme="dark"     // FraudOff defaults to dark mode
  enableSystem            // also detects OS preference
>
```

- `attribute="class"` — theme is applied as a CSS class on `<html>`, not a `data-*` attribute
- `defaultTheme="dark"` — first-time visitors see dark mode
- `enableSystem` — if no stored preference, follows OS color scheme

---

## CSS Variable Propagation

**File:** [styles/globals.css](../../styles/globals.css)

Variables are defined in two blocks:

```css
:root {
  /* light mode values */
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --accent: 330 60% 50%;
  /* ... */
}

.dark {
  /* dark mode values */
  --background: 224 71.4% 4.1%;
  --foreground: 210 20% 98%;
  /* ... */
}
```

When next-themes adds `class="dark"` to `<html>`, the `.dark` block overrides `:root`. All Tailwind color utilities (e.g. `bg-background`, `text-foreground`) resolve through these variables.

---

## ModeToggle

**File:** [components/mode-toggle.tsx](../../components/mode-toggle.tsx)

- Client component (`"use client"`)
- Uses `useTheme()` from next-themes
- Dropdown with Sun/Moon icons from Lucide React
- Calls `setTheme("light")` / `setTheme("dark")` / `setTheme("system")`
- Animates icon rotation on theme change
- Active theme is indicated with a bullet point in the dropdown

---

## Background Images

Background images switch with theme via custom Tailwind utilities defined in [tailwind.config.ts](../../tailwind.config.ts):

```ts
backgroundImage: {
  "background-dark": "url('/background-dark.webp')",
  "background-light": "url('/background-light.webp')",
}
```

Used in [app/page.tsx](../../app/page.tsx):
```tsx
<main className="bg-background-light dark:bg-background-dark bg-cover">
```

---

## Flash of Unstyled Content (FOUC) Prevention

next-themes uses a small inline script injected before page paint to read the stored theme and set the class on `<html>` synchronously. This prevents a flash of the wrong theme on first load.

---

## Related

- [styling-system.md](../architecture/styling-system.md)
- [component-library.md](../architecture/component-library.md)
