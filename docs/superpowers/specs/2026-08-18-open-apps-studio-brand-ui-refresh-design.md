# Open Apps Studio Brand and UI Refresh Design

**Date:** 2026-08-18

## Objective

Create a coherent, distinctive Open Apps Studio visual system across the studio homepage and every public product site without making the products feel interchangeable. The refresh must improve clarity, trust, responsiveness, and visual polish while preserving every existing URL, redirect, SEO contract, privacy claim, and functional call to action.

## Scope

The refresh covers the seven public experiences and their supporting content pages:

- `openappsstudio.com`
- `sync.openappsstudio.com`
- `better-browsing-history.openappsstudio.com`
- `coldstorage.openappsstudio.com`
- `periodic.openappsstudio.com`
- `lingo.openappsstudio.com`
- `mentalmath.openappsstudio.com`

The studio repository's product overview routes and SEO guide pages are part of the same system. Product application interfaces are out of scope unless they are directly part of the public marketing site.

## Chosen Direction

Use a shared studio system with product-specific accents.

Open Apps Studio will feel editorial, useful, and quietly technical rather than corporate or ornamental. A warm neutral canvas, dark ink, precise type scale, subtle grid texture, generous spacing, and small functional details form the shared foundation. Each product receives one recognizable accent color and its own lightweight visual motif derived from what the product does.

This direction is preferred over:

1. **One identical template everywhere:** maximizes consistency but erases product identity and makes the portfolio feel mass-produced.
2. **A light logo and color swap:** minimizes risk but does not materially improve hierarchy, navigation, mobile usability, or perceived quality.

## Brand System

### Studio identity

- Use the existing geometric Open Apps Studio mark as the primary logo.
- Pair the mark with a consistent text lockup and a concise descriptor: “Small tools. Thoughtfully open.”
- Present the studio as the maker on every product page through a consistent navigation link and footer signature.
- Avoid stock imagery. Product UI, diagrams, and typographic compositions should do the visual work.

### Shared visual language

- **Canvas:** warm off-white rather than pure white.
- **Ink:** near-black for high contrast and a crafted editorial tone.
- **Surfaces:** lightly tinted cards with crisp one-pixel borders and restrained shadows.
- **Typography:** a robust system-font stack so the sites remain fast and dependency-free; use tighter display tracking and comfortable reading widths.
- **Shape:** medium radii for cards and buttons, with occasional sharper utility elements.
- **Motion:** short, optional hover and entrance transitions that respect `prefers-reduced-motion`.
- **Texture:** a very subtle grid or dot field in hero areas, implemented in CSS and never allowed to reduce text contrast.

### Product accents

- Studio: cobalt blue
- Sync Contacts: clear blue
- Better Browser History: violet
- Cold Storage: ice cyan on charcoal
- Periodic Table: emerald
- Lingo Lessons: coral
- Mental Math: amber

Accent colors support wayfinding and product personality. They must not become the only indicator of state or meaning.

## Information Architecture

### Studio homepage

1. Compact studio navigation with logo, “Apps,” “Principles,” and GitHub access.
2. A confident hero that explains what the studio builds and why it is different.
3. A featured-app grid with stronger names, plain-language outcomes, platform labels, and clear destinations.
4. A short principles section covering privacy, focus, openness, and fair pricing.
5. A studio footer that provides canonical links and a consistent maker signature.

### Product pages

1. Shared studio-aware navigation with the product name and contextual links.
2. Product-specific hero focused on one outcome, one primary action, and one supporting action.
3. A lightweight visual demonstration or interface composition above the fold where source assets allow it.
4. Scannable benefit sections, using evidence and precise copy instead of generic marketing claims.
5. Clear privacy/open-source reassurance near conversion points.
6. Consistent support, privacy, source, and studio links in the footer.

### Guides and supporting pages

- Retain search intent and semantic heading structure.
- Apply the shared typography, navigation, reading-width, callout, and footer treatments.
- Keep content-first layouts and avoid decorative elements that interfere with scanning.

## Interaction and Responsive Behavior

- All primary controls must have visible hover, active, and keyboard-focus states.
- Mobile layouts collapse to a single column without horizontal scrolling.
- Navigation remains usable at 320 CSS pixels; richer sites may use an accessible menu toggle.
- Cards use the full target area where appropriate and maintain at least 44-pixel touch targets.
- Decorative motion is disabled under `prefers-reduced-motion`.
- Layouts should remain stable when system fonts differ or text is zoomed to 200 percent.

## Accessibility and Content Rules

- Meet WCAG AA contrast for text and interactive controls.
- Preserve logical heading order, landmarks, descriptive link text, image alternatives, and current structured metadata.
- Maintain visible focus treatment across light and dark surfaces.
- Do not claim capabilities that the underlying product does not provide.
- Keep established privacy language accurate: local/on-device behavior is described only where the implementation already supports it.

## Technical Approach

The sites are static and deploy from separate repositories and Vercel projects. Each repository will retain its current build/deploy shape. The shared system will be expressed through a small, equivalent set of CSS custom properties and component conventions rather than adding a framework or a cross-repository runtime dependency.

- Preserve canonical URLs, redirects, metadata, structured data, sitemaps, and robots files.
- Reuse existing product assets before creating new decorative assets.
- Keep pages functional without client-side JavaScript unless JavaScript already powers an existing interaction.
- Avoid external font and analytics dependencies.
- Make focused commits in the repository that owns each deployment.

## Verification

Before release:

- Run repository-specific test, lint, typecheck, and build commands where available.
- Serve each static site locally and inspect desktop and mobile layouts.
- Check every internal navigation link, primary call to action, image, canonical tag, and structured-data block.
- Verify no console errors, horizontal overflow, missing assets, or broken focus states.
- Check representative pages at 320, 768, and 1440 CSS pixels and with reduced motion enabled.
- Deploy each site to its existing Vercel project, then repeat smoke tests on all seven production domains.
- Push only after the owning repository's verification passes.

## Success Criteria

- Every public site is immediately recognizable as part of Open Apps Studio.
- Each product remains visually distinct and its primary outcome is understandable above the fold.
- Navigation, calls to action, accessibility, and mobile behavior improve without functional regressions.
- Existing SEO and migration contracts remain intact.
- Production deployments and corresponding Git branches are verified and clean.
