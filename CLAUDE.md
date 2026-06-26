# CLAUDE.md — thedeveco.com Session Bible

> **Purpose**: This file is the single-source onboarding document for any Claude Chat or Claude Code session working on thedeveco.com. Read this file FIRST before making any changes. It contains everything needed to understand the codebase, make changes safely, and produce work that matches the established patterns.

---

## Project Identity

**Repository**: `github.com/devEcoConsultingLLC/thedeveco.com`
**Live URL**: `https://thedeveco.com`
**Owner**: devEco Consulting LLC (Robert Wolff, Founder)
**Purpose**: Professional website for devEco Consulting — a developer relations consultancy and community platform

---

## Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | Vue 3 (Composition API + `<script setup>`) | ^3.5.22 |
| Language | TypeScript (strict) | ~5.9.0 |
| Build | Vite | ^7.1.7 |
| Router | Vue Router 4 (history mode) | ^4.5.1 |
| State | Pinia (installed, only boilerplate store exists) | ^3.0.3 |
| Icons | Font Awesome 7 (solid + brands, via `@fortawesome/vue-fontawesome`) | ^7.1.0 |
| Typeface | JetBrains Mono (Google Fonts CDN, loaded in App.vue) | — |
| Node | ^20.19.0 or >=22.12.0 | — |

---

## Deployment Pipeline

**Host**: GitHub Pages (gh-pages branch)
**Trigger**: Push to `main` branch
**Workflow**: `.github/workflows/deploy.yml`

```
Push to main → GitHub Actions → npm ci → npm run build-only → peaceiris/actions-gh-pages@v4 → dist/ → gh-pages branch
```

**Key deployment details**:
- The workflow uses Node 22 and runs `npm run build-only` (Vite build without type-check, for speed)
- Output publishes to `gh-pages` branch via `peaceiris/actions-gh-pages@v4`
- Custom domain configured via `CNAME` files (root and `/public/CNAME`) containing `thedeveco.com`
- SPA routing on GitHub Pages uses a `404.html` redirect trick: stores path in `sessionStorage`, redirects to `/`, and `main.ts` picks it up after router is ready
- `/public/.nojekyll` prevents GitHub Pages from processing files through Jekyll

---

## Project Structure

