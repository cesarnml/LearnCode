# Astro Doc - Basics

- [Astro Doc - Basics](#astro-doc---basics)
  - [Project Structure](#project-structure)
    - [`src/`](#src)
      - [`src/components`](#srccomponents)
      - [`src/layouts`](#srclayouts)
      - [`src/pages`](#srcpages)
      - [`src/styles`](#srcstyles)
      - [`public/`](#public)
  - [Astro Components](#astro-components)
    - [The Component Script](#the-component-script)
    - [The Component Template](#the-component-template)
    - [Dynamic Tags](#dynamic-tags)
    - [Fragments](#fragments)
    - [Differences between Astro and JSX](#differences-between-astro-and-jsx)
    - [Component Props](#component-props)
    - [Slots](#slots)
      - [Named Slots](#named-slots)
    - [CSS Styles](#css-styles)
    - [Client-Side Scripts](#client-side-scripts)
  - [Pages](#pages)
    - [Custom 404 Error Page](#custom-404-error-page)
  - [Layouts](#layouts)
    - [MD/MDX `layout` frontmatter property](#mdmdx-layout-frontmatter-property)
    - [Markdown/MDX layout components `Astro.props`](#markdownmdx-layout-components-astroprops)
    - [Manual importing of Layout components](#manual-importing-of-layout-components)
  - [Markdown And MDX](#markdown-and-mdx)
    - [Draft Frontmatter Property](#draft-frontmatter-property)
    - [MD/MDX Heading anchors](#mdmdx-heading-anchors)
    - [MDX Features](#mdx-features)
    - [Importing MD/MDX files into Astro components](#importing-mdmdx-files-into-astro-components)
    - [MD/MDX Content Component](#mdmdx-content-component)
    - [Dynamic Page Routing on MD/MDX files](#dynamic-page-routing-on-mdmdx-files)
    - [Syntax Highlighting](#syntax-highlighting)
  - [Routing](#routing)
    - [Dynamic Routes](#dynamic-routes)
      - [Static (SSG) Mode](#static-ssg-mode)
      - [Rest parameter in Dynamic Routes](#rest-parameter-in-dynamic-routes)
    - [Server (SSR) Mode](#server-ssr-mode)
    - [Route Priority Order](#route-priority-order)
    - [Pagination](#pagination)
      - [Page Interface](#page-interface)
        - [Nested Pagination Example](#nested-pagination-example)
    - [Excluding pages](#excluding-pages)
  - [Imports](#imports)
  - [Endpoints](#endpoints)
  - [Data Fetching](#data-fetching)
  - [Deploy](#deploy)
  - [Troubleshooting](#troubleshooting)

## Project Structure

- `public/*` static assets
- `src/*` code
- `astro.config.mjs` astro config file

### `src/`

- Includes:
  - Pages
  - Layouts
  - Components
  - UI-framework Components (React, etc.)
  - Styles
  - Markdown
- Astro processes, optimizes, and bundles `src/` files

#### `src/components`

- By convention `.astro` and external ui-framework components live here

#### `src/layouts`

- By convention, these components define the layout of a page and render child components via `<slot/>` component

#### `src/pages`

- A required directory which contains special - `page` components which generate site pages via a file-based routing paradigm

#### `src/styles`

- By convention, this directory stores CSS/SCSS files

#### `public/`

- Static files that do not require processing should live here (images, fonts, robots.txt manifest.webmanifest)

## Astro Components

- [[Astro components]] are HTML-only templating components with no client-side runtime.
- [[Astro components]] syntax is a superset of HTML. Adds support for including components and JavaScript expressions
- [[Astro components]] _render to HTML during your build._ Zero JavaScript footprint added by default
- [[Astro components]] have two main parts. A _Component Script_ and the _Component Template_

```js
---
// Component Script (JavaScript)
---
<!-- Component Template (HTML + JS Expressions) -->
```

### The Component Script

- Delineated by code fence (`---`)
- The [[Component Script]] contains all the JavaScript logic and data manipulation that the Component Template will consume on build to generate the HTML markup for the page

### The Component Template

- [[Component Template]] decides the HTML output of a component
- Supports:
  - JavaScript expressions
  - Imported components
  - Special [[Astro directives]]
  - Use of any values defined in the [[Component Script]]

> Dynamic values used in Astro components are not _reactive_. Astro components are templates that only run once - at build time!

- Any valid JS expression can be inserted into the Component Template via curly braces syntax

```js
---
const name = "Astro";
---
<div>
  <h1>Hello {name}!</h1>  <!-- Outputs <h1>Hello Astro!</h1> -->
</div>
```

> _WARNING_: Interpolated HTML attributes in Astro components will be converted to strings (i.e. cannot pass functions or objects to HTML element attributes)

- For example, the example below won't work!

```js
---
function handleClick () {
    console.log("button clicked!");
}
---
<!-- ❌ This doesn't work! ❌ -->
<button onClick={handleClick}>Nothing will happen when you click me!</button>
```

### Dynamic Tags

- Dynamic HTML Tags must be capitalized in order to interpolated correctly.
- Dynamic HTML Tags pattern does not support hydration directives (e.g. `client:load`)

### Fragments

- `<Fragment></Fragment>` or `<></>` are supported and come in handy when adding `set:html` directives

```js
---
const htmlString = '<p>Raw HTML content</p>';
---
<Fragment set:html={htmlString} />
```

> _NOTE:_ Fragments must be used when using JS expressions to dynamically generate multiple html elements

### Differences between Astro and JSX

- `Attributes` use kebab-case format instead of camelCase format
- `Comments` use <!-- -->

### Component Props

- Astro component props live on `Astro.props` global
- `interface Props` automatically picked up in the frontmatter

```ts
---
interface Props {
  name: string;
  greeting?: string;
}

const { greeting = "Hello", name } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

### Slots

- _The `<slot/>` element is placeholder for external HTML content, allowing child elements from other files to be injected into a layout component template_

#### Named Slots

```js
// Wrapper
<div>
    <slot name="top"/>
    <h1>Heading</h1>
    <slot />
    <footer>Made with Love</footer>
    <slot name="bottom"/>
</div>

// Child to render in slot
<img slot="top" href="path/to/image"/>
<div>
    <p>Render in default slot</p>
</div>
<p slot="bottom">Buy me a coffee. Renders at bottom slot</p>
```

- Any explicit children passed to a slot within its Layout component will be used as fallback render if nothing is passed to a slot

### CSS Styles

- `<style>` tags written in `.astro` components are locally scoped and do not impact children components!

### Client-Side Scripts

- Can be used to add event listeners, send analytics data, play animations, etc.

## Pages

- [[Pages]] are files that live in the `src/pages/` directory. The are responsible for handling routing, data loading, and overall page layout

### Custom 404 Error Page

- `404.astro` or `404.md` within `/src/pages/`

## Layouts

- [[Layouts]] are [[Astro components]] used to provide a reusable UI structure
- A typical "layout" component provides
  1. a page shell (`<html>`, `<head>`, `<body>` tags)
  2. a `<slot />` to specify where individual page content should be injected
- By convention, layout components live in `src/layouts`

### MD/MDX `layout` frontmatter property

- Markdown and MDX pages can use layout component via `layout` frontmatter property

```js
---
layout: ../layouts/BaseLayout.astro
title: "Hello, World!"
author: "Matthew Phillips"
date: "09 Aug 2022"
---
All frontmatter properties are available as props to an Astro layout component.

The `layout` property is the only special one provided by Astro.

You can use it in both Markdown and MDX files located within `src/pages/`.
```

> _TIP:_ Wrap `layout` path in quotes to enable file path aliases

```js
layout: '@layouts/MarkdownPostLayout.astro'
```

- Use `MarkdownLayoutProps` or `MDXLayoutProps` TS utility types to type incoming frontmatter props

```ts
---
import type { MarkdownLayoutProps } from 'astro';

type Props = MarkdownLayoutProps<{
  // Define frontmatter props here
  title: string;
  author: string;
  date: string;
}>;

// Now, `frontmatter`, `url`, and other Markdown layout properties
// are accessible with type safety
const { frontmatter, url } = Astro.props;
---
<html>
  <head>
    <meta rel="canonical" href={new URL(url, Astro.site).pathname}>
    <title>{frontmatter.title}</title>
  </head>
  <body>
    <h1>{frontmatter.title} by {frontmatter.author}</h1>
    <slot />
    <p>Written on: {frontmatter.date}</p>
  </body>
</html>
```

### Markdown/MDX layout components `Astro.props`

- `file` - Absolute path of md/mdx file
- `url` - Url of page - if it's a page 🙂
- `frontmatter` - all frontmatter properties
- `headings` - all `h1` to `h6` headings
- `type Heading = { depth: number, slug: string, text: string }`
- (MD only) `rawContent()` and `compiledContext()` functions

> _NOTE:_ `export` statements in MDX are not available on `Astro.props` for Layout components

### Manual importing of Layout components

- Allows `Astro.props` to store functions to be used within the layout component

```js
---
layout: ../../layouts/BaseLayout.astro
title: 'My first MDX post'
publishDate: '21 September 2022'
---
import BaseLayout from '../../layouts/BaseLayout.astro';

function fancyJsHelper() {
  return "Try doing that with YAML!";
}

<BaseLayout title={frontmatter.title} fancyJsHelper={fancyJsHelper}>
  Welcome to my new Astro blog, using MDX!
</BaseLayout>
```

> _TIP:_ Conditional destructing is a thing:

```js
const { title } = Astro.props.frontmatter || Astro.props
```

## Markdown And MDX

### Draft Frontmatter Property

- MD/MDX pages can be marked as `unpublished` via the optional `draft` frontmatter property
  - `draft: true` will result in:
    - page exclude from the site build
    - page will be returned by `Astro.glob()` invocations

> _TIP:_ Filter out draft md/mdx pages using JavaScript

```js
const posts = await Astro.glob('../pages/post/*.md')
const nonDraftPosts = posts.filter((post) => !post.frontmatter.draft)
```

### MD/MDX Heading anchors

- MD/MDX Headings can be used to directly link to specific sections of a page

```js
---
title: My page of content
---
## Introduction

I can link internally to [my conclusion](#conclusion) on the same page when writing Markdown.

## Conclusion

I can use the URL `https://my-domain.com/page-1/#introduction` to navigate directly to my Introduction on the page.
```

### MDX Features

- MDX integration allows for JS variables, expressions, and components to be used in mdx file
- MDX files have access to any `export` named properties, frontmatter properties, and imported Astro/UI-framework components
- MDX syntax can be mapped to custom components
  - e.g. `# Heading => <MyCustomHeadingComponent/>`
  - Don't forget to use `<slot/>` within the custom component!

```js
import Blockquote from '../components/Blockquote.astro';
export const components = {blockquote: Blockquote}

> This quote will be a custom Blockquote
```

```js
---
const props = Astro.props;
---
<blockquote {...props} class="bg-blue-50 p-4">
  <span class="text-4xl text-blue-600 mb-2">“</span>
  <slot /> <!-- Be sure to add a `<slot/>` for child content! -->
</blockquote>
```

### Importing MD/MDX files into Astro components

- MD/MDX files can be individual imported into `.astro` components or bulk imported via `Astro.glob`

```js
---
import * as greatPost from '../pages/post/great-post.md';

const posts = await Astro.glob('../pages/post/*.md');
---
```

- How to optionally type imported frontmatter

```js
---
interface Frontmatter {
  title: string;
  description?: string;
}
const posts = await Astro.glob<Frontmatter>('../pages/post/*.md');
---
```

> _NOTE:_ Astro "layout" components don't receive named exports from mdx files, whereas "non-layout" `.astro` components do receive named exports

### MD/MDX Content Component

- Import `{ Content }` to render MD/MDX content in importing component

```js
---
import {Content as PromoBanner} from '../components/promoBanner.md';
---

<h2>Today's promo</h2>
<PromoBanner />
```

- Passing custom components to MD/MDX `<Content />` component

```js
---
import { Content, components } from '../content.mdx';
import Heading from '../Heading.astro';
---
<!-- Creates a custom <h1> for the # syntax, _and_ applies any custom components defined in `content.mdx` -->
<Content components={{...components, h1: Heading }} />
```

### Dynamic Page Routing on MD/MDX files

- Dynamic page routing of md/mdx files

```js
// src/pages/[slug].astro
---
export async function getStaticPaths() {
  const posts = await Astro.glob('../posts/**/*.md')

  return posts.map(post => ({
    params: {
      slug: post.frontmatter.slug
    },
    props: {
      post
    },
  }))
}

const { Content } = Astro.props.post
---
<article>
  <Content/>
</article>
```

> _NOTE:_ `remark` is used by Astro to parse md/mdx files using GitHub-flavored Markdown and Smartypants plugins by default

- [[GitHub Flavored Markdown]] - This package is a unified (remark) plugin to enable the extensions to markdown that GitHub adds: autolink literals (www.x.com), footnotes ([^1]), strikethrough (~~stuff~~), tables (| cell |…), and tasklists (\* [x]). You can use this plugin to add support for parsing and serializing them. These extensions by GitHub to CommonMark are called GFM (GitHub Flavored Markdown).

- Extending MD/MDX parsing via plugins

```js
import { defineConfig } from 'astro/config'
import remarkToc from 'remark-toc'
import { rehypeAccessibleEmojis } from 'rehype-accessible-emojis'

export default {
  markdown: {
    // Applied to .md and .mdx files
    remarkPlugins: [remarkToc],
    // Preserves remark-gfm and remark-smartypants
    extendDefaultPlugins: true,
  },
  integrations: [
    mdx({
      // Applied to .mdx files only
      rehypePlugins: [rehypeAccessibleEmojis],
    }),
  ],
}
```

### Syntax Highlighting

- Astro comes with built-in support for Shiki and Prism. Shiki is the default.
- Use built-in `<Code />` component to provide syntax highlighting to code fences.

> _NOTE:_ Importing json is supported by default but jsonc is not!

> _NOTE:_ Astro does NOT support fetching remote markdown!

## Routing

- Astro uses _file-based routing_

### Dynamic Routes

#### Static (SSG) Mode

- Dynamic routes in static mode must export a `getStaticPaths()` function that returns `{params, props}[]`
- Dynamic routes can contain multiple parameters

```js
---
export function getStaticPaths () {
 return [
    {params: {lang: 'en', version: 'v1'}},
    {params: {lang: 'fr', version: 'v2'}},
  ];
}

const { lang, version } = Astro.params;
---
...
```

#### Rest parameter in Dynamic Routes

```js
// src/pages/sequences/[...path].astro
---
export function getStaticPaths() {
  return [
    {params: {path: 'one/two/three'}},
    {params: {path: 'four'}},
    {params: {path: undefined }}
  ]
}

const { path } = Astro.params;
---
...
// matches `/sequences/one/two/three`, `/sequences/four`, and `/sequences`
```

### Server (SSR) Mode

- Do not use `getStaticPaths()` in SSR mode

### Route Priority Order

- Static routes with path parameters > named parameters > rest parameters
- Ties are sorted alphabetically

### Pagination

```js
---
export async function getStaticPaths({ paginate }) {
  const astronautPages = [{
    astronaut: 'Neil Armstrong',
  }, {
    astronaut: 'Buzz Aldrin',
  }, {
    astronaut: 'Sally Ride',
  }, {
    astronaut: 'John Glenn',
  }];
  // Generate pages from our array of astronauts, with 2 to a page
  return paginate(astronautPages, { pageSize: 2 });
}
// All paginated data is passed on the "page" prop
const { page } = Astro.props;
---

<!--Display the current page number. Astro.params.page can also be used!-->
<h1>Page {page.currentPage}</h1>
<ul>
  <!--List the array of astronaut info-->
  {page.data.map(({ astronaut }) => <li>{astronaut}</li>)}
</ul>
```

#### Page Interface

```ts
interface Page<T = any> {
  /** result */
  data: T[]
  /** metadata */
  /** the count of the first item on the page, starting from 0 */
  start: number
  /** the count of the last item on the page, starting from 0 */
  end: number
  /** total number of results */
  total: number
  /** the current page number, starting from 1 */
  currentPage: number
  /** number of items per page (default: 25) */
  size: number
  /** number of last page */
  lastPage: number
  url: {
    /** url of the current page */
    current: string
    /** url of the previous page (if there is one) */
    prev: string | undefined
    /** url of the next page (if there is one) */
    next: string | undefined
  }
}
```

##### Nested Pagination Example

```js
// src/pages/[tag]/[page].astro
---
export async function getStaticPaths({ paginate }) {
  const allTags = ['red', 'blue', 'green'];
  const allPosts = await Astro.glob('../../posts/*.md');
  // For every tag, return a paginate() result.
  // Make sure that you pass `{params: {tag}}` to `paginate()`
  // so that Astro knows which tag grouping the result is for.
  return allTags.map((tag) => {
    const filteredPosts = allPosts.filter((post) => post.frontmatter.tag === tag);
    return paginate(filteredPosts, {
      params: { tag },
      pageSize: 10
    });
  });
}
const { page } = Astro.props;
const params = Astro.params;
```

### Excluding pages

- prefix files/directories with `_page/_directory` to exclude them from being built

## Imports

## Endpoints

## Data Fetching

## Deploy

## Troubleshooting
