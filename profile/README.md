# PasteStack

**PasteStack is a cross-language front-end platform built on a unified asset graph, a modular UI kit, and a multi-runtime surface rendering engine.**  
At its foundation lies **paste**, a minimalist JavaScript library originally created in **2011** to power high-performance webviews inside early iPhone applications.

PasteStack carries that spirit forward—speed, simplicity, modularity—while expanding it into a modern ecosystem for consistent UI surfaces across **Scala**, **Node**, **Python**, and more.

---

## 🏛️ Origins: The Story of *paste* (2011 → Today)

In **2011**, *paste* began as a lightweight, dependency-free JavaScript toolkit designed to:

- optimize performance inside iPhone WebViews  
- outperform jQuery, YUI, Zepto, and other giants  
- provide essential utilities with near-zero overhead  
- remain tiny (~11k/8k gzipped) and highly modular  
- support dependency management and topological JS loading  

According to the original README (2011) :contentReference[oaicite:0]{index=0}, paste included modules such as:

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

## 🚀 Evolution: From *paste* → PasteStack (2025)

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

PasteStack consists of four major layers:

---

### 1. **paste (Foundational JavaScript Library)**  
The original minimalist JS library lives on here.

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

### 2. **Elements (UI Kit)**

**`paste-elements`**  
Source-only JS + SCSS components (e.g., sticky nav, scroll behaviors, utility helpers).

**`paste-elements-webjar`**  
Optional WebJar packaging for JVM/Scala applications.

---

### 3. **AssetGraph (Asset Pipeline)**

**`paste-assetgraph`**  
A Node/TypeScript pipeline that:

- scans UI + JS modules  
- constructs a dependency graph  
- topologically sorts assets  
- compiles + minifies  
- produces hashed output bundles  
- generates a universal `manifest.json`

All Surface implementations rely on this manifest.

---

### 4. **Surface (Cross-Language Rendering Layer)**

**`paste-surface-spec`**  
Defines the shared rendering contract:

- ViewModel schema  
- Component structure  
- Layout rules  
- Asset injection logic  
- `{% paste %}` DSL for templates  

**`paste-surface-scala`**  
Twirl renderer + Akka/Pekko HTTP integration.

**`paste-surface-node`**  
Nunjucks/EJS renderer + Express middleware.

**`paste-surface-python`**  
Jinja2 renderer + Flask/FastAPI integration.

---

## 🧩 How Everything Fits Together

```
paste ← foundational JS utilities

paste-elements ← JS/SCSS UI components
↓
paste-assetgraph ← builds bundles + manifest.json
↓
paste-surface-<runtime> ← templates, ViewModels, asset injection
↓
Your App ← consistent HTML & assets across languages
```



This gives teams consistent, modern tooling regardless of backend language.

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