```
thedeveco.com/
├── .github/
│   ├── scripts/              # Python scripts for event updates (unused currently)
│   │   ├── update_events.py
│   │   └── update_events_local.py
│   └── workflows/
│       └── deploy.yml        # GitHub Pages deployment workflow
├── public/                   # Static assets (copied as-is to dist/)
│   ├── 404.html              # SPA redirect for GitHub Pages
│   ├── .nojekyll             # Prevents Jekyll processing
│   ├── CNAME                 # Custom domain: thedeveco.com
│   ├── favicon.ico
│   ├── clients/              # Partner/client logo images
│   ├── images/               # Site images (logos)
│   │   ├── deveco-community-logo.png
│   │   └── deveco-consulting-logo.png
│   ├── projects/             # Product screenshots/logos
│   │   ├── portalnetwork.png
│   │   ├── portalnetwork-new.png
│   │   ├── b3vy.png
│   │   └── launchcue.png
│   ├── team/                 # Team member photos
│   │   ├── robert.jpg
│   │   ├── moheeb.jpg
│   │   └── tory.jpg
│   └── trail-sandiego/       # Static export of EDGE AI Trail game (Next.js)
├── src/
│   ├── App.vue               # Root component: HeaderNav + RouterView + FooterNav
│   │                         # Contains ALL global CSS variables and base styles
│   ├── main.ts               # Entry point: Font Awesome setup, Pinia, Router, SPA redirect handler
│   ├── assets/
│   │   ├── base.css          # Vue scaffolding CSS (mostly overridden by App.vue)
│   │   ├── main.css          # Minimal app CSS (mostly overridden by App.vue)
│   │   └── logo.svg          # Unused Vue default logo
│   ├── components/
│   │   ├── layout/
│   │   │   ├── HeaderNav.vue # Sticky nav with mobile hamburger + About dropdown + Audits dropdown + Community dropdown + Explore dropdown
│   │   │   └── FooterNav.vue # 5-column footer: brand + About, Audits, Community, Explore columns
│   │   ├── ui/
│   │   │   ├── LogoCarousel.vue        # Auto-scrolling logo carousel (pause-on-hover, CSS animation)
│   │   │   ├── EcosystemTile.vue      # Clickable tile card for ecosystem listings
│   │   │   ├── EcosystemModal.vue     # Detail modal for ecosystem listings (Teleport to body)
│   │   │   └── ProcessVisualization.vue # 4-step process with FA icons (unused on current pages)
│   │   ├── HelloWorld.vue    # Vue scaffolding (unused)
│   │   ├── TheWelcome.vue    # Vue scaffolding (unused)
│   │   ├── WelcomeItem.vue   # Vue scaffolding (unused)
│   │   └── icons/            # Vue scaffolding icon components (unused)
│   ├── router/
│   │   └── index.ts          # 12 routes: / /consultancy /ecosystem /devxrl /smrl /g2mrl /trl /community /team /about (redirect) /contact /heimdall
│   ├── stores/
│   │   └── counter.ts        # Pinia boilerplate (unused)
│   └── views/
│       ├── HomeView.vue       # Landing page with hex grid animation, services, process, team, CTA
│       ├── ConsultancyView.vue # Services page with animated logo, services grid, process, team bios
│       ├── EcosystemView.vue  # Data-driven ecosystem directory (20+ listings, tile+modal)
│       ├── DevXRL.vue         # Full-page scroll-snap DevXRL audit product page
│       ├── SMRL.vue           # Full-page scroll-snap SMRL (Social Media Readiness Levels) audit product page
│       ├── G2MRL.vue          # Full-page scroll-snap G2MRL (Go-to-Market Readiness Levels) audit product page
│       ├── TRL.vue            # Full-page scroll-snap TRL (Technology Readiness Levels) audit product page
│       ├── CommunityView.vue  # Community page with logo slam animation
│       ├── TeamView.vue       # Team/About page with rocket-portal SVG animation and Community Engine widget
│       ├── ContactView.vue    # Contact form (simulated submission, no backend)
│       ├── HeimdallView.vue   # Hidden easter egg page (not in nav)
│       └── AboutView.vue      # Boilerplate placeholder (not in router)
├── index.html                # Vite entry HTML
├── package.json
├── vite.config.ts            # Vite config: @ alias, vendor chunk splitting, base: '/'
├── tsconfig.json             # References tsconfig.app.json and tsconfig.node.json
├── tsconfig.app.json
├── tsconfig.node.json
├── eslint.config.ts
├── env.d.ts                  # Vite client types
├── CNAME                     # Domain file (root level)
├── README.md                 # Project README
├── CLAUDE.md                 # THIS FILE — session bible
├── ROADMAP.md                # Project roadmap
├── CHANGELOG.md              # Change history
├── animation.html            # Standalone hex animation prototype (not part of build)
├── hexanimate.html           # Standalone hex animation prototype (not part of build)
└── mockup.html               # Standalone design mockup (not part of build)
```

---

## Route Map

| Path | View Component | Load | Nav Label |
|------|---------------|------|-----------|
| `/` | HomeView.vue | Eager | (logo click) |
| `/consultancy` | ConsultancyView.vue | Lazy | About → Consultancy (dropdown) |
| `/ecosystem` | EcosystemView.vue | Lazy | About → Ecosystem (dropdown) |
| `/devxrl` | DevXRL.vue | Lazy | Audits → DevXRL (dropdown, new tab) |
| `/smrl` | SMRL.vue | Lazy | Audits → SMRL (dropdown, new tab) |
| `/g2mrl` | G2MRL.vue | Lazy | Audits → G2MRL (dropdown, new tab) |
| `/trl` | TRL.vue | Lazy | Audits → TRL (dropdown, new tab) |
| `/community` | CommunityView.vue | Lazy | Community (has dropdown) |
| `/team` | TeamView.vue | Lazy | About → Team (dropdown) |
| `/about` | (redirect) | — | (redirects to /team) |
| `/contact` | ContactView.vue | Lazy | Contact Us (CTA button) |
| `/heimdall` | HeimdallView.vue | Lazy | (hidden — easter egg, not in nav) |

**Nav order**: About (dropdown) | Audits (dropdown) | Community (dropdown) | Explore (dropdown) | Contact Us (CTA)

The About nav item is a non-clickable dropdown trigger containing: Team (/team), Consultancy (/consultancy), Ecosystem (/ecosystem). Same-tab navigation via `router-link`.

The Audits nav item is a non-clickable dropdown trigger containing the four readiness audits: DevXRL (/devxrl), SMRL (/smrl), G2MRL (/g2mrl), TRL (/trl). These render as anchor tags with `target="_blank"` so each audit opens in a new tab.

The Community nav item is a clickable `router-link` to /community with a hover dropdown containing external social links: Discord, GitHub, LinkedIn, YouTube, Instagram, Twitch.

The Explore nav item is a non-clickable dropdown trigger containing external links only: devEco.io, devEco.app.

---

## Design System

### Typography
- **Font**: JetBrains Mono exclusively (loaded from Google Fonts in App.vue)
- **Weights**: 400, 500, 700 (+ 600, 800 on DevXRL page)
- **Scale**: `clamp()` for responsive headings; `0.875rem` body text; `0.75rem` labels/small

### Color Palette

**Brand Colors**:
| Token | Hex | Usage |
|-------|-----|-------|
| `--teal-light` | `#4db3a8` | Primary accent, links, highlights |
| `--teal` | `#3d9991` | Buttons, active states |
| `--teal-dark` | `#2a6b6b` | Hover states |
| `--navy` | `#1e3a3a` | Dark backgrounds, text headings, logo frame |

**Accent Colors**:
| Token | Hex | Usage |
|-------|-----|-------|
| `--accent-pink` | `#e84a7f` | Stamps, badges |
| `--accent-yellow` | `#f7c948` | Highlights, yellow accent |
| `--accent-cyan` | `#4ecdc4` | Cyan accent |
| `--accent-blue` | `#5bc0eb` | Blue accent |

**Neutrals**:
| Token | Hex | Usage |
|-------|-----|-------|
| `--text-dark` | `#1a2e2e` | Body text, footer background |
| `--text-mid` | `#4a5e5e` | Secondary text |
| `--text-light` | `#7a8e8e` | Muted text |
| `--border` | `#c5d5d5` | Borders, dividers |
| `--bg-light` | `#eef3f6` | Page backgrounds |
| `--bg-white` | `#f8fafb` | Section backgrounds |

**DevXRL Page** uses its own parallel palette (defined in DevXRL.vue):
| Token | Hex |
|-------|-----|
| `--dxrl-bg` | `#f0ede4` (parchment) |
| `--dxrl-teal` | `#4db3a8` |
| `--dxrl-gold` | `#d4a843` |
| `--dxrl-red` | `#c45b5b` |
| `--dxrl-ink` | `#1e3a3a` |

### Component Patterns

**Zero border-radius** is used throughout the entire site. No rounded corners anywhere.

**Stamps** (dashed border tags):
```html
<span class="stamp stamp-teal">Label</span>
<span class="stamp stamp-pink">Label</span>
<span class="stamp stamp-navy">Label</span>
```

**Badges** (solid background tags):
```html
<span class="badge badge-teal">Label</span>
<span class="badge badge-pink">Label</span>
<span class="badge badge-navy">Label</span>
```

**Cards** (hoverable with offset box-shadow):
```html
<div class="card">Content</div>
<!-- Hover: border-color: teal, box-shadow: 6px 6px 0 teal -->
```

**Buttons**:
```html
<router-link class="btn btn-primary">Primary</router-link>  <!-- Teal bg, navy shadow -->
<router-link class="btn btn-secondary">Secondary</router-link>  <!-- Transparent, navy border -->
```

**Section structure**:
```html
<section>
  <div class="container">
    <div class="section-header">
      <h2>Title</h2>  <!-- Gets ::after teal underline -->
    </div>
    <!-- Content -->
  </div>
</section>
```

### Layout
- **Max width**: 1200px (`.container`)
- **Grids**: CSS Grid with `auto-fit, minmax()` for responsive columns
- **Breakpoints**: 900px (grid collapse), 768px (mobile), 480px (small mobile)
- **Spacing**: CSS custom properties (`--space-xs` through `--space-2xl`)

