# include-container

[![GitHub release](https://img.shields.io/github/v/release/PaulLVG/include-container?style=flat-square)](https://github.com/PaulLVG/include-container/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](https://github.com/PaulLVG/include-container/blob/main/LICENSE)
[![GitHub Pages](https://img.shields.io/badge/View%20Docs-GitHub%20Pages-green?style=flat-square)](https://paullvg.github.io/include-container/)
[![npm version](https://img.shields.io/npm/v/include-container?style=flat-square)](https://www.npmjs.com/package/include-container)
[![Sass](https://img.shields.io/badge/Sass-compatible-cc6699?style=flat-square&logo=sass)](https://sass-lang.com/)

A tiny Sass helper that ports the beloved [`include-media`](https://eduardoboucas.github.io/include-media/) API to **Container Queries**.

- Same shorthand syntax: `'>=md'`, `'<lg'`, `'height >= 40ch'`  
- No media-query fallback (by design) — **no global layout surprises**  
- Optional `@supports` guard for `container-type` (enabled by default)
- Works with **named containers** (`container-name`) or unnamed ones
- Ships with a sensible default **breakpoints map** and **unit intervals** (including `ch`)

**Author:** [@PaulLVG](https://github.com/PaulLVG)  
**License:** MIT

---

## Credits & Acknowledgements

This project is a respectful fork and adaptation of the wonderful [`include-media`](https://eduardoboucas.github.io/include-media/) library originally created by:

- **Eduardo Boucas** – [@eduardoboucas](https://github.com/eduardoboucas)  
- **Kitty Giraudel** – [@KittyGiraudel](https://github.com/KittyGiraudel)

All credits go to them for their elegant Sass design and documentation style.  
This project merely extends their ideas to **CSS Container Queries**.

Maintained by [@PaulLVG](https://github.com/PaulLVG)

---

## Installation

```bash
npm i -D include-container
# or
pnpm add -D include-container
```

Import it in your Sass folder:

```scss
// Override defaults BEFORE importing (optional)
$ic-breakpoints: (
  'xxs': 320px,
  'xs':  480px,
  'sm':  640px,
  'md':  768px,
  'lg':  1024px,
  'xl':  1280px,
  'xxl': 1536px
);

// Optionally disable @supports guard (not recommended)
$ic-guard-supports: true;

// Now import the library
@use "include-container";
```

> If you vend this file directly (not via npm), just `@use "include-container";` with the file in your load path.

---

## Getting started

### Examples

```scss
// Inline-size by default
@include container('>=md') {
  .card { display: grid; gap: 1rem; }
}

// Height-based container query
@include container('height >= 40ch') {
  .teaser { display: block; }
}

// Named container
@include container('>=lg', $name: 'layout') {
  .sidebar { position: sticky; top: 1rem; }
}

// Combine conditions (min AND max)
@include container('>=sm', '<=lg') {
  .grid { grid-template-columns: repeat(2, 1fr); }
}

// Custom tweakpoint in scope:
@include container-context(($tall: 70ch)) {
  @include container('height>=tall') {
    .hero { align-items: center; }
  }
}
```

### Backwards-compatible alias

Just in case your project already includes a mixin called "container", this syntax also works:

```scss
@include include-container('>sm') { /* ... */ }
```

---

## Browser support

Relies on [CSS Container Queries](https://caniuse.com/css-container-queries), supported by all modern browsers:

- Chrome ≥ 105  
- Edge ≥ 105  
- Firefox ≥ 109  
- Safari ≥ 16.4  

Use the `@supports (container-type: inline-size)` guard to prevent errors on older engines.

---

## Why another library?

include-container brings the same developer experience as include-media — but for Container Queries.
No reinvented syntax, no JavaScript runtime, just pure Sass sugar syntax for modern responsive components.

---

## Why no media-query fallback?

Container Queries are **contextual**. Falling back to viewport-based media queries can break layouts when a component is designed to react to its **container** (e.g., a small card in a wide desktop page).  
This library **does not** emit any media fallback by design. It optionally wraps output with:

```scss
@supports (container-type: inline-size) { ... }
```

So unsupported browsers just **don’t apply** those rules (safer default).

---

## API

### `@include container($conditions..., $name: null, $type: null, $supports: null)`

- **$conditions...**: one or more shorthand expressions (strings)
  - `'>=md'`, `'<lg'`
  - `'height >= 40ch'`, `'block-size < 60ch'`
- **$name**: optional container name (must match `container-name` in your CSS)
- **$type**: if set to `'size'`, the `@supports` guard uses `(container-type: size)`.
  - When omitted, **inline-size** is assumed.
- **$supports**: whether to wrap output in a `@supports` rule. Defaults to `$ic-guard-supports` (true).

The mixin supports **multiple conditions** by **nesting** `@container` rules (equivalent to logical AND).

---

## Configuration

### Breakpoints

Default map (override before `@use`):

```scss
$ic-breakpoints: (
  'xxs': 320px,
  'xs':  480px,
  'sm':  640px,
  'md':  768px,
  'lg':  1024px,
  'xl':  1280px,
  'xxl': 1536px
);
```

You can refer to named breakpoints in conditions, e.g. `'>=md'`, `'<xl'`.

### Unit intervals

For exclusive ranges (`>` / `<`), include-container adjusts the numeric value with a small **unit interval** (like `include-media`). Default:

```scss
$ic-unit-intervals: (
  'px': 1,
  'em': 0.01,
  'rem': 0.1, // common when :root font-size is 62.5%
  'ch': 0.1,  // convenient for typographic widths
  '':  0
);
```

So `'> 40ch'` becomes `(min-width: 40.1ch)` and `'< 60ch'` becomes `(max-width: 59.9ch)`.

### Supports guard

Enabled by default for safety:

```scss
$ic-guard-supports: true;
```

- If `$type: 'size'`, guard uses `(container-type: size)`.
- Otherwise it uses `(container-type: inline-size)`.

Set `$ic-guard-supports: false` only if you’re certain about your target browsers.

---

## Using `ch` units (typographic containers)

`ch` is roughly the width of the “0” glyph in the current font. It’s great for **readable text widths**. Common guidelines:

- Long-form text line length: **55–75ch** sweet spot
- Narrow columns / sidebars: **35–50ch**
- Hero text blocks: **60–75ch**

Examples:

```scss
// Make a card layout once the container is at least a comfortable text width
@include container('>= 60ch') {
  .article-card { display: grid; grid-template-columns: 2fr 1fr; gap: 1.25rem; }
}

// Collapse to a single column below 45ch
@include container('< 45ch') {
  .article-card { grid-template-columns: 1fr; }
}
```

Remember: the parent must establish a container context, e.g.:

```scss
.content-area {
  container-type: inline-size;          // or: size
  container-name: layout;
}
```

---

## Links

- **Author:** [@PaulLVG](https://github.com/PaulLVG)
- **Repo:** https://github.com/PaulLVG/include-container
- **Docs (GitHub Pages):** https://paullvg.github.io/include-container/
- **npm:** https://www.npmjs.com/package/include-container

---

## License

MIT © [@PaulLVG](https://github.com/PaulLVG)