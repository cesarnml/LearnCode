# Astro Blog Tutorial Notes

- [Astro Blog Tutorial Notes](#astro-blog-tutorial-notes)
  - [Part 0 Notes - Welcome, world!](#part-0-notes---welcome-world)
  - [Part 1 Notes - Create and Deploy Astro Site](#part-1-notes---create-and-deploy-astro-site)
  - [Part 2 Notes - Add styles and link to pages](#part-2-notes---add-styles-and-link-to-pages)
  - [Part 3 Notes - Build and Design Astro UI Components](#part-3-notes---build-and-design-astro-ui-components)
  - [Part 4 Notes - Layouts](#part-4-notes---layouts)
  - [Part 5 Notes - Astro API](#part-5-notes---astro-api)
  - [Part 6 Notes - Astro Islands](#part-6-notes---astro-islands)

## Part 0 Notes - Welcome, world!

- [Astro New](https://astro.new/) - Project starter templates

## Part 1 Notes - Create and Deploy Astro Site

- Requires `node` v14.18.0, or v16.12.0, or higher
- Astro project creation command:

```bash
# create a new project with pnpm
pnpm create astro@latest
```

- Launch project with:

```bash
pnpm run dev
```

- [Netlify](https://www.netlify.com/) - is ideal for Astro deployments because it supports both SSG and SSR

## Part 2 Notes - Add styles and link to pages

- Astro pages go inside `src/pages/` directory. [[pages]] directory uses file-base routing system
- To link to pages simply use the html anchor tag,

```html
<a href="/">Home</a>
```

- Astro can import markdown files (`.md`). The frontmatter is available to importing components on `Astro.props.frontmatter.[field]`
- Astro code fences (`---`) allow for valid JS/TS. Declared variables are available in the template block via curlies `{}`
- Astro supports `<style>` blocks and with the `define:vars={{}}` attribute directive variables declared in the code fence block can be used via CSS variable syntax (e.g. `var(--variableName)`). Styles declared within the `<style>` tag are local by default
- Stylesheets (global) can also be imported in the code fenced section of `.astro` components/pages

## Part 3 Notes - Build and Design Astro UI Components

- A standard convention is to place reusable components in `src/components/`
- Astro components can run client-side JS via the `<script>` tag. JS/TS files can be imported directly via the script tag
- Import global styles in Layout components so that they are applied across multiple components/pages that share the common layout

## Part 4 Notes - Layouts

- Layout components are used to share page formatting styles and render children via the `<slot/>` component
- By convention, place Layout components in `src/layouts/`
- Astro component props can be accessed within the code fences (`---`) via global `Astro.props` instance
- Markdown files can select a Layout component to render its content via the `layout` frontmatter property

```mdx
---
layout: ../../filepath/to/Layout.astro
---
```

## Part 5 Notes - Astro API

- Use `Astro.glob()` to access data from files in your project

```js
---
const allPosts = await Astro.glob('../pages/posts/*.md');
---
```

- Use `getStaticPaths()` to create multiple pages (routes) at once

```js
export async function getStaticPaths() {
  const allPosts = await Astro.glob('../posts/*.md');
  
  const uniqueTags = [...new Set(allPosts.map((post) => post.frontmatter.tags).flat())];

  return uniqueTags.map((tag) => {
    const filteredPosts = allPosts.filter((post) => post.frontmatter.tags.includes(tag));
    return {
      params: { tag },
      props: { posts: filteredPosts },
    };
  });
}
```

## Part 6 Notes - Astro Islands

- A pattern to add client-side interactive content via external frameworks