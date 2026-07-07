# Personal Homepage Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a personal website homepage with Astro — dark minimalist aesthetic, glassmorphism navbar, animated Canvas hero background, blog snippet cards, and demo showcase cards.

**Architecture:** Astro static site with zero runtime JS frameworks. Components are `.astro` files composing HTML, scoped CSS, and minimal vanilla JS for animations (Canvas, Intersection Observer). Content sourced from Markdown (blog) and static arrays (demos).

**Tech Stack:** Astro 5.x, vanilla CSS (custom properties), vanilla JS (Canvas API, Intersection Observer), Inter font from Google Fonts.

## Global Constraints

- Zero runtime framework dependencies (no React, Vue, Svelte)
- All styles via CSS custom properties defined in `global.css`
- No CSS preprocessor
- All pages use `Layout.astro` wrapper
- Design tokens per spec §Visual Tokens — exact values must match
- Annotations per spec §Animations — no additional animations beyond spec

---

### Task 1: Scaffold Astro project

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`
- Create: `src/pages/index.astro` (minimal placeholder)
- Create: `src/styles/global.css` (empty)
- Create: `src/layouts/Layout.astro` (minimal wrapper)

**Interfaces:**
- Produces: project structure ready for component development, `npm run dev` works

- [ ] **Step 1: Initialize npm project and install Astro**

```bash
cd D:/Web/home
npm init -y
npm install astro @astrojs/markdown
```

Expected: `package.json` created, `node_modules/` populated.

- [ ] **Step 2: Write `package.json` scripts**

Read the generated `package.json`, then replace it:

```json
{
  "name": "home",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  },
  "dependencies": {
    "@astrojs/markdown": "^0.6.0",
    "astro": "^5.0.0"
  }
}
```

- [ ] **Step 3: Write `astro.config.mjs`**

```js
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://example.com',
  markdown: {
    syntaxHighlight: 'prism',
  },
});
```

- [ ] **Step 4: Write `tsconfig.json`**

```json
{
  "compilerOptions": {
    "strict": true,
    "jsx": "preserve"
  },
  "extends": "astro/tsconfigs/strict"
}
```

- [ ] **Step 5: Write minimal `src/styles/global.css`**

```css
/* Placeholder — populated in Task 2 */
```

- [ ] **Step 6: Write minimal `src/layouts/Layout.astro`**

```astro
---
// Placeholder — populated in Task 3
const { title } = Astro.props;
---
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{title ?? 'Home'}</title>
    <link rel="stylesheet" href="/src/styles/global.css" />
  </head>
  <body>
    <slot />
  </body>
</html>
```

- [ ] **Step 7: Write minimal `src/pages/index.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
---
<Layout title="Home">
  <h1>Hello World</h1>
</Layout>
```

- [ ] **Step 8: Verify `npm run dev` starts successfully**

```bash
npm run dev
```

Expected: Astro dev server starts on `http://localhost:4321`. Kill it after confirming.

- [ ] **Step 9: Commit**

```bash
git add -A
git commit -m "feat: scaffold Astro project"
```

---

### Task 2: Global styles and CSS variables

**Files:**
- Create: `src/styles/global.css` (replace placeholder)

**Interfaces:**
- Produces: CSS custom properties available site-wide, base reset, utility classes `fade-in-up` and `visible`

- [ ] **Step 1: Write `src/styles/global.css`**