---

## Conventions & Patterns

### Vue Component Conventions
- All components use `<script setup lang="ts">` (Composition API)
- Scoped styles (`<style scoped>`) for component-specific CSS
- Unscoped styles (`<style>`) used for SVG animations that need to target child elements
- Global CSS variables defined in `App.vue`'s unscoped `<style>` block
- Font Awesome icons registered globally in `main.ts` and used as `<font-awesome-icon :icon="['fas', 'icon-name']" />`

### File Naming
- Views: `PascalCase` + `View` suffix (e.g., `HomeView.vue`, `ConsultancyView.vue`)
- Components: `PascalCase` (e.g., `ClientLogos.vue`, `HeaderNav.vue`)
- Exception: `DevXRL.vue` (product name, not a generic view)

### Image Assets
- Client/partner logos: `/public/clients/` — lowercase filenames, PNG/SVG/JPG
- Team photos: `/public/team/` — `firstname.jpg`
- Product logos: `/public/projects/`
- Site images: `/public/images/`
- Reference in templates as `/clients/filename.ext` (no `public/` prefix)

### Client Logo Management
Client logos in `LogoCarousel.vue` are defined as a TypeScript array in the `<script setup>` block. Each entry has `href`, `src`, `alt`, and `darkBg`. To add a logo:
1. Add image file to `/public/clients/` (lowercase filename)
2. Add an entry to the `logos` array in `src/components/ui/LogoCarousel.vue`
3. For logos that need a dark background, set `darkBg: true`

### Ecosystem Directory
Ecosystem listings in `EcosystemView.vue` are defined as a TypeScript array of `EcosystemListing` objects. Each listing has:
- `id`, `name`, `logo?`, `category` (partner | support | client | collaborator)
- `shortDescription`, `tags?`, `longDescription?`, `website?`, `socialLinks?`, `highlights?`
- Listings are grouped into sections by category and rendered as `EcosystemTile` + `EcosystemModal` pairs

---

## Git Workflow

### Branch Strategy
- `main`: Production branch. Pushes trigger auto-deploy to GitHub Pages
- Feature branches: Create from `main`, merge via PR

### Commit Convention
Use Conventional Commits:
```
feat: add new blog section
fix: correct mobile nav z-index
style: update hero section spacing
docs: update CLAUDE.md with new route
refactor: extract service card component
chore: update dependencies
```

### Deployment
1. Create feature branch from `main`
2. Make changes, test locally with `npm run dev`
3. Run `npm run build` to verify production build
4. Push branch, create PR to `main`
5. Merge PR — GitHub Actions auto-deploys to GitHub Pages

---

## Development Commands

```bash
npm run dev          # Start dev server (localhost:5173)
npm run build        # Type-check + Vite build (full validation)
npm run build-only   # Vite build only (what CI runs)
npm run preview      # Preview production build locally
npm run type-check   # TypeScript checking via vue-tsc
npm run lint         # ESLint with auto-fix
```

---

## Known Issues & Technical Debt

1. **Unused Vue scaffolding files** — `HelloWorld.vue`, `TheWelcome.vue`, `WelcomeItem.vue`, icon components, `AboutView.vue`, `counter.ts` store
2. **base.css and main.css are largely overridden** — Global styles in `App.vue` take precedence; these files contain leftover Vue create-app defaults
3. **Contact form has no backend** — `submitForm()` in `ContactView.vue` simulates a 2-second delay, no actual email/API integration
4. **ProcessVisualization.vue component exists but is unused** — The home page and consultancy page have their own inline process sections
5. **DevXRL page overrides global styles** — Has its own complete reset and design system within `.devxrl` scope; adds `devxrl-page` class to `<html>` for scroll-snap
6. **Some animation styles are unscoped** — HomeView, CommunityView, ConsultancyView, and TeamView use unscoped `<style>` blocks for SVG animations. Be careful not to create class name collisions
7. **CommunityView has a mislabeled button** — A "View Products" button on /community routes to /team. The destination is correct after the nav restructure, but the label is stale and should be updated to something like "Meet the Team" in a follow-up.
8. **Org migration note**: In April 2026, the repository was transferred from `thedeveco` (legacy org) to `devEcoConsultingLLC` (current org, public repo). GitHub's permanent redirect means old links to `github.com/thedeveco/thedeveco.com` continue to work. The `thedeveco` org may or may not still exist depending on subsequent decisions, but the canonical home for this codebase is `devEcoConsultingLLC`. The repo is currently public; there is an open consideration to flip it to private later, which would require the org to be on GitHub Team plan or higher.

