- [Astro-Demo-Notes](#astro-demo-notes)
  - [Start Here Notes](#start-here-notes)

# Astro-Demo-Notes

- [[Astro]] is an **all-in-one web framework** for building fast, [[content-focused]] websites.
- Features:
  - [[Component Islands]]
  - [[Server-first API design]]
  - [[Zero JS, by default]]
  - [[Edge-ready]]
  - [[Customizable]]
  - [[UI-agnostic]]

## Start Here Notes

- Create a new `Astro` project via the automated CLI using `pnpm`

```bash
# create a new project with pnpm
pnpm create astro@latest
```

- Setup `tsconfig.json`. [strictest](https://github.com/withastro/astro/tree/main/packages/astro/tsconfigs) is best - imo

```json
  {
    "extends": "base" | "strict" | "strictest"
  }
```

- Add prettier support for `.astro` files

```bash
pnpm i -D prettier prettier-plugin-astro
```

- Tell VSCode to use `prettier` to format `.astro` files
  - In `settings.json`

```json
{
  "prettier.documentSelectors": ["**/*.astro"],
  "[astro]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

- Setup `eslint-plugin-astro`

```bash
pnpm install --save-dev eslint eslint-plugin-astro
```

- Setup `TypeScript` in `.astro`

```bash
pnpm install --save-dev @typescript-eslint/parser
```

- Setup `a11y`

```bash
pnpm install --save-dev eslint-plugin-jsx-a11y
```

- Configure ESLint via `.eslintrc.cjs`

```js
module.exports = {
  plugins: ['jsx-a11y'],
  // ...
  extends: [
    // ...
    'plugin:astro/recommended',
    'plugin:jsx-a11y/recommended',
  ],
  // ...
  overrides: [
    {
      // Define the configuration for `.astro` file.
      files: ['*.astro'],
      // Allows Astro components to be parsed.
      parser: 'astro-eslint-parser',
      // Parse the script in `.astro` as TypeScript by adding the following configuration.
      // It's the setting you need when using TypeScript.
      parserOptions: {
        parser: '@typescript-eslint/parser',
        extraFileExtensions: ['.astro'],
      },
      rules: {
        // override/add rules settings here, such as:
        // "astro/no-set-html-directive": "error"
      },
    },
    // ...
  ],
}
```

- Configure `VSCode` to use `ESLint` to validate `.astro` files
  - In `settings.json`

```json
{
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "astro", // Enable .astro
    "typescript", // Enable .ts
    "typescriptreact" // Enable .tsx
  ]
}
```

- **Aliasing**

```json
{
  "extends": "astro/tsconfigs/strictest",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
		"@components/*": ["src/components/*"],
		"@layouts/*": ["src/layouts/*"],
		"@pages/*": ["src/pages/*"],
		"@styles/*": ["src/styles/*"],
		"@scripts/*": ["src/scripts/*"]
    },
    "jsx": "react-jsx",
	"jsxImportSource": "preact"
  }
}
```

- Fix bug with `import` being a reserve keyword lint error by:
  - In `.eslintrc.cjs`

```js
  parserOptions: {
    sourceType: "module",
    ecmaVersion: "latest"
},
```

- Change mdx syntax highlighting theme via `astro.config.mjs`
  - Add to `defineConfig`

```js
 markdown: {
        shikiConfig: {
          // Choose from Shiki's built-in themes (or add your own)
          // https://github.com/shikijs/shiki/blob/main/docs/themes.md
          theme: 'material-palenight',
          // Add custom languages
          // Note: Shiki has countless langs built-in, including .astro!
          // https://github.com/shikijs/shiki/blob/main/docs/languages.md
          langs: [],
          // Enable word wrap to prevent horizontal scrolling
          wrap: true,
        },
      },
```
