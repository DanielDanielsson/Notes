# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Quartz v4 is a static site generator for publishing digital gardens and notes as websites. It transforms Markdown content (typically from Obsidian) into a fully-featured static site with graph views, backlinks, full-text search, and more.

## Common Commands

```bash
# Development server with hot reload
npx quartz build --serve

# Build for production
npx quartz build

# Build docs (uses /docs folder)
npm run docs

# Type checking and formatting
npm run check    # tsc --noEmit && prettier --check
npm run format   # prettier --write

# Run tests
npm run test
```

## Architecture

### Build Pipeline

The build process flows through three plugin types in sequence:

1. **Transformers** - Map over content, parsing Markdown and adding metadata
   - Text → MDAST (remark-parse) → HAST (remark-rehype) → HTML
   - Location: `quartz/plugins/transformers/`

2. **Filters** - Determine which files to publish (e.g., RemoveDrafts)
   - Location: `quartz/plugins/filters/`

3. **Emitters** - Generate output files (HTML pages, RSS, sitemap, etc.)
   - Location: `quartz/plugins/emitters/`

### Key Entry Points

- `quartz/bootstrap-cli.mjs` - CLI entry, handles esbuild transpilation and dev server
- `quartz/build.ts` - Main build orchestration, file watching, incremental rebuilds
- `quartz.config.ts` - Site configuration and plugin registration
- `quartz.layout.ts` - Page layout component composition

### Components

Components are Preact-based but only for server-side rendering (no hooks/state). They live in `quartz/components/` and are re-exported from `quartz/components/index.ts`.

Component structure:
```tsx
export default ((opts?: Options) => {
  const MyComponent = (props: QuartzComponentProps) => {
    return <div>...</div>
  }
  MyComponent.css = `...`  // Global CSS
  MyComponent.afterDOMLoaded = `...`  // Client-side JS
  return MyComponent
}) satisfies QuartzComponentConstructor
```

Client-side scripts use `.inline.ts` files that get bundled separately for the browser.

### Content Processing

- Content lives in `content/` directory (Markdown files)
- Worker threads parse content in parallel for >128 files
- Path handling is complex - see `quartz/util/path.ts` for slug/path utilities
- The `"nav"` DOM event fires on page load and SPA navigation for component setup

### Static Resources

CSS is processed with SCSS and minified with Lightning CSS. Scripts are split into:
- `beforeDOMLoaded` → `public/prescript.js`
- `afterDOMLoaded` → `public/postscript.js`

## Creating Plugins

All plugins follow the pattern: `(opts?: Options) => PluginInstance`

Register new plugins in their respective `index.ts` files under `quartz/plugins/`.

## Creating Components

1. Create component in `quartz/components/YourComponent.tsx`
2. Export from `quartz/components/index.ts`
3. Use in `quartz.layout.ts` via `Component.YourComponent()`
