# Personal Homepage Design Spec

**Date:** 2026-07-07
**Status:** Approved

## Overview

Personal website homepage built with Astro. Dark minimalist aesthetic with glassmorphism elements, featuring a flowing animated hero background, blog snippets, and demo showcases.

## Tech Stack

- **Astro** (static site generator)
- **CSS** (custom properties, no preprocessor)
- **Vanilla JS** (Canvas background animation, Intersection Observer scroll reveals)
- **Inter** font (Google Fonts)
- Zero runtime framework dependency

## Architecture

```
home/
├── src/
│   ├── pages/
│   │   ├── index.astro          # Homepage (Hero + BlogSnippets + DemoShowcase)
│   │   ├── blogs.astro          # Placeholder
│   │   ├── demos.astro          # Placeholder
│   │   └── photos.astro         # Placeholder
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── BlogSnippets.astro
│   │   ├── DemoShowcase.astro
│   │   └── Footer.astro
│   ├── content/
│   │   └── blog/                # Markdown blog posts (2-3 samples)
│   ├── styles/
│   │   └── global.css
│   └── layouts/
│       └── Layout.astro
├── public/
├── astro.config.mjs
└── package.json
```

## Component Specs

### Navbar
- Fixed top, z-index high
- Glass effect: `rgba(10,10,10,0.6)` + `backdrop-filter: blur(20px)` + `border: 0.5px solid rgba(255,255,255,0.08)`
- Logo left (text "Home" or name), 4 nav links right: Home, Blogs, Demos, Photos
- On scroll past Hero: bottom border becomes `0.5px` green glow line
- Mobile: hamburger menu (future)

### Hero
- Full viewport height (`100vh`)
- Canvas-driven background: 3-4 large-radius fluorescent green blobs (`#0aff0a` at low opacity) drifting slowly over black (`#0a0a0a`), with heavy CSS blur overlay
- Centered text: name (large), subtitle/tagline (smaller, muted)
- Downward scroll indicator arrow at bottom
- All content above background via z-index

### BlogSnippets
- Section heading "Recent Blogs" with green accent underline
- 3 cards in a row (responsive: stack on mobile)
- Each card: title, date, ~150-char excerpt, "Read more →" link
- Hover: card translates up 4px, border glows green
- Data sourced from Astro content collection (Markdown)

### DemoShowcase
- Section heading "Demos" with green accent underline
- Card grid similar to blog
- Each card: demo name, one-line description, tech tags, external link icon
- Hover: same lift + glow pattern
- Data from a static array in the component (or future CMS)

### Footer
- Minimal: centered social icon links (GitHub, etc.) + copyright line
- Subtle top border

## Visual Tokens

| Token | Value |
|-------|-------|
| Background | `#0a0a0a` |
| Accent | `#0aff0a` |
| Accent dark | `#0a3a0a` |
| Glass bg | `rgba(10,10,10,0.6)` |
| Glass blur | `backdrop-filter: blur(20px)` |
| Glass border | `rgba(255,255,255,0.08)` |
| Text primary | `#f0f0f0` |
| Text muted | `#888` |
| Font | Inter, system sans-serif fallback |
| Border radius | 12-16px |
| Card bg | `rgba(255,255,255,0.03)` |

## Animations

1. **Hero background**: Canvas — multiple green blobs drifting at different slow speeds, blurred via CSS `filter: blur(80px)` on the canvas element
2. **Scroll reveals**: Intersection Observer adds `.visible` class triggering `fade-in-up` (opacity 0→1, translateY 20px→0, 0.6s ease)
3. **Navbar**: Green accent bottom border appears when scrolled past Hero
4. **Cards**: `transform: translateY(-4px)` + green border glow on hover, 0.3s transition

## Pages (non-homepage)

blogs.astro, demos.astro, photos.astro — all show `<Layout>` with Navbar + Footer and a centered "Coming Soon" message. Functional nav links for routing.

## Out of Scope

- Actual blog/demo CMS integration
- Dark/light mode toggle
- i18n
- Photo gallery functionality
- Mobile hamburger menu (nav links stay visible, wrap on small screens)
