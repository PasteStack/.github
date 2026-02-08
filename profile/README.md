# PasteStack

**PasteStack is a cross-language front-end platform built on a unified asset graph, a modular UI kit, and a multi-runtime surface rendering engine.**  
At its foundation lies **paste**, a minimalist JavaScript library originally created in **2011** to power high-performance webviews inside early iPhone applications.

PasteStack carries that spirit forward—speed, simplicity, modularity—while expanding it into a modern ecosystem for consistent UI surfaces across **Scala**, **Python**, and more.

---

## 📦 Latest Releases

| Project | Version | Description |
|---------|---------|-------------|
| **paste** | `v2.0.0` | ES modules, Mocha/Chai, UI widgets moved to paste-elements |
| **paste-elements** | `v0.1.0` | YUI-style module restructure, heroscroll, stickynav, smoothscroll |
| **paste-surface-scala** | `v0.1.0` | Hero, Address components, webbase/jambase layouts, view models |
| **paste-surface-spec** | `v0.1.0` | OpenAPI spec, BDD features, convention documentation |
| **paste-assetgraph** | `v0.1.0` | Rust pipeline, pluggable storage, Pekko runtime, JAM Spec v1.1 |
| **paste-devkit** | `v0.1.0` | Architecture docs, dev setup guide |
| **paste-webjar** | `v0.1.0` | Maven WebJar packaging for paste core |
| **paste-elements-webjar** | `v0.1.0` | Maven WebJar packaging for paste-elements |

---

## 🏛️ Origins: The Story of *paste* (2011 → Today)
In **2011**, *paste* began as a lightweight, dependency-free JavaScript toolkit designed to:

- optimize performance inside iPhone WebViews  
- outperform jQuery, YUI, Zepto, and other giants  
- provide essential utilities with near-zero overhead  
- remain tiny (~11k/8k gzipped) and highly modular  
- support dependency management and topological JS loading  

According to the original README (2011), paste included modules such as:

- `paste/dom`
- `paste/event`
- `paste/oop`
- `paste/util`
- `paste/io`
- `paste/storage`
- `paste/featuredetect`
- `paste/lru`
- `paste/speed`
- plus polyfills for older browsers

Its goal was straightforward:

> **Provide the smallest possible set of utilities to build real applications fast.**

This minimalist design—small modules, tight boundaries, and performance-first thinking—became the philosophical backbone of everything that PasteStack is today.

---

## 🚀 Evolution: From *paste* → PasteStack (2026)
As applications grew more distributed and multi-language systems became common, the original strengths of paste made it ideal to serve as the foundation for an expanded platform.

PasteStack extends the original ethos into:

- a **unified front-end architecture**
- cross-language **surface rendering**
- a deterministic **asset graph + bundler**
- a **source-first UI kit**
- a **templating DSL** for declaring assets
- a consistent **layout + ViewModel** system

Paste remains the lowest-level base of the stack—lightweight, fast, universal.

---

## 🧱 Architecture Overview
PasteStack consists of four major layers, connected by **conventions** (CSS classes, element IDs, data attributes) rather than code dependencies:

---

### 1. **paste (Foundational JavaScript Library)** — `v2.0.0`
The original minimalist JS library, now with ES modules.

It includes:

- DOM utilities  
- event system  
- async script loading  
- feature detection  
- object utilities  
- lightweight OOP  
- caching (LRU)  
- performance helpers  
- polyfills  

Everything else in PasteStack builds above this layer.

---

