# Astro Core Concepts

- [Astro Core Concepts](#astro-core-concepts)
  - [Why Astro](#why-astro)
    - [\[\[Content-focused\]\]](#content-focused)
    - [\[\[Server-first\]\]](#server-first)
    - [\[\[Fast by default\]\]](#fast-by-default)
    - [\[\[Easy to use\]\]](#easy-to-use)
    - [\[\[Fully-featured, but flexible\]\]](#fully-featured-but-flexible)
  - [MPA vs SPA](#mpa-vs-spa)
  - [Astro Islands](#astro-islands)

## Why Astro

- [[Astro]] is an `all-in-one web framework` for building `fast, content-focused websites`
- Five core design principles behind Astro:
  1. [[Content-focused]]
  2. [[Sever-first]]
  3. [[Fast by default]]
  4. [[Easy to use]]
  5. [[Fully-featured, but flexible]]

### [[Content-focused]]

- Good for content-rich websites:
  - marketing sites
  - publishing sites
  - documentation sites
  - blogs
  - portfolios
  - _some_ ecommerce sites
- Not a great fit for _web-apps_

### [[Server-first]]

- Leverages server-side rendering over client-side rendering as much as possible

### [[Fast by default]]

- _It should be nearly impossible to build a slow website with Astro_

### [[Easy to use]]

- Astro components are renderer only on the server. There is no reactivity!

### [[Fully-featured, but flexible]]

- _Astro is an all-in-one web framework that comes with everything you need to build a website_
- Includes:
  - component syntax (`.astro`)
  - file-based routing
  - asset handing
  - a build process
  - bundling
  - optimizations
  - data-fetching
- Awesome integrations (such as React, Tailwind CSS, MDX, image optimizations, and deployment to popular host providers)

## MPA vs SPA

- _Astro is a Multi-Page Application (MPA)_
- Multi-Page Application [[MPA]] is a website consisting of multiple HTML pages, mostly rendered on a server.
- Single-Page Application [[SPA]] is a website consisting of a single JavaScript application that loads in a user's browser and renders HTML locally.
- [[MPA]]s have better SEO and first page-load speeds, but suffer from clunky navigation across pages
  - Hotwire's [Turbo](https://turbo.hotwired.dev/) - can be used to mimic smooth pae transitions that is available on client-side routing [[SPA]]
- _Astro prioritizes the performance and simplicity of MPAs because it makes the most sense for our usecase of content-focused websites. More stateful, interaction-heavy websites may benefit more from the app-like architecture that SPAs bring at the expense of first-load performance_
- [Surma and Jake on SPAs vs MPAs](https://www.youtube.com/watch?v=ivLhf3hq7eM)

## Astro Islands

- [Preact creator Jason Miller on Island Architecture](https://jasonformat.com/islands-architecture/)
