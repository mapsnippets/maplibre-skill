# MapLibre GL JS Reference Index 🗂️⚡

> Master index and topic routing directory for all MapLibre GL JS agent references, API standards, official documentation guides, and MapTiler basemap integrations. Load on demand.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Fast Search & Directory Routing

| Topic Area | Directory / Prefix | Contents |
| :--- | :--- | :--- |
| **Official Task Examples** | **[examples/INDEX.md](../examples/INDEX.md)** | **30 atomic official examples** with full HTML, CSS, and native JS across 3D Terrain, Globe, FlyTo, Clustering, Feature State, PMTiles, and Overlays |
| **Core API & Architecture** | `references/api-*`, `references/architecture-*` | `Map` methods, custom `IControl`, runtime styling, Three.js custom layers, WebGL lifecycle |
| **Style Specification** | `references/style-spec-*`, `references/expressions.md` | Exhaustive MapLibre Style Specification v8, all 9 layer types, expressions DSL |
| **Basemaps, Schemas & Services**| `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |

---

## 📑 Complete Reference Catalog (`references/`)

### 1. Core API Specifications & Guides
* **[api-classes-and-controls.md](api-classes-and-controls.md)** — `Map` methods, custom `IControl` interface, runtime styling (`setPaintProperty`), `Marker`, `Popup`.
* **[style-spec-reference.md](style-spec-reference.md)** — MapLibre Style Specification v8, root properties, 9 layer types, expressions DSL.
* **[sources-layers.md](sources-layers.md)** — Dynamic source/layer management (`vector`, `raster`, `raster-dem`, `geojson`, `image`, `video`).
* **[expressions.md](expressions.md)** — Comprehensive expression syntax guide (interpolate, step, case, match, math).
* **[architecture-and-guides.md](architecture-and-guides.md)** — WebGL 16-context cleanup pattern, Three.js layer integration, custom protocols.
* **[plugins-catalog.md](plugins-catalog.md)** — Directory of third-party plugins (`@maptiler/geocoding-control`, `terra-draw`, compare).
* **[frameworks.md](frameworks.md)** — React (`react-map-gl/maplibre`), Next.js SSR fix, Vue 3, Svelte (`svelte-maplibre`), Angular.
* **[events.md](events.md)** — Pointer tracking, layer events, spatial query listeners.
* **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 10 MapLibre bugs (`[lng, lat]` order, missing CSS, context loss).
* **[prompt-benchmarks.md](prompt-benchmarks.md)** — Standardized MapLibre evaluation prompts and patterns.

### 2. Basemaps, Schemas & Services
* **[vector-tile-schemas.md](vector-tile-schemas.md)** — Planet v4 vector tile schema (transportation, building, water, place, poi).
* **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `dataviz-v4-dark`, `outdoor-v4`, `satellite-v4`.
* **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.

---

> For task-driven implementations with full HTML/CSS/JS, see **[examples/INDEX.md](../examples/INDEX.md)**.
