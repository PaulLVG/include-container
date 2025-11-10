# Changelog

All notable changes to **include-container** will be documented in this file.  
This project follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] - 2025-11-10
### 🎉 Initial release

- ✨ First stable release of **include-container**, a Sass mixin library for writing elegant, maintainable, and responsive **container queries**.
- 🧩 Fully compatible API with [`include-media`](https://eduardoboucas.github.io/include-media/), adapted for container queries instead of media queries.
- 🧠 Provides human-friendly syntax such as:
  ```scss
  @include container('>=md') {
    /* Styles here */
  }
  ```
- 🔧 Default breakpoints:
  ```
  xxs: 320px
  xs: 480px
  sm: 640px
  md: 768px
  lg: 1024px
  xl: 1280px
  xxl: 1536px
  ```
- 🧮 Supports all common CSS units including `px`, `em`, `rem`, `%`, `ch`, and viewport units (`vw`, `vh`, `svh`, `lvh`, `dvh`).
- 💡 Defaults to `inline-size` for container queries.
- 🧾 Distributed under the [MIT License](LICENSE).

---

### 🔗 Links
- **Documentation**: [https://paullvg.github.io/include-container/](https://paullvg.github.io/include-container/)
- **Repository**: [https://github.com/PaulLVG/include-container](https://github.com/PaulLVG/include-container)
- **Author**: [PaulLVG](https://github.com/PaulLVG)
- **License**: MIT
