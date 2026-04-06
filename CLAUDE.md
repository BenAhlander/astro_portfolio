# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- **Dev server**: `npm run dev` (runs at localhost:4321)
- **Build**: `npm run build` (runs `astro check` then `astro build` to `./dist/`)
- **Preview production build**: `npm run preview`
- **Type check only**: `npx astro check`

There are no tests configured in this project. Type checking via `astro check` is the primary validation.

## Architecture

This is a personal portfolio site built with **Astro 5** using static site generation. It ships zero client-side JavaScript by default — only three inline scripts handle theme detection, menu toggle, and a load event listener.

### Content System

Portfolio projects live as Markdown files in `src/content/work/`. The collection schema is defined in `src/content/config.ts` using Zod (fields: title, description, publishDate, tags, img, img_alt). Pages query these via Astro's `getCollection('work')` API.

The dynamic route `src/pages/work/[...slug].astro` generates individual project pages from `getStaticPaths()`.

### Styling

All design tokens (colors, typography, shadows, spacing) are CSS custom properties defined in `src/styles/global.css`. There is no Tailwind or CSS-in-JS — components use scoped `<style>` blocks with these tokens.

**Theme system**: Light by default, dark via `.theme-dark` class on `<html>`. An inline script in `MainHead.astro` reads localStorage/system preference before paint to avoid flash. All color variables are re-mapped in the `.theme-dark` selector.

**Responsive breakpoint**: Primary at `50em` (800px), secondary at `60em` (960px). Mobile-first approach.

### Component Hierarchy

```
BaseLayout
  ├── MainHead (metadata, global CSS, theme script)
  ├── Nav (navigation, menu-button custom element)
  ├── [Page Content via <slot />]
  └── Footer
```

Key components: `Hero` (page headers), `Grid` (gallery layout with `offset`/`small` variants), `PortfolioPreview` (project cards), `Icon` (SVG renderer using paths from `IconPaths.ts`), `ThemeToggle`.

### Pages

Four routes: `/` (index), `/work` (gallery), `/work/[slug]` (project detail), `/about`, plus a custom `/404`.
