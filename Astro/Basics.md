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
  - [Layouts](#layouts)
  - [Markdown \& MDX](#markdown--mdx)
  - [Routing](#routing)
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
- [[Astro components]] have two main parts. A *Component Script* and the *Component Template*

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

> Dynamic values used in Astro components are not *reactive*. Astro components are templates that only run once - at build time!

- Any valid JS expression can be inserted into the Component Template via curly braces syntax

```js
---
const name = "Astro";
---
<div>
  <h1>Hello {name}!</h1>  <!-- Outputs <h1>Hello Astro!</h1> -->
</div>
```

>**Caution**: Interpolated HTML attributes in Astro components will be converted to strings (i.e. cannot pass functions or objects to HTML element attributes)

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

> *NOTE:* Fragments must be used when using JS expressions to dynamically generate multiple html elements

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

- *The `<slot/>` element is placeholder for external HTML content, allowing child elements from other files to be injected into a layout component template*

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

- Any explicit children passed to a slot within its Layout component will be used as fallback render  if nothing is passed to a slot

### CSS Styles

- `<style>` tags written in `.astro` components are locally scoped and do not impact children components!

### Client-Side Scripts

- Can be used to add event listeners, send analytics data, play animations, etc.

## Pages

- [[Pages]] are files that live in the `src/pages/` directory. The are responsible for handling routing, data loading, and overall page layout
- Custom 404 Error Page: `404.astro` or `404.md` within `/src/pages/`

## Layouts

## Markdown & MDX

## Routing

## Imports

## Endpoints

## Data Fetching

## Deploy

## Troubleshooting