```css
/* === Inter Font === */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

/* === CSS Custom Properties === */
:root {
  --bg: #0a0a0a;
  --accent: #0aff0a;
  --accent-dark: #0a3a0a;
  --glass-bg: rgba(10, 10, 10, 0.6);
  --glass-blur: blur(20px);
  --glass-border: rgba(255, 255, 255, 0.08);
  --text-primary: #f0f0f0;
  --text-muted: #888;
  --card-bg: rgba(255, 255, 255, 0.03);
  --radius: 14px;
  --font: 'Inter', system-ui, -apple-system, sans-serif;
  --nav-height: 64px;
}

/* === Reset === */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  -webkit-font-smoothing: antialiased;
}

body {
  font-family: var(--font);
  background-color: var(--bg);
  color: var(--text-primary);
  line-height: 1.6;
  overflow-x: hidden;
}

a {
  color: inherit;
  text-decoration: none;
}

img {
  max-width: 100%;
  display: block;
}

/* === Section Layout === */
.section {
  padding: 100px 24px;
  max-width: 1100px;
  margin: 0 auto;
}

.section__heading {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 48px;
  position: relative;
  display: inline-block;
}

.section__heading::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 40%;
  height: 3px;
  background: var(--accent);
  border-radius: 2px;
}

/* === Card Base === */
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

@media (max-width: 768px) {
  .card-grid {
    grid-template-columns: 1fr;
  }
}

.card {
  background: var(--card-bg);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius);
  padding: 28px;
  transition: transform 0.3s ease, border-color 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
}

.card:hover {
  transform: translateY(-4px);
  border-color: var(--accent);
  box-shadow: 0 0 20px rgba(10, 255, 10, 0.08);
}

/* === Scroll Reveal === */
.reveal {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* === Coming Soon === */
.coming-soon {
  min-height: calc(100vh - var(--nav-height));
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 16px;
}

.coming-soon__icon {
  font-size: 3rem;
}

.coming-soon__text {
  font-size: 1.25rem;
  color: var(--text-muted);
}
```

- [ ] **Step 2: Verify dev server hot-reloads without errors**

```bash
npm run dev
```

Expected: dev server starts, styles are served. Kill after confirming.

- [ ] **Step 3: Commit**

```bash
git add src/styles/global.css
git commit -m "feat: add global CSS with design tokens and utilities"
```

---

### Task 3: Layout component

**Files:**
- Modify: `src/layouts/Layout.astro` (replace placeholder)

**Interfaces:**
- Consumes: `global.css` from Task 2
- Produces: `<Layout title="...">` wrapper with `<html>`, `<head>`, `<body>`, imports `global.css`, passes `<slot />`

- [ ] **Step 1: Replace `src/layouts/Layout.astro`**

```astro
---
export interface Props {
  title?: string;
  description?: string;
}

const { title = 'Home', description = 'Personal website' } = Astro.props;
---
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <title>{title}</title>
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  </head>
  <body>
    <slot />
  </body>
</html>
<style is:global>
  @import '../styles/global.css';
</style>
```

- [ ] **Step 2: Verify — update `src/pages/index.astro`** to use updated Layout

```astro
---
import Layout from '../layouts/Layout.astro';
---
<Layout title="Home" description="My personal homepage">
  <h1>Hello World</h1>
</Layout>
```

- [ ] **Step 3: Verify `npm run dev` serves correctly**

```bash
npm run dev
```

Expected: page renders with global styles applied.

- [ ] **Step 4: Commit**

```bash
git add src/layouts/Layout.astro src/pages/index.astro
git commit -m "feat: finalize Layout component with meta tags"
```

---

### Task 4: Navbar component

**Files:**
- Create: `src/components/Navbar.astro`

**Interfaces:**
- Consumes: CSS variables from `global.css`
- Produces: `<Navbar />` — fixed glassmorphism nav with logo + 4 links, scroll-aware accent border

- [ ] **Step 1: Write `src/components/Navbar.astro`**

```astro
---
const navLinks = [
  { href: '/', label: 'Home' },
  { href: '/blogs', label: 'Blogs' },
  { href: '/demos', label: 'Demos' },
  { href: '/photos', label: 'Photos' },
];

const currentPath = Astro.url?.pathname || '/';
---

<nav class="navbar" id="navbar">
  <div class="navbar__inner">
    <a href="/" class="navbar__logo">~</a>
    <ul class="navbar__links">
      {navLinks.map(link => (
        <li>
          <a
            href={link.href}
            class={`navbar__link ${currentPath === link.href ? 'navbar__link--active' : ''}`}
          >
            {link.label}
          </a>
        </li>
      ))}
    </ul>
  </div>
</nav>

<script>
  const navbar = document.getElementById('navbar');
  let ticking = false;

  function updateNavbar() {
    const scrolled = window.scrollY > window.innerHeight - 64;
    if (scrolled) {
      navbar?.classList.add('navbar--scrolled');
    } else {
      navbar?.classList.remove('navbar--scrolled');
    }
    ticking = false;
  }

  window.addEventListener('scroll', () => {
    if (!ticking) {
      requestAnimationFrame(updateNavbar);
      ticking = true;
    }
  });

  // Initial check
  updateNavbar();
</script>

<style>
  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    height: var(--nav-height);
    background: var(--glass-bg);
    backdrop-filter: var(--glass-blur);
    -webkit-backdrop-filter: var(--glass-blur);
    border-bottom: 0.5px solid var(--glass-border);
    transition: border-color 0.3s ease;
  }

  .navbar--scrolled {
    border-bottom-color: var(--accent);
  }

  .navbar__inner {
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 24px;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .navbar__logo {
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--accent);
    transition: opacity 0.2s ease;
  }

  .navbar__logo:hover {
    opacity: 0.7;
  }

  .navbar__links {
    list-style: none;
    display: flex;
    gap: 32px;
  }

  .navbar__link {
    font-size: 0.95rem;
    font-weight: 500;
    color: var(--text-muted);
    transition: color 0.2s ease;
    padding: 4px 0;
    position: relative;
  }

  .navbar__link::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 0;
    width: 0;
    height: 1.5px;
    background: var(--accent);
    transition: width 0.2s ease;
  }

  .navbar__link:hover,
  .navbar__link--active {
    color: var(--text-primary);
  }

  .navbar__link:hover::after,
  .navbar__link--active::after {
    width: 100%;
  }

  @media (max-width: 640px) {
    .navbar__links {
      gap: 20px;
    }

    .navbar__link {
      font-size: 0.85rem;
    }
  }
</style>
```

- [ ] **Step 2: Verify — integrate into `src/pages/index.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
---
<Layout title="Home" description="My personal homepage">
  <Navbar />
  <div style="height: 200vh; padding-top: 64px;">
    <p style="color: #f0f0f0; padding: 40px;">Scroll to test navbar accent border</p>
  </div>
</Layout>
```

- [ ] **Step 3: Test scroll behavior in browser**

```bash
npm run dev
```

Expected: Navbar fixed at top. Glass effect visible. When scrolling past viewport height, bottom border turns green. Kill after confirming.

- [ ] **Step 4: Commit**

```bash
git add src/components/Navbar.astro src/pages/index.astro
git commit -m "feat: add glassmorphism Navbar with scroll-aware accent border"
```

---

### Task 5: Hero component with Canvas blob background

**Files:**
- Create: `src/components/Hero.astro`

**Interfaces:**
- Consumes: CSS variables from `global.css`
- Produces: `<Hero />` — full-viewport section with animated Canvas blob background, centered name/subtitle, scroll-down indicator

- [ ] **Step 1: Write `src/components/Hero.astro`**

```astro
---
// User-configurable content
const name = 'Your Name';
const tagline = 'Building things for the web.';
---

<section class="hero">
  <canvas class="hero__canvas" id="hero-canvas"></canvas>
  <div class="hero__content">
    <h1 class="hero__name">{name}</h1>
    <p class="hero__tagline">{tagline}</p>
  </div>
  <div class="hero__scroll-hint">
    <svg width="24" height="40" viewBox="0 0 24 40" fill="none" stroke="var(--text-muted)" stroke-width="1.5">
      <rect x="1" y="1" width="22" height="38" rx="11" />
      <circle cx="12" cy="14" r="3" fill="var(--text-muted)">
        <animate attributeName="cy" values="14;22;14" dur="2s" repeatCount="indefinite" />
      </circle>
    </svg>
  </div>
</section>

<script>
  class Blob {
    x: number;
    y: number;
    vx: number;
    vy: number;
    radius: number;
    color: string;

    constructor(w: number, h: number) {
      this.x = Math.random() * w;
      this.y = Math.random() * h;
      this.vx = (Math.random() - 0.5) * 0.3;
      this.vy = (Math.random() - 0.5) * 0.3;
      this.radius = 200 + Math.random() * 300;
      this.color = `rgba(10, 255, 10, ${0.03 + Math.random() * 0.04})`;
    }

    update(w: number, h: number) {
      this.x += this.vx;
      this.y += this.vy;

      if (this.x < -this.radius) this.x = w + this.radius;
      if (this.x > w + this.radius) this.x = -this.radius;
      if (this.y < -this.radius) this.y = h + this.radius;
      if (this.y > h + this.radius) this.y = -this.radius;
    }

    draw(ctx: CanvasRenderingContext2D) {
      ctx.beginPath();
      const gradient = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, this.radius);
      gradient.addColorStop(0, this.color);
      gradient.addColorStop(1, 'transparent');
      ctx.fillStyle = gradient;
      ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
      ctx.fill();
    }
  }

  const canvas = document.getElementById('hero-canvas') as HTMLCanvasElement | null;
  if (canvas) {
    const ctx = canvas.getContext('2d');
    if (ctx) {
      let w = window.innerWidth;
      let h = window.innerHeight;
      canvas.width = w;
      canvas.height = h;

      const blobs = Array.from({ length: 4 }, () => new Blob(w, h));

      function animate() {
        if (!ctx || !canvas) return;
        ctx.clearRect(0, 0, w, h);

        for (const blob of blobs) {
          blob.update(w, h);
          blob.draw(ctx);
        }

        requestAnimationFrame(animate);
      }

      animate();

      window.addEventListener('resize', () => {
        w = window.innerWidth;
        h = window.innerHeight;
        canvas.width = w;
        canvas.height = h;
      });
    }
  }
</script>

<style>
  .hero {
    position: relative;
    height: 100vh;
    width: 100%;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .hero__canvas {
    position: absolute;
    inset: 0;
    filter: blur(80px);
    z-index: 0;
  }

  .hero__content {
    position: relative;
    z-index: 1;
    text-align: center;
  }

  .hero__name {
    font-size: clamp(2.5rem, 6vw, 5rem);
    font-weight: 700;
    letter-spacing: -0.03em;
    color: var(--text-primary);
  }

  .hero__tagline {
    margin-top: 16px;
    font-size: clamp(1rem, 2vw, 1.25rem);
    color: var(--text-muted);
    font-weight: 400;
  }

  .hero__scroll-hint {
    position: absolute;
    bottom: 32px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 1;
    opacity: 0.6;
  }
</style>
```

- [ ] **Step 2: Integrate Hero into `src/pages/index.astro`**

Replace index.astro:

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Hero from '../components/Hero.astro';
---
<Layout title="Home" description="My personal homepage">
  <Navbar />
  <main>
    <Hero />
  </main>
</Layout>
```

- [ ] **Step 3: Verify Canvas animation renders correctly**

```bash
npm run dev
```

Expected: Hero section fills viewport. Green blobs drift slowly. Blur effect visible. Centered name and tagline. Animated scroll indicator at bottom. Kill after confirming.

- [ ] **Step 4: Commit**

```bash
git add src/components/Hero.astro src/pages/index.astro
git commit -m "feat: add Hero with Canvas blob background animation"
```

---

### Task 6: BlogSnippets component with content collection

**Files:**
- Create: `src/content/blog/post-1.md`
- Create: `src/content/blog/post-2.md`
- Create: `src/content/blog/post-3.md`
- Create: `src/content/config.ts`
- Create: `src/components/BlogSnippets.astro`

**Interfaces:**
- Consumes: Astro content collections API, CSS card classes from `global.css`
- Produces: `<BlogSnippets />` — section with 3 blog cards sourced from Markdown

- [ ] **Step 1: Write `src/content/config.ts`**

```ts
import { defineCollection, z } from 'astro:content';

