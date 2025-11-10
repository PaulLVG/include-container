# include-container

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

This project is a respectful fork and adaptation of the wonderful  
[`include-media`](https://eduardoboucas.github.io/include-media/) library  
originally created by:

- **Eduardo Boucas** – [@eduardoboucas](https://github.com/eduardoboucas)  
- **Kitty Giraudel** – [@KittyGiraudel](https://github.com/KittyGiraudel)

All credits go to them for their elegant Sass design and documentation style.  
This project merely extends their ideas to **CSS Container Queries**.

Maintained by [@PaulLVG](https://github.com/PaulLVG)

---

## Installation

```bash
npm i -D @paullvg/include-container
# or
pnpm add -D @paullvg/include-container
```

Import it in your Sass:

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
@use "@paullvg/include-container/include-container";
```

> If you vend this file directly (not via npm), just `@use "include-container";` with the file in your load path.

---

## Why no media-query fallback?

Container Queries are **contextual**. Falling back to viewport-based media queries can break layouts when a component is designed to react to its **container** (e.g., a small card in a wide desktop page).  
This library **does not** emit any media fallback by design. It optionally wraps output with:

```scss
@supports (container-type: inline-size) { ... }
```

so unsupported browsers just **don’t apply** those rules (safer default).

---

## API

### `@include container($conditions..., $name: null, $type: null, $supports: null)`

- **$conditions...**: one or more shorthand expressions (strings)
  - `'>=md'`, `'<lg'`
  - `'height >= 40ch'`, `'block-size < 60ch'`
- **$name**: optional container name (must match `container-name` in your CSS)
- **$type**: if set to `'size'`, the `@supports` guard uses `(container-type: size)`.
  - When omitted, **inline-size** is assumed.
- **$supports**: override to `false` to skip the `@supports` guard. Defaults to `$ic-guard-supports` (true).

The mixin supports **multiple conditions** by **nesting** `@container` rules (equivalent to logical AND).

#### Examples

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
```

### Backwards-compatible alias

```scss
@include include-container('>sm') { /* ... */ }
```

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
- **npm:** https://www.npmjs.com/package/@paullvg/include-container

---

## License

MIT © [@PaulLVG](https://github.com/PaulLVG)