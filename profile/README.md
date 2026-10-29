# PasteStack

**A cross-language front-end platform built around a unified asset graph, a modular UI kit and a multi-runtime surface rendering engine.**

At its foundation is **paste**, a minimalist JavaScript library first written in **2011** to power high-performance webviews inside early iPhone applications. PasteStack carries that spirit forward (speed, simplicity, modularity) into a platform for consistent UI surfaces across server runtimes: Scala today, with more planned.

PasteStack is maintained by [Tomshley](https://github.com/tomshley) and runs [tomshley.com](https://tomshley.com).

---

## Latest releases

| Project | What it does | Latest |
|---|---|---|
| [paste-surface-scala](https://github.com/PasteStack/paste-surface-scala) | Server-built pages: Twirl layouts and shared components, served with Pekko HTTP. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste-surface-scala?sort=semver&label=&color=1e1e47) |
| [paste-surface-spec](https://github.com/PasteStack/paste-surface-spec) | The contract every page renderer follows, written as plain-language tests. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste-surface-spec?sort=semver&label=&color=1e1e47) |
| [paste-elements](https://github.com/PasteStack/paste-elements) | Interface components: sticky navigation, scroll spy, forms that send in the background. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste-elements?sort=semver&label=&color=1e1e47) |
| [paste-assetgraph](https://github.com/PasteStack/paste-assetgraph) | Builds asset bundles and a manifest, so each page asks only for what it uses. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste-assetgraph?sort=semver&label=&color=1e1e47) |
| [paste](https://github.com/PasteStack/paste) | The dependency-free JavaScript toolkit underneath, first written in 2011. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste?sort=semver&label=&color=1e1e47) |
| [paste-webjar](https://github.com/PasteStack/paste-webjar) | paste packaged as a Maven WebJar for JVM builds. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste-webjar?sort=semver&label=&color=1e1e47) |
| [paste-elements-webjar](https://github.com/PasteStack/paste-elements-webjar) | paste-elements packaged as a Maven WebJar for JVM builds. | ![latest](https://img.shields.io/github/v/tag/PasteStack/paste-elements-webjar?sort=semver&label=&color=1e1e47) |

---

## Origins: the story of paste (2011 to today)

In 2011, paste began as a lightweight, dependency-free JavaScript toolkit designed to:

- optimize performance inside iPhone WebViews
- outperform jQuery, YUI, Zepto and other giants
- provide essential utilities with near-zero overhead
- stay tiny (~11k, 8k gzipped) and highly modular
- support dependency management and topological JS loading

According to the original 2011 README, paste included modules such as `paste/dom`, `paste/event`, `paste/oop`, `paste/util`, `paste/io`, `paste/storage`, `paste/featuredetect`, `paste/lru` and `paste/speed`, plus polyfills for older browsers.

Its goal was straightforward:

> **Provide the smallest possible set of utilities to build real applications fast.**

That minimalist design (small modules, tight boundaries and performance-first thinking) became the backbone of PasteStack.

## Evolution: from paste to PasteStack

As applications grew more distributed and multi-language systems became common, paste's original strengths made it the right foundation for a wider platform. PasteStack extends it into:

- a **unified front-end architecture**
- cross-language **surface rendering**
- a deterministic **asset graph and bundler**
- a **source-first UI kit**
- a **templating DSL** for declaring assets
- a consistent **layout and ViewModel** system

paste remains the lowest layer of the stack: lightweight, fast, universal.

---

## Architecture

PasteStack has four layers, plus packaging, connected by **conventions** (CSS classes, element IDs and data attributes) rather than code dependencies.

### 1. paste: the foundational JavaScript library

The original minimalist library, now with ES modules: DOM utilities, an event system, async script loading, feature detection, object utilities, lightweight OOP, LRU caching, performance helpers and polyfills. Everything else in PasteStack builds on it.

### 2. Elements: the UI kit

**`paste-elements`** is source-only JS and SCSS, organized in YUI-style modules:

- **base/**: reset, variables and foundational styles
- **structure/**: grid, layout, spacing and typography
- **modules/**: heroscroll, stickynav, smoothscroll, autogrow and throttle

### 3. AssetGraph: the asset pipeline

**`paste-assetgraph`** is a **Rust** binary that scans UI and JS modules, builds a dependency graph, sorts assets topologically, compiles SCSS to CSS (via grass), minifies JS and CSS, writes hashed bundles and a universal `manifest.json`, and serves JAM (JavaScript Asset Management) combo URLs per the [JAM Spec](https://github.com/PasteStack/paste-assetgraph/blob/main/JAM_SPEC.md).

- **Storage backends:** **local** writes to `target/` for Scala and sbt projects; **dist** writes versioned output with rollback for static sites; **S3** is planned for CDN-backed production.
- **Runtime library:** `paste-assetgraph/runtime/scala` provides Apache Pekko HTTP routes, a manifest reader and an sbt plugin.

### 4. Surface: the cross-language rendering layer

- **`paste-surface-spec`** defines the shared rendering contract: the ViewModel schema, component structure (Hero, Address, FormField), layout rules (webbase, jambase), asset injection and BDD feature specs for the conventions.
- **`paste-surface-scala`** is the Twirl renderer with Apache Pekko HTTP integration.
- **`paste-surface-python`** (planned): a Jinja2 renderer with Flask and FastAPI integration.

### 5. Packaging

**`paste-webjar`** and **`paste-elements-webjar`** package paste and paste-elements as Maven WebJars for JVM build systems.

---

## Convention-based integration

**paste-elements and paste-surface have no code dependencies on each other.** They're connected by conventions:

| Convention | Example | Defined in | Used by |
|---|---|---|---|
| CSS classes | `.paste-ui-hero` | paste-elements SCSS | paste-surface Twirl |
| Element IDs | `#introduction-image` | paste-surface Twirl | paste-elements JS and site CSS |
| Data attributes | `data-paste-parallax` | paste-surface Twirl | paste-elements JS |

That keeps releases decoupled, allows more than one surface implementation, and leaves room for site-level overrides.

## How everything fits together

```
paste              foundational JS utilities
paste-elements     JS and SCSS UI components, YUI-style modules
       ↓
paste-assetgraph   Rust binary: builds bundles and manifest.json
       ↓
paste-surface-*    templates, ViewModels and asset injection
       ↓
your app           the same HTML and assets on every runtime
```

## Where it fits

**Libraries, not a framework.** Each block connects through plain conventions, so your application never depends on it, and any block can be lifted out and replaced.

- Works with any HTTP server that serves HTML and static assets; Twirl and Pekko HTTP are supported today.
- ViewModels and templates are designed to carry across runtimes.
- The asset pipeline runs on its own as a Rust binary, so builds need no JVM or Node.
- The original paste patterns remain: JAM URLs, `paste.define` and `paste.require`, and multi-CDN support.

It's a good fit for internal tools and enterprise applications, server-rendered architectures, organizations with mixed-language stacks, teams that need deterministic, standardized asset loading, performance-sensitive applications and UI modernization.

---

## Contributing

Opinionated, and open to good changes. PasteStack is a multi-repo ecosystem, and each repository says what it's for and what's out of scope. Bug fixes, security, accessibility and documentation improvements are especially welcome.

1. Open an issue in the relevant repository that describes the change.
2. Fork the repository.
3. Send a focused pull request with clean, scoped commits.
4. Found a vulnerability? Report it privately, as the repository's `SECURITY.md` describes, never in a public issue.

## License

Unless otherwise noted, all PasteStack components are licensed under the **Apache License 2.0**, with no license fee. Maintained by Tomshley LLC.

---

PasteStack is the evolution of a fifteen-year-old idea: **simple tools, sharp boundaries and performance-first design**, now scaled into a full, modern front-end platform.
