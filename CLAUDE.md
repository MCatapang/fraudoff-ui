# fraudoff-ui

A cybersecurity education and awareness web application built with Next.js.

## Tech Stack

- **Framework**: Next.js 14 (App Router) with React 18 and TypeScript 5
- **Styling**: Tailwind CSS 3 with CSS variables for theming
- **UI Components**: shadcn/ui (Radix UI primitives) + lucide-react (MUI Icons being migrated away in Phase 3)
- **Animations**: Framer Motion (upgrading to `motion` v11+ in Phase 3)
- **Auth**: AWS Cognito via API (`next-auth` installed but being removed in Phase 3 — see ADR-004)
- **Theme**: next-themes (dark/light mode via CSS class)
- **HTTP**: Axios

## Package Manager

Use **pnpm** exclusively. It is enforced via a `preinstall` hook.

```bash
pnpm install       # install dependencies
pnpm dev           # start dev server
pnpm build         # production build
pnpm start         # start production server
pnpm lint          # run ESLint
```

## Project Structure

```
/app               # Next.js App Router — pages, layouts, routes
/components        # React components (UI primitives, page sections)
/lib               # Utilities, TypeScript interfaces and types
/styles            # Global CSS with Tailwind directives
/public            # Static assets (logo, background images)
```

## Patterns & Conventions

### Styling
- Utility-first Tailwind CSS with `cn()` helper (`clsx` + `tailwind-merge`) from `@/lib/utils`
- Colors use HSL CSS variables defined in `globals.css` — do not hardcode colors
- Dark mode uses the `class` strategy via next-themes; use `dark:` Tailwind variants
- Background images: `background-light.webp` / `background-dark.webp` (defined in `tailwind.config.ts`)
- Brand palette: pink/purple primary gradients, blue accents

### Components
- Follow the shadcn/ui pattern: Radix UI primitives + Tailwind + `cn()`
- Path alias `@/` maps to the project root
- Server Components by default; add `"use client"` only when needed
- Component imports use the `@/components` alias

### Types
- Shared interfaces live in `/lib/interfaces`, shared types in `/lib/types`
- Key types: `NavLink { title, description, href }`, `LinkCardParams { href, icon?, title, description }`

### Key Components
- `NavigationBar` — top nav with logo, links, docs dropdown, theme toggle
- `MainBanner` — hero section with gradient text and hover animations
- `CardGrid` — maps `LinkCardParams[]` to feature cards (The Breach, Phishing School, Rate-A-Risk, Tech Threads)
- `ItemCarousel` — featured content carousel
- `ModeToggle` — dark/light theme switcher

## Branch Naming Convention

```
feature/<short-description>   # new features
fix/<short-description>        # bug fixes
chore/<short-description>      # tooling, deps, docs
release/<version>              # release prep
```

Examples: `feature/phishing-school-page`, `fix/nav-dropdown-mobile`, `chore/upgrade-tailwind`

Use `/git-create-branch` to create branches — it enforces this convention automatically.

## Claude Skills

Custom skills live under `.claude/skills/<skill-name>/SKILL.md` (flat — no nested subdirectories).

### Naming Convention

Skills follow the pattern: `<technology>-<verb>-<descriptor>`

- **Technology prefix** — the tool or framework the skill interacts with (`git`, `github`, `nextjs`, `react`, `docs`, `claude`)
- **Verb** — the action being performed (`create`, `list`, `merge`, `review`, `audit`, `update`, `close`)
- **Descriptor** — the resource or artifact being acted on (`branch`, `issue`, `pr`, `release`, `component`, `page`, `memory`)

Examples: `/github-create-pr`, `/react-audit-component`, `/nextjs-create-page`

### Available Skills

| Skill | Description |
|---|---|
| `/git-create-branch` | Create a branch following the naming convention |
| `/github-create-issue` | Create a labeled GitHub issue |
| `/github-close-issue` | Close an issue with a reason comment |
| `/github-create-pr` | Create a PR with standardized template |
| `/github-review-pr` | Review a PR for bugs and quality |
| `/github-merge-pr` | Squash-merge a PR |
| `/github-list-issues` | List/filter open issues |
| `/github-create-release` | Generate changelog and draft a release |
| `/react-create-component` | Scaffold a shadcn/ui component |
| `/react-audit-component` | Audit a component for a11y, perf, conventions |
| `/nextjs-create-page` | Scaffold an App Router page with loading/error |
| `/nextjs-create-api-route` | Scaffold an API route handler |
| `/docs-create-adr` | Create an Architecture Decision Record |
| `/docs-create-standard` | Draft a new coding standard |
| `/claude-update-memory` | Sync learnings to Claude memory |

## GitHub Issue Labels

Labels attach to both **issues and pull requests**. Use them for triage, filtering, and grouping work in release notes.

### Primary labels (every issue/PR gets exactly one)

| Label | Apply when… |
|---|---|
| `bug` | Something is broken or behaving incorrectly |
| `feature` | New functionality being added (not yet existing) |
| `chore` | Maintenance work — dependency upgrades, tooling, refactors, CI |
| `docs` | Documentation-only changes (no code impact) |
| `security` | A vulnerability, secret exposure, or auth/CSP concern |

### Secondary labels (add alongside a primary label)

| Label | Apply when… |
|---|---|
| `ux` | Visual, layout, or interaction design changes |
| `blocked` | Cannot proceed — waiting on another issue, PR, or external decision |

### Triage labels (GitHub defaults)

| Label | Apply when… |
|---|---|
| `duplicate` | This issue or pull request already exists |
| `enhancement` | New feature or request (prefer `feature` for new work) |
| `documentation` | Improvements or additions to documentation (prefer `docs` for new work) |
| `good first issue` | Good for newcomers — low complexity, well-scoped |
| `help wanted` | Extra attention is needed from contributors |
| `invalid` | The issue doesn't seem right or isn't reproducible |
| `question` | Further information is requested before work can begin |
| `wontfix` | This will not be worked on |

**Rules:**
- Every issue should have exactly one primary label (`bug`, `feature`, `chore`, `docs`, or `security`)
- `ux` and `blocked` are secondary — add alongside a primary label, not instead of one
- PRs inherit the label of the issue they close; add one directly if there is no linked issue
- Prefer `feature` over `enhancement` and `docs` over `documentation` for consistency
- Triage labels (`duplicate`, `invalid`, `wontfix`, `question`) can be used standalone when closing or triaging

## ESLint

Uses ESLint 9 flat config (`eslint.config.js`). Run `pnpm lint` before committing.