### 2. **Elements (UI Kit)** — `v0.1.0`
**`paste-elements`**  
Source-only JS + SCSS components organized in YUI-style module structure:
- **base/** — reset, variables, foundational styles
- **structure/** — grid, layout, spacing, typography
- **modules/** — heroscroll, stickynav, smoothscroll, autogrow, throttle

**`paste-elements-webjar`**  
Optional WebJar packaging for JVM/Scala applications.

---

### 3. **AssetGraph (Asset Pipeline)** — `v0.1.0`
**`paste-assetgraph`**  
A **Rust** binary pipeline that:

- scans UI + JS modules  
- constructs a dependency graph  
- topologically sorts assets  
- compiles SCSS → CSS (via grass)
- minifies JS and CSS  
- produces hashed output bundles  
- generates a universal `manifest.json`
- serves JAM (JavaScript Asset Management) combo URLs per [JAM Spec v1.1](https://gitlab.com/tomshley/brands/global/tware/tech/products/paste/paste-assetgraph/-/blob/main/JAM_SPEC.md)

**Storage backends:**
- **local** — writes to `target/` for Scala/sbt projects
- **dist** — versioned output with rollback support for static sites
- **S3** — *(planned)* CDN-backed production deployments

**Runtime libraries:**
- `paste-assetgraph/runtime/scala` — Apache Pekko HTTP routes, manifest reader, sbt plugin

---

### 4. **Surface (Cross-Language Rendering Layer)**
**`paste-surface-spec`** — `v0.1.0`  
Defines the shared rendering contract:

- ViewModel schema  
- Component structure (Hero, Address, FormField)  
- Layout rules (webbase, jambase)  
- Asset injection logic  
- BDD feature specs for conventions  

**`paste-surface-scala`** — `v0.1.0`  
Twirl renderer + Apache Pekko HTTP integration.

**`paste-surface-python`** *(planned)*  
Jinja2 renderer + Flask/FastAPI integration.

---

### 5. **DevKit & Packaging**
**`paste-devkit`** — `v0.1.0`  
Architecture documentation, dev setup guides, and the PasteStack layer diagram.

**`paste-webjar`** — `v0.1.0`  
Maven WebJar packaging for paste core JS, for use in JVM build systems.

---

## 🔗 Convention-Based Integration

**paste-elements and paste-surface do NOT have code dependencies on each other.**

They are connected by conventions:

| Convention Type | Example | Defined In | Used By |
|-----------------|---------|------------|---------|
| CSS Classes | `.paste-ui-hero` | paste-elements SCSS | paste-surface Twirl |
| Element IDs | `#introduction-image` | paste-surface Twirl | paste-elements JS + Site CSS |
| Data Attributes | `data-paste-parallax` | paste-surface Twirl | paste-elements JS |

This enables decoupled releases, multiple surface implementations, and site-level overrides.

---

## 🧩 How Everything Fits Together

paste ← foundational JS utilities (v2.0.0)

paste-elements ← JS/SCSS UI components, YUI-style modules (v0.1.0) ↓ paste-assetgraph ← Rust binary: builds bundles + manifest.json (v0.1.0) ↓ paste-surface- ← templates, ViewModels, asset injection (v0.1.0) ↓ Your App ← consistent HTML & assets across languages


This gives teams consistent, modern tooling regardless of backend language.

---

## 🤔 Why PasteStack vs Full Frameworks?
| Aspect | Full Frameworks (Play, Next, Django) | PasteStack |
|--------|--------------------------------------|------------|
| **Scope** | Own your whole app | Asset pipeline + templates only |
| **Runtime** | Framework-specific | Works with *any* HTTP server |
| **Lock-in** | Routes, controllers, forms tied to framework | Portable ViewModels + templates |
| **Asset Pipeline** | Plugin-based (sbt-web, webpack) | Rust binary, no JVM/Node for builds |
| **Multi-language** | Single language | Scala, Python, basic HTML |
| **Size** | Heavy (~100+ deps) | Minimal (just what you need) |

**PasteStack is a library, not a framework.**

- Use it with Pekko HTTP, http4s, FastAPI, Flask, or plain HTML
- Same ViewModels and templates work across runtimes
- Asset pipeline runs independently (fast CI, no JVM needed)
- Original paste patterns: JAM URLs, `paste.define`/`paste.require`, multi-CDN support

**When to use a full framework instead:**
- You want forms, CSRF, sessions, i18n out of the box
- You're building a traditional MVC app
- You don't need multi-runtime portability

---

## 🌐 Ideal Use Cases
PasteStack is perfect for:

- internal tools and enterprise applications  
- server-rendered architectures  
- organizations with mixed-language stacks  
- teams needing deterministic, standardized asset loading  
- performance-sensitive applications  
- UI modernization efforts  

---

## 🤝 Contributing
PasteStack is organized as a multi-repo ecosystem.  
Contributions are welcome in any area:

- JS utilities (paste)  
- UI components (paste-elements)  
- asset pipeline (paste-assetgraph)  
- renderer implementations  
- documentation & examples  
- language integrations  
- tooling & testing  

Open a PR or issue in the relevant repository.

---

## 📄 License
Unless otherwise noted, all PasteStack components are **MIT Licensed**.

---

PasteStack is the evolution of a decade-long idea:  
**simple tools, sharp boundaries, and performance-first design**—  
now scaled into a full, modern front-end platform.