---

## Page-by-Page Reference

### HomeView (/)
- **Hero**: Two-column grid with text left, animated hex grid right
- **Hex grid animation**: Procedurally generated SVG hexagons with 6 phases (appear → breathe → pulse → explode outward → logo frame expands → logo hexes pop in → color shift → random flash)
- **Logo Carousel**: `LogoCarousel` component — auto-scrolling partner/client logos between hero and services
- **Services**: 6-card grid with stamps
- **Process**: 4-step numbered grid with hexagonal step indicators
- **CTA**: Navy background, centered
- *Note*: The Collective team section was moved to the About page; Partners & Clients grid replaced by LogoCarousel

### ConsultancyView (/consultancy)
- **Hero**: Navy background with animated SVG logo (staggered keyframe entrance)
- **Services**: 6-card grid with service lists
- **Process**: Same 4-step layout as home
- **Team**: Full bios with expandable read-more on mobile
- **CTA**: Navy background

### EcosystemView (/ecosystem)
- **Hero**: Navy, text only
- **Sections**: 4 category sections (Partners, Support, Clients, Collaborators) each with grid of `EcosystemTile` components
- **Modal**: Click tile → `EcosystemModal` with full details (Teleport to body, ESC to close, body scroll lock)

### DevXRL (/devxrl)
- **Full-page scroll-snap** with 10 sections (hero, problem, pipeline, scale, journey, deliverables, methodology, process, trl, cta) and navigation dots synced via IntersectionObserver
- **Own design system**: Parchment background, `--dxrl-*` CSS variable set scoped on `.devxrl`, SVG noise overlay scoped to `html.devxrl-page`
- **Interactive elements**: Clickable scale table rows (single-row expand), canvas-drawn journey graph with three-way toggle (BOTH, CURRENT STATE, AFTER AUDIT), intersection observer animations
- **Pipeline diagram**: Vertical flow with clip-path trapezoids representing the activation funnel (gold) and conversion funnel (teal), plus a branch node for Social and Discord channels
- **TRL section**: Side-by-side explainer for Technology Readiness Levels and Go-to-Market Readiness (G2MRL), with a 3-badge summary row
- **Completely self-contained**: All class names prefixed `devxrl-`, styles namespaced within `.devxrl` scope; adds `devxrl-page` class to `<html>` on mount and removes on unmount

### SMRL (/smrl)
- **Full-page scroll-snap** with 10 sections (hero, problem, pipeline, scale, kb, deliverables, methodology, process, trl, cta) and navigation dots synced via IntersectionObserver
- **Own design system**: Parchment background, `--smrl-*` CSS variable set scoped on `.smrl`, SVG noise overlay scoped to `html.smrl-page`. Mirrors the DevXRL pattern but with its own namespace.
- **Interactive elements**: Clickable scale table rows (single-row expand), intersection observer fade-in and stagger animations. No canvas graph (this is the main structural difference from DevXRL).
- **Pipeline diagram**: Vertical flow with clip-path trapezoids representing the production funnel (gold, CONTENT ENGINE to DISTRIBUTION) and relationship funnel (teal, LISTEN/RESPOND/BUILD to RELATIONSHIP to PIPELINE), plus a branch node for LinkedIn and YouTube channels.
- **KB Foundation section**: Side-by-side layout. Left column explains how the SMRL audit plants the source-of-truth knowledge base that devEco builds and maintains. Right column is a vertical diagram: Upstream Sources (Social Content, Brand Voice, Positioning, Customer Stories) flow into a hexagonal KNOWLEDGE BASE core, which fans out to Downstream Surfaces (AI Assistants, Content Gen, Sales & Support, White-Label Tools).
- **TRL section**: Side-by-side layout with a DevXRL mini-table on the left and TRL plus G2MRL intro on the right, followed by a row of three badges (DevXRL, TRL, G2MRL).
- **Completely self-contained**: All class names prefixed `smrl-`, styles namespaced within `.smrl` scope; adds `smrl-page` class to `<html>` on mount and removes on unmount.

