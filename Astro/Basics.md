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

## Pages

## Layouts

## Markdown & MDX

## Routing

## Imports

## Endpoints

## Data Fetching

## Deploy

## Troubleshooting