const blogCollection = defineCollection({
  schema: z.object({
    title: z.string(),
    date: z.date(),
    excerpt: z.string().max(160),
    tags: z.array(z.string()).optional().default([]),
  }),
});

export const collections = {
  blog: blogCollection,
};
```

- [ ] **Step 2: Write three sample blog posts**

`src/content/blog/post-1.md`:

```md
---
title: 'Getting Started with TypeScript'
date: 2026-06-15
excerpt: 'TypeScript adds static typing to JavaScript, catching errors early and improving developer experience. A gentle introduction to the type system and tooling.'
tags: ['typescript', 'javascript']
---

## Why TypeScript?

TypeScript has become the de facto standard for building large-scale JavaScript applications...

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore.
```

`src/content/blog/post-2.md`:

```md
---
title: 'CSS Grid Tips and Tricks'
date: 2026-05-20
excerpt: 'CSS Grid is a powerful layout system. Here are five practical patterns I use daily, from auto-fill cards to overlapping hero layouts.'
tags: ['css', 'frontend']
---

## Grid Patterns

CSS Grid makes complex layouts straightforward with just a few properties...

Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
```

`src/content/blog/post-3.md`:

```md
---
title: 'Building a CLI Tool in Rust'
date: 2026-04-10
excerpt: 'Rust is a great choice for CLI tools — fast startup, small binaries, and excellent error handling. Walk through building a real CLI with clap.'
tags: ['rust', 'cli']
---

## Why Rust for CLI?

Rust combines performance with safety, making it ideal for command-line tools...

Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

- [ ] **Step 3: Write `src/components/BlogSnippets.astro`**

```astro
---
import { getCollection } from 'astro:content';

const posts = await getCollection('blog');
const recent = posts
  .sort((a, b) => b.data.date.getTime() - a.data.date.getTime())
  .slice(0, 3);
---

<section class="section" id="blogs">
  <h2 class="section__heading">Recent Blogs</h2>
  <div class="card-grid">
    {recent.map(post => (
      <article class="card">
        <a href={`/blogs/${post.slug}`}>
          <time class="blog-card__date" datetime={post.data.date.toISOString()}>
            {post.data.date.toLocaleDateString('en-US', { year: 'numeric', month: 'short', day: 'numeric' })}
          </time>
          <h3 class="blog-card__title">{post.data.title}</h3>
          <p class="blog-card__excerpt">{post.data.excerpt}</p>
          <span class="blog-card__link">Read more &rarr;</span>
        </a>
      </article>
    ))}
  </div>
</section>

<style>
  .blog-card__date {
    font-size: 0.8rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .blog-card__title {
    margin-top: 8px;
    font-size: 1.15rem;
    font-weight: 600;
    color: var(--text-primary);
  }

  .blog-card__excerpt {
    margin-top: 12px;
    font-size: 0.92rem;
    color: var(--text-muted);
    line-height: 1.6;
  }

  .blog-card__link {
    display: inline-block;
    margin-top: 16px;
    font-size: 0.9rem;
    font-weight: 500;
    color: var(--accent);
    transition: opacity 0.2s ease;
  }

  .blog-card__link:hover {
    opacity: 0.7;
  }
</style>
```

- [ ] **Step 4: Integrate into `src/pages/index.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Hero from '../components/Hero.astro';
import BlogSnippets from '../components/BlogSnippets.astro';
---
<Layout title="Home" description="My personal homepage">
  <Navbar />
  <main>
    <Hero />
    <BlogSnippets />
  </main>
</Layout>
```

- [ ] **Step 5: Verify blog cards render**

```bash
npm run dev
```

Expected: Scroll down from Hero, "Recent Blogs" section with 3 cards. Each shows date, title, excerpt, "Read more". Hover lifts card and glows border green. Kill after confirming.

- [ ] **Step 6: Commit**

```bash
git add src/content/ src/components/BlogSnippets.astro src/pages/index.astro
git commit -m "feat: add BlogSnippets component with content collection"
```

---

### Task 7: DemoShowcase component

**Files:**
- Create: `src/components/DemoShowcase.astro`

**Interfaces:**
- Consumes: CSS card classes from `global.css`
- Produces: `<DemoShowcase />` — section with 3 demo project cards from static data

- [ ] **Step 1: Write `src/components/DemoShowcase.astro`**

```astro
---
const demos = [
  {
    name: 'Pixel Weather',
    description: 'A minimal weather app with pixel-art animations and location-based forecasts.',
    tags: ['React', 'OpenWeather API'],
    url: 'https://github.com',
  },
  {
    name: 'Markdown Notes',
    description: 'Quick note-taking app with Markdown preview, tags, and full-text search.',
    tags: ['Vue', 'IndexedDB'],
    url: 'https://github.com',
  },
  {
    name: 'Color Palette Generator',
    description: 'Generate harmonious color palettes from an image or a base color. Export as CSS variables or Tailwind config.',
    tags: ['Vanilla JS', 'Canvas'],
    url: 'https://github.com',
  },
];
---

<section class="section" id="demos">
  <h2 class="section__heading">Demos</h2>
  <div class="card-grid">
    {demos.map(demo => (
      <article class="card">
        <a href={demo.url} target="_blank" rel="noopener noreferrer">
          <h3 class="demo-card__name">
            {demo.name}
            <svg class="demo-card__external" width="14" height="14" viewBox="0 0 14 14" fill="none" stroke="currentColor" stroke-width="1.5">
              <path d="M4 2H2v10h10V9 M11 3L7 7 M11 3H8.5 M11 3v2.5" />
            </svg>
          </h3>
          <p class="demo-card__desc">{demo.description}</p>
          <div class="demo-card__tags">
            {demo.tags.map(tag => <span class="demo-card__tag">{tag}</span>)}
          </div>
        </a>
      </article>
    ))}
  </div>
</section>

<style>
  .demo-card__name {
    font-size: 1.15rem;
    font-weight: 600;
    color: var(--text-primary);
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .demo-card__external {
    opacity: 0.4;
    transition: opacity 0.2s ease, transform 0.2s ease;
    flex-shrink: 0;
  }

  .card:hover .demo-card__external {
    opacity: 1;
    transform: translate(2px, -2px);
  }

  .demo-card__desc {
    margin-top: 12px;
    font-size: 0.92rem;
    color: var(--text-muted);
    line-height: 1.6;
  }

  .demo-card__tags {
    margin-top: 16px;
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .demo-card__tag {
    font-size: 0.75rem;
    font-weight: 500;
    padding: 4px 10px;
    border-radius: 20px;
    background: var(--accent-dark);
    color: var(--accent);
  }
</style>
```

- [ ] **Step 2: Integrate into `src/pages/index.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Hero from '../components/Hero.astro';
import BlogSnippets from '../components/BlogSnippets.astro';
import DemoShowcase from '../components/DemoShowcase.astro';
---
<Layout title="Home" description="My personal homepage">
  <Navbar />
  <main>
    <Hero />
    <BlogSnippets />
    <DemoShowcase />
  </main>
</Layout>
```

- [ ] **Step 3: Verify demo cards render**

```bash
npm run dev
```

Expected: "Demos" section below blogs. 3 cards with name (external link icon), description, tech tags (green pill badges). Hover lift + glow. Kill after confirming.

- [ ] **Step 4: Commit**

```bash
git add src/components/DemoShowcase.astro src/pages/index.astro
git commit -m "feat: add DemoShowcase component with project cards"
```

---

### Task 8: Footer component

**Files:**
- Create: `src/components/Footer.astro`

**Interfaces:**
- Consumes: CSS variables from `global.css`
- Produces: `<Footer />` — minimal footer with social links + copyright

- [ ] **Step 1: Write `src/components/Footer.astro`**

```astro
---
const socials = [
  { label: 'GitHub', href: 'https://github.com', icon: 'GH' },
  { label: 'Twitter', href: 'https://twitter.com', icon: 'TW' },
];
---

<footer class="footer">
  <div class="footer__inner">
    <div class="footer__socials">
      {socials.map(s => (
        <a href={s.href} target="_blank" rel="noopener noreferrer" class="footer__social-link" aria-label={s.label}>
          {s.icon}
        </a>
      ))}
    </div>
    <p class="footer__copy">&copy; {new Date().getFullYear()} All rights reserved.</p>
  </div>
</footer>

<style>
  .footer {
    border-top: 0.5px solid var(--glass-border);
    padding: 40px 24px;
  }

  .footer__inner {
    max-width: 1100px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;
  }

  .footer__socials {
    display: flex;
    gap: 24px;
  }

  .footer__social-link {
    font-size: 0.85rem;
    font-weight: 600;
    letter-spacing: 0.05em;
    color: var(--text-muted);
    transition: color 0.2s ease;
  }

  .footer__social-link:hover {
    color: var(--accent);
  }

  .footer__copy {
    font-size: 0.8rem;
    color: var(--text-muted);
  }
</style>
```

- [ ] **Step 2: Add Footer to `src/pages/index.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Hero from '../components/Hero.astro';
import BlogSnippets from '../components/BlogSnippets.astro';
import DemoShowcase from '../components/DemoShowcase.astro';
import Footer from '../components/Footer.astro';
---
<Layout title="Home" description="My personal homepage">
  <Navbar />
  <main>
    <Hero />
    <BlogSnippets />
    <DemoShowcase />
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 3: Verify footer renders**

```bash
npm run dev
```

Expected: Footer at bottom with social links and copyright. Subtle top border. Kill after confirming.

- [ ] **Step 4: Commit**

```bash
git add src/components/Footer.astro src/pages/index.astro
git commit -m "feat: add Footer component with social links"
```

---

### Task 9: Scroll reveal animation

**Files:**
- Modify: `src/layouts/Layout.astro` (add Intersection Observer script)

**Interfaces:**
- Consumes: `.reveal` CSS class from `global.css`
- Produces: all `.reveal` elements get `.visible` class on scroll into view

- [ ] **Step 1: Add Intersection Observer to Layout.astro**

Update `src/layouts/Layout.astro` — add `<script>` before closing `</body>`:

```astro
---
export interface Props {
  title?: string;
  description?: string;
}

const { title = 'Home', description = 'Personal website' } = Astro.props;
---
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <title>{title}</title>
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  </head>
  <body>
    <slot />
    <script>
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              entry.target.classList.add('visible');
            }
          });
        },
        { threshold: 0.15, rootMargin: '0px 0px -40px 0px' }
      );

      document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
    </script>
  </body>
</html>
<style is:global>
  @import '../styles/global.css';
</style>
```

- [ ] **Step 2: Add `.reveal` classes to section headings and cards**

Update `src/components/BlogSnippets.astro` — wrap cards:

Change the section and card rendering to include `reveal` classes:

```astro
<section class="section reveal" id="blogs">
  <h2 class="section__heading">Recent Blogs</h2>
  <div class="card-grid">
    {recent.map((post, i) => (
      <article class="card reveal" style={`transition-delay: ${i * 0.1}s`}>
        ...
      </article>
    ))}
  </div>
</section>
```

Update `src/components/DemoShowcase.astro` similarly:

```astro
<section class="section reveal" id="demos">
  <h2 class="section__heading">Demos</h2>
  <div class="card-grid">
    {demos.map((demo, i) => (
      <article class="card reveal" style={`transition-delay: ${i * 0.1}s`}>
        ...
      </article>
    ))}
  </div>
</section>
```

- [ ] **Step 3: Verify scroll reveals work**

```bash
npm run dev
```

Expected: As you scroll down, section headings and cards fade in from below with staggered delays. Kill after confirming.

- [ ] **Step 4: Commit**

```bash
git add src/layouts/Layout.astro src/components/BlogSnippets.astro src/components/DemoShowcase.astro
git commit -m "feat: add Intersection Observer scroll reveal animations"
```

---

### Task 10: Placeholder pages (blogs, demos, photos)

**Files:**
- Create: `src/pages/blogs.astro`
- Create: `src/pages/demos.astro`
- Create: `src/pages/photos.astro`

**Interfaces:**
- Consumes: `Layout`, `Navbar`, `Footer`
- Produces: 3 placeholder pages with "Coming Soon" message, routed via Navbar links

- [ ] **Step 1: Write `src/pages/blogs.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Footer from '../components/Footer.astro';
---
<Layout title="Blogs" description="Blog posts">
  <Navbar />
  <main class="coming-soon">
    <div class="coming-soon__icon">&#9998;</div>
    <h1 class="coming-soon__text">Blogs &mdash; Coming Soon</h1>
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 2: Write `src/pages/demos.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Footer from '../components/Footer.astro';
---
<Layout title="Demos" description="Project demos">
  <Navbar />
  <main class="coming-soon">
    <div class="coming-soon__icon">&#9881;</div>
    <h1 class="coming-soon__text">Demos &mdash; Coming Soon</h1>
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 3: Write `src/pages/photos.astro`**

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Footer from '../components/Footer.astro';
---
<Layout title="Photos" description="Photo gallery">
  <Navbar />
  <main class="coming-soon">
    <div class="coming-soon__icon">&#128247;</div>
    <h1 class="coming-soon__text">Photos &mdash; Coming Soon</h1>
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 4: Verify navigation works**

```bash
npm run dev
```

Expected: Clicking each nav link navigates to the correct placeholder page. "Coming Soon" message centered. Navbar + Footer present.

- [ ] **Step 5: Commit**

```bash
git add src/pages/blogs.astro src/pages/demos.astro src/pages/photos.astro
git commit -m "feat: add placeholder pages for blogs, demos, photos"
```

---

### Task 11: Final integration and polish

**Files:**
- Modify: `src/pages/index.astro` (ensure all components integrated)
- Create: `public/favicon.svg`

**Interfaces:**
- Produces: complete, navigation-ready homepage

- [ ] **Step 1: Verify final `src/pages/index.astro`**

Read the current `src/pages/index.astro`. It should already contain all imports — confirm it matches:

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Hero from '../components/Hero.astro';
import BlogSnippets from '../components/BlogSnippets.astro';
import DemoShowcase from '../components/DemoShowcase.astro';
import Footer from '../components/Footer.astro';
---
<Layout title="Home" description="My personal homepage">
  <Navbar />
  <main>
    <Hero />
    <BlogSnippets />
    <DemoShowcase />
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 2: Create `public/favicon.svg`**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="6" fill="#0a0a0a" />
  <rect x="1" y="1" width="30" height="30" rx="5" fill="none" stroke="#0aff0a" stroke-width="1" />
  <text x="16" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="18" font-weight="700" fill="#0aff0a">~</text>
</svg>
```

- [ ] **Step 3: Run a full build**

```bash
npm run build
```

Expected: Build completes without errors. Output in `dist/`.

- [ ] **Step 4: Run `npm run dev`** and do final visual check

```bash
npm run dev
```

Checklist:
- Hero: 4 green blobs drifting, blurred, full-screen. Name + tagline centered. Scroll indicator pulsing.
- Navbar: Fixed, glass effect. Scroll past Hero → bottom border turns green.
- Blogs: 3 cards with real content. Hover lift + green glow.
- Demos: 3 cards with tags. External link arrow animates on hover.
- Footer: Centered social links + copyright.
- Scroll reveal: sections fade in on scroll.
- Navigation: All 4 nav links work (Home, Blogs → Coming Soon, Demos → Coming Soon, Photos → Coming Soon).

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "feat: final integration polish — favicon, build verification"
```