### G2MRL (/g2mrl)
- **Full-page scroll-snap** with 10 sections (hero, problem, pipeline, scale, kb, deliverables, methodology, process, trl, cta) and navigation dots synced via IntersectionObserver
- **Own design system**: Parchment background, `--g2mrl-*` CSS variable set scoped on `.g2mrl`, SVG noise overlay scoped to `html.g2mrl-page`. Mirrors the SMRL pattern but with its own namespace.
- **Interactive elements**: Clickable scale table rows (single-row expand), intersection observer fade-in and stagger animations. No canvas graph.
- **Pipeline diagram**: Vertical flow with clip-path trapezoids representing the acquisition funnel (gold, DEMAND GENERATION to QUALIFICATION) and revenue funnel (teal, COMMIT/CLOSE to EXPAND to ADVOCATE), plus a branch node for Direct, Channel, and Ecosystem paths.
- **Data Foundation section**: Side-by-side layout. Left column explains how the G2MRL audit surfaces hidden GTM intelligence and how devEco builds the structured knowledge base that feeds sales AI. Right column is a vertical diagram: Upstream Sources (Win Stories, Objections, Competitive Intel, Pricing Signals) flow into a hexagonal GTM INTELLIGENCE core, which fans out to Downstream Surfaces (Sales AI Tools, Rep Onboarding, Forecast Scoring, CS Playbooks). Tag label is "THE DATA FOUNDATION" and core title is "GTM INTELLIGENCE" (differs from SMRL's "KNOWLEDGE BASE").
- **TRL section**: Side-by-side layout with a DevXRL mini-table on the left and SMRL plus TRL intros on the right, followed by a row of three badges (DevXRL, SMRL, TRL). G2MRL is framed as the top-down counterpart to DevXRL's bottom-up motion.
- **Completely self-contained**: All class names prefixed `g2mrl-`, styles namespaced within `.g2mrl` scope; adds `g2mrl-page` class to `<html>` on mount and removes on unmount.

### TRL (/trl)
- **Full-page scroll-snap** with 10 sections (hero, problem, pipeline, scale, kb, deliverables, methodology, process, trl, cta) and navigation dots synced via IntersectionObserver
- **Own design system**: Parchment background, `--trl-*` CSS variable set scoped on `.trl`, SVG noise overlay scoped to `html.trl-page`. Mirrors the G2MRL and SMRL pattern but with its own namespace.
- **Interactive elements**: Clickable scale table rows (single-row expand), intersection observer fade-in and stagger animations. No canvas graph.
- **Pipeline diagram**: Vertical flow with clip-path trapezoids representing the development funnel (gold, LAB PROTOTYPE to FIELD VALIDATION) and deployment funnel (teal, QUALIFIED to LAUNCH to DEPLOYED), plus a branch node for Safety, Regulatory, and Industry certification paths.
- **Technical Foundation section**: Side-by-side layout. Left column explains how the TRL audit surfaces undocumented engineering decisions and how devEco preserves the institutional asset. Right column is a vertical diagram: Upstream Sources (Engineering Decisions, Test Results, IP Portfolio, Validation Data) flow into a hexagonal TECHNICAL KNOWLEDGE core, which fans out to Downstream Surfaces (Investor Data Rooms, Grant Filings, Engineering Onboarding, Tax Credit Audits). Tag label is "THE TECHNICAL FOUNDATION" and core title is "TECHNICAL KNOWLEDGE" (differs from SMRL's "KNOWLEDGE BASE" and G2MRL's "GTM INTELLIGENCE").
- **What-comes-after section**: Uses `.trl-next-*` class prefix instead of the `.{prefix}-trl-*` pattern used on the other audit pages (renamed to avoid the awkward `.trl-trl-*` double-trl that would result from the shared naming scheme). Side-by-side layout with a G2MRL mini-table on the left and DevXRL plus SMRL intros on the right, followed by a row of three badges (G2MRL, DevXRL, SMRL). TRL is framed as the technical foundation that G2MRL converts into revenue.
- **Scale table**: Canonical NASA TRL 1-9 with devEco's commercial translation layer. The fourth column "Commercial Translation" differs from the "Product Status", "Presence Status", "GTM Status" columns on the other audit pages and explicitly ties each level to a funding or commercial milestone (e.g., TRL 5 = Series A Readiness, TRL 7 = Pilot Customers Live).
- **Completely self-contained**: All class names prefixed `trl-`, styles namespaced within `.trl` scope; adds `trl-page` class to `<html>` on mount and removes on unmount.

### CommunityView (/community)
- **Hero**: Teal background with logo slam animation (frame slam → hex pop → glitch → particles → text slide → color shift)
- **Values**: 6-card grid
- **Events**: 4-card grid
- **CTA**: Navy background

### TeamView (/team)
- **Hero**: Navy with rocket-through-portal SVG animation (6s loop with gear emergence), "About Us" label, "The people behind devEco." heading
- **Team**: The Collective — 2-member grid with hexagonal clipped photos, badges, roles, bios, and links (moved from HomeView)
- **Community Engine Explorer**: Interactive widget with 6-cell node grid (5 clickable nodes + 1 hub), expandable detail panel, pulse dot animations, and CTA buttons
- *Note*: `/about` redirects to `/team` to preserve backward compatibility with older external links.

### ContactView (/contact)
- **Hero**: Teal, centered
- **Contact options**: 3-card grid (Email preferred, Discord, LinkedIn)
- **Form**: Name, email, company, service dropdown, message textarea (simulated submission)
- **Office hours**: Navy section with schedule details

---

## Adding a New Page (Step-by-Step)

1. Create view component: `src/views/NewPageView.vue`
2. Add route in `src/router/index.ts`:
   ```ts
   {
     path: '/new-page',
     name: 'new-page',
     component: () => import('../views/NewPageView.vue'),
   }
   ```
3. Add nav link in `src/components/layout/HeaderNav.vue` inside `.nav-links`
4. Optionally add footer link in `src/components/layout/FooterNav.vue`
5. Follow existing view structure: hero section → content sections → CTA section
6. Use scoped styles with existing CSS variables
7. Test locally, verify build, push to feature branch

---

## Adding New Font Awesome Icons

1. Find icon name at [fontawesome.com](https://fontawesome.com/icons)
2. Import in `src/main.ts`:
   ```ts
   import { faNewIcon } from '@fortawesome/free-solid-svg-icons'
   // or from '@fortawesome/free-brands-svg-icons'
   ```
3. Add to `library.add(...)` call
4. Use in template: `<font-awesome-icon :icon="['fas', 'new-icon']" />`

---

## Session Handoff Protocol

When ending a session that made changes:
1. Update this `CLAUDE.md` if any structural changes were made
2. Update `CHANGELOG.md` with what was done
3. Update `ROADMAP.md` if planned items were completed or new items identified
4. Provide clear commit messages following Conventional Commits
5. Note any incomplete work or follow-up items

---

## Easter Eggs

### Heimdall Portal
- **Trigger**: Hover over the top-left inner hexagon of the animated logo on the home page, then scroll the mouse wheel forward (up). Requires ~3-4 scroll ticks (threshold: `SCROLL_THRESHOLD = -150` accumulated deltaY).
- **Effect**: A teal portal animation expands from the hexagon's position, then navigates to `/heimdall`.
- **Route**: `/heimdall` — lazy-loaded `HeimdallView.vue`
- **Page**: Placeholder with "You found the Bifrost" messaging. Not linked from nav or footer.
- **Implementation**: Portal hex is the second `<polygon>` in `<g id="logoHexes">` in HomeView.vue, identified by `ref="portalHex"`. Event listeners attached in `onMounted`, cleaned up in `onUnmounted`. Portal overlay uses `<Teleport to="body">` with unscoped CSS animations.

---

*Last updated: 2026-06-26*
*Bible file version: 1.0*
