# ROADMAP.md — thedeveco.com

> **Purpose**: Living document tracking planned features, improvements, and known work items. Updated each session.

---

## Current State (as of 2026-06-25)

The site is live at thedeveco.com, deployed via GitHub Pages. It has 10 nav-reachable pages (Home, Consultancy, Ecosystem, DevXRL, SMRL, G2MRL, TRL, Community, Team, Contact) plus an `/about` redirect to `/team` and a hidden `/heimdall` easter egg, for 12 routes total. The four readiness audit pages (DevXRL, SMRL, G2MRL, TRL) and the About, Audits, Community, and Explore nav structure are live. Core structure is stable. No active feature branches.

---

## Priority: Technical Debt & Cleanup

These items should be addressed before major new features:

### High Priority

- [ ] **Remove unused Vue scaffolding files** — Delete `HelloWorld.vue`, `TheWelcome.vue`, `WelcomeItem.vue`, all icon components in `src/components/icons/`, `AboutView.vue`, and `src/stores/counter.ts`. Clean up any imports referencing them.
- [ ] **Clean up base.css and main.css** — These contain leftover Vue create-app defaults that are overridden by App.vue. Consolidate needed styles into App.vue and simplify or remove these files.
- [ ] **Remove standalone HTML prototypes** — `animation.html`, `hexanimate.html`, `mockup.html` in the root are development artifacts not part of the build. Archive or delete.

### Medium Priority

- [ ] **Contact form backend integration** — `ContactView.vue` currently simulates form submission with a 2-second delay. Integrate with a form service (Formspree, Netlify Forms, or custom API endpoint).
- [ ] **ProcessVisualization.vue audit** — Component exists but is not used on any page. Either integrate it or remove it to reduce dead code.
- [ ] **SEO and meta tags** — Add page-specific `<title>` and `<meta description>` tags via Vue Router guards or a head management plugin. Currently all pages share the same title from `index.html`.
- [ ] **Accessibility audit** — Review focus management, ARIA labels, keyboard navigation, color contrast across all pages.

### Low Priority

- [ ] **Consolidate animation patterns** — HomeView, CommunityView, ConsultancyView, and ProjectsView each have unique SVG animations with unscoped styles. Consider extracting shared animation utilities or at minimum namespacing class names to prevent future collisions.
- [ ] **DevXRL page scroll-snap behavior** — The scroll-snap is aggressive on some devices. Consider making it optional or adding a fallback for users who find it disorienting.
- [ ] **Image optimization** — Some client logos and team photos could be further compressed. Consider WebP format with PNG fallback.

---

## Planned Features

### Content & Pages

- [ ] **Blog section** — Add a `/blog` route for publishing articles from the Community Operating System (52 principles) and other devEco content. Likely needs a lightweight content system (markdown files with frontmatter, or a headless CMS integration).
- [ ] **Individual blog post pages** — Dynamic routing for `/blog/:slug` with proper meta tags and social sharing.
- [ ] **Case studies page** — Dedicated page showcasing client work with detailed project descriptions and outcomes.
- [ ] **About page** — Replace the boilerplate `AboutView.vue` with a proper about page, or remove the route entirely if covered by other pages.

### Ecosystem & Community

- [ ] **Ecosystem listing management** — Currently all 20+ ecosystem entries are hardcoded in `EcosystemView.vue`. Consider extracting to a JSON/YAML data file or separate TypeScript module for easier content updates.
- [ ] **portalNetwork stats integration** — portalNetwork stats were previously shown on the Products page. That page is now /team (TeamView) and the portalNetwork, B3VY, and LaunchCue product cards were removed. If product cards return to the site, consider pulling live stats from the portalNetwork API instead of hardcoding them.

### Design & UX

- [ ] **Dark mode** — `base.css` already has a `prefers-color-scheme: dark` media query (from Vue scaffolding) but the actual site styles don't support it. Implement proper dark mode using CSS custom properties.
- [ ] **Page transition animations** — Add Vue `<Transition>` on `<RouterView>` for smooth page transitions.
- [ ] **Loading states** — Add skeleton screens or loading indicators for lazy-loaded routes.
- [ ] **Scroll-to-top on navigation** — Vue Router scroll behavior to return to top on route change.

### Performance & Infrastructure

- [ ] **Analytics integration** — Add privacy-respecting analytics (Plausible, Fathom, or similar).
- [ ] **Error monitoring** — Integrate error tracking (Sentry or similar) for production.
- [ ] **Sitemap generation** — Auto-generate `sitemap.xml` for SEO.
- [ ] **RSS feed** — If blog is added, include RSS feed generation.
- [ ] **CI/CD enhancements** — Add type-check and lint steps to the GitHub Actions workflow before build.

---

## Future Vision

### Phase: Content Platform
Transform the site from a static brochure into a content platform with blog, resources, and interactive tools. This aligns with devEco's content pipeline and the Community Operating System.

### Phase: Interactive Tools
Embed interactive tools directly in the site:
- DevXRL self-assessment calculator
- Community health scorecard
- Developer journey mapping tool

### Phase: Integration Layer
Connect the site with devEco's broader platform:
- portalNetwork live status and stats
- Discord community activity feed
- Event calendar with upcoming streams and workshops
- Newsletter signup with content preferences

---

## Completed Items

Items move here when finished, with the date and any relevant notes.

- 2026-06-25: Deleted outdated DEPLOYMENT.md (DigitalOcean references). Deployment is documented in CLAUDE.md and README.md.

---

*Last updated: 2026-06-25*
