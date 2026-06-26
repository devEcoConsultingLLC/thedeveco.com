# thedeveco.com

Professional website for **devEco Consulting LLC**, a developer relations consultancy helping organizations build thriving developer communities.

**Live site**: [thedeveco.com](https://thedeveco.com)

---

## Tech Stack

- **Framework**: Vue 3 with Composition API (`<script setup>`)
- **Language**: TypeScript
- **Build**: Vite 7
- **Routing**: Vue Router 4 (history mode)
- **State**: Pinia
- **Icons**: Font Awesome 7 (solid + brands)
- **Typography**: JetBrains Mono (Google Fonts)
- **Deployment**: GitHub Pages via GitHub Actions

## Quick Start

```bash
# Prerequisites: Node.js 20+ or 22+
git clone https://github.com/devEcoConsultingLLC/thedeveco.com.git
cd thedeveco.com
npm install
npm run dev
# Open http://localhost:5173
```

## Available Scripts

```bash
npm run dev          # Development server with hot reload
npm run build        # Type-check + production build
npm run build-only   # Production build only (used by CI)
npm run preview      # Preview production build locally
npm run type-check   # TypeScript validation
npm run lint         # ESLint with auto-fix
```

## Deployment

The site deploys automatically to GitHub Pages when changes are pushed to `main`.

**Pipeline**: Push to `main` → GitHub Actions (`.github/workflows/deploy.yml`) → `npm ci && npm run build-only` → publish `dist/` to `gh-pages` branch

Custom domain `thedeveco.com` is configured via CNAME files. SPA routing on GitHub Pages is handled by a `404.html` redirect that preserves paths for Vue Router.

## Project Structure

```
src/
├── App.vue                    # Root component with global styles and CSS variables
├── main.ts                    # Entry: Font Awesome, Pinia, Router, SPA redirect handler
├── assets/                    # CSS files
├── components/
│   ├── layout/
│   │   ├── HeaderNav.vue      # Sticky nav with mobile menu + Community dropdown
│   │   └── FooterNav.vue      # 4-column footer
│   └── ui/
│       ├── LogoCarousel.vue    # Auto-scrolling partner/client logo carousel
│       ├── EcosystemTile.vue  # Clickable card for ecosystem listings
│       ├── EcosystemModal.vue # Detail modal for ecosystem listings
│       ├── CookieConsent.vue  # Fixed-bottom cookie consent notice
│       └── ProcessVisualization.vue
├── router/
│   └── index.ts               # 13 routes
├── stores/
│   └── counter.ts             # Pinia boilerplate
└── views/
    ├── HomeView.vue            # Landing page with hex grid animation
    ├── ConsultancyView.vue     # Services, process, team bios
    ├── EcosystemView.vue       # Ecosystem directory (20+ listings)
    ├── DevXRL.vue              # DevXRL audit product page (scroll-snap)
    ├── SMRL.vue               # SMRL audit product page (scroll-snap)
    ├── G2MRL.vue              # G2MRL audit product page (scroll-snap)
    ├── TRL.vue                # TRL audit product page (scroll-snap)
    ├── CommunityView.vue       # Community page with logo animation
    ├── TeamView.vue            # Team/About page with rocket-portal animation
    ├── ContactView.vue         # Contact form and office hours
    ├── HeimdallView.vue        # Hidden easter egg page (not in nav)
    ├── PrivacyView.vue         # Privacy and cookie statement (linked from footer)
    └── AboutView.vue           # Boilerplate placeholder (not in router)

public/
├── clients/                   # Partner/client logo images
├── images/                    # Site logos
├── projects/                  # Product images
├── team/                      # Team member photos
├── trail-sandiego/            # EDGE AI Trail game (static Next.js export)
├── 404.html                   # SPA redirect for GitHub Pages
├── CNAME                      # Custom domain
└── .nojekyll                  # Prevents Jekyll processing
```

## Site Pages

| Page | Path | Nav location | Description |
|------|------|--------------|-------------|
| Home | `/` | Logo | Landing with animated hex grid, services overview, team, partners |
| Consultancy | `/consultancy` | About dropdown | Detailed service offerings, process, team bios |
| Ecosystem | `/ecosystem` | About dropdown | Directory of partners, clients, supported creators, collaborators |
| DevXRL | `/devxrl` | Audits dropdown (new tab) | DevXRL Audit product page with interactive readiness scale |
| SMRL | `/smrl` | Audits dropdown (new tab) | Social Media Readiness Levels audit product page |
| G2MRL | `/g2mrl` | Audits dropdown (new tab) | Go-to-Market Readiness Levels audit product page |
| TRL | `/trl` | Audits dropdown (new tab) | Technology Readiness Levels audit product page |
| Community | `/community` | Community | Community values, events, Discord CTA |
| Team | `/team` | About dropdown | The Collective team section, rocket-portal animation |
| Contact | `/contact` | Contact Us CTA | Contact options, form, virtual office hours |
| Privacy | `/privacy` | Footer link | Privacy and cookie statement (linked from footer) |

`/about` redirects to `/team` to preserve backward compatibility with older links. A hidden `/heimdall` easter egg route exists but is not linked from the nav.

## Design System

- **Typeface**: JetBrains Mono exclusively
- **Border radius**: Zero throughout (sharp corners everywhere)
- **Primary palette**: Teal (`#4db3a8`), Navy (`#1e3a3a`)
- **Accents**: Pink (`#e84a7f`), Yellow (`#f7c948`), Cyan (`#4ecdc4`)
- **Components**: Stamps (dashed borders), Badges (solid), Cards (hover shadow offset)
- **Layout**: 1200px max-width, CSS Grid, responsive at 900px/768px/480px

Full design system details are documented in `CLAUDE.md`.

## Adding Client Logos

Client logos live in the auto-scrolling carousel. They are defined as a TypeScript array in the `<script setup>` block of `src/components/ui/LogoCarousel.vue`; each entry has `href`, `src`, `alt`, and `darkBg`.

1. Add image file to `/public/clients/` (lowercase filename, PNG/SVG/JPG)
2. Add an entry to the `logos` array in `src/components/ui/LogoCarousel.vue`:
   ```ts
   { href: 'https://company.com', src: '/clients/company-logo.png', alt: 'Company Name', darkBg: false }
   ```
3. Set `darkBg: true` if the logo needs a dark background to be legible
4. Test locally, commit both files

## Contributing

1. Create feature branch from `main`
2. Follow patterns in `CLAUDE.md`
3. Test locally with `npm run dev`
4. Verify production build with `npm run build`
5. Submit PR with Conventional Commit messages

## Bible Files

This project uses bible files for session-agnostic development:

- **CLAUDE.md** — Primary onboarding document for Claude sessions (architecture, patterns, conventions)
- **README.md** — This file (project overview and quick reference)
- **ROADMAP.md** — Planned features and improvements
- **CHANGELOG.md** — History of changes

## Contact

- **Email**: hello@thedeveco.com
- **Discord**: [discord.gg/deveco](https://discord.gg/deveco)
- **LinkedIn**: [linkedin.com/company/thedeveco](https://linkedin.com/company/thedeveco)

---

*devEco Consulting LLC — DevRel from the workshop floor.*

*Last updated: 2026-06-26*
