# MapLibre GL JS v6.7.0 Reference Index 🗂️⚡

> Master index and topic routing directory for all MapLibre GL JS v6.7.0 agent references, API standards, comprehensive documentation guides, and MapTiler basemap integrations. Load on demand.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Fast Search & Directory Routing

| Topic Area | Directory / Prefix | Contents |
| :--- | :--- | :--- |
| **Official API Docs** | **[references/api-catalog.md](api-catalog.md)** | **Exhaustive catalog of all 60 classes, 15 interfaces & global functions** linking to [`maplibre.org/maplibre-gl-js/docs/API/`](https://maplibre.org/maplibre-gl-js/docs/API/) |
| **Task Examples** | **[examples/INDEX.md](../examples/INDEX.md)** | **40 atomic runnable recipes** with full HTML, CSS, and native JS across 3D Terrain, Globe, FlyTo, Clustering, Feature State, Satellite Hybrid, and Overlays |
| **Official Examples**| **[references/examples-catalog.md](examples-catalog.md)** | Full index of all 140 official MapLibre website examples cross-referenced to recipes |
| **Core API & Architecture** | `references/api-*`, `references/architecture-*` | `Map` methods, custom `IControl`, runtime styling, Three.js custom layers, WebGL lifecycle |
| **Style Specification** | `references/style-spec-*`, `references/expressions.md` | Exhaustive MapLibre Style Specification v8, all 9 layer types, expressions DSL |
| **Basemaps, Schemas & Services**| `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |

---

## 📑 Complete Reference Catalog (`references/`)

### 1. Core API Specifications & Guides
* **[api-catalog.md](api-catalog.md)** — **Exhaustive official MapLibre GL JS API directory** (60 classes, 15 interfaces, 21 functions) linking directly to [`maplibre.org/maplibre-gl-js/docs/API/`](https://maplibre.org/maplibre-gl-js/docs/API/).
* **[examples-catalog.md](examples-catalog.md)** — Full index of all 140 official MapLibre website examples cross-referenced to standalone recipes.
* **[style-spec-reference.md](style-spec-reference.md)** — Exhaustive MapLibre Style Specification v8, root properties, 3D terrain, sky, all 9 layer types, and paint/layout properties.
* **[expressions.md](expressions.md)** — Comprehensive expression syntax guide and operator dictionary (`interpolate`, `step`, `case`, `match`, math, typography).
* **[api-classes-and-controls.md](api-classes-and-controls.md)** — `Map` methods, custom `IControl` interface, runtime styling (`setPaintProperty`), `Marker`, `Popup`, protocol handlers.
* **[sources-layers.md](sources-layers.md)** — Dynamic source/layer management (`vector`, `raster`, `raster-dem`, `geojson`, `image`, `video`), sandwich pattern, and style swaps.
* **[examples-catalog.md](examples-catalog.md)** — API-to-Recipe directory cross-referencing all MapLibre APIs to the 41 standalone task recipes.
* **[architecture-and-guides.md](architecture-and-guides.md)** — WebGL 16-context cleanup pattern, Three.js layer integration, custom protocols.
* **[events.md](events.md)** — Map lifecycle, camera physics, pointer tracking, layer-scoped events, and spatial query listeners.
* **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top critical MapLibre bugs (`[lng, lat]` order, missing CSS, context loss, reactive proxy bugs).
* **[plugins-catalog.md](plugins-catalog.md)** — Directory of third-party plugins (`@maptiler/geocoding-control`, `terra-draw`, compare).
* **[frameworks.md](frameworks.md)** — React (`react-map-gl/maplibre`), Next.js SSR fix, Vue 3 (`shallowRef`), Svelte, Angular.
* **[prompt-benchmarks.md](prompt-benchmarks.md)** — Standardized MapLibre evaluation prompts and patterns.

### 2. Basemaps, Schemas & Services
* **[vector-tile-schemas.md](vector-tile-schemas.md)** — Complete Planet v4 vector tile schema (transportation, building, water, place, poi, boundary).
* **[versions.md](versions.md)** — Core MapLibre GL JS v6.7.0, companion plugins, CDN URLs, and V4 styles.
* **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `dataviz-v4-dark`, `outdoor-v4`, `satellite-v4`, and Terrain-RGB.
* **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.

---

> For task-driven implementations with full HTML/CSS/JS, see **[examples/INDEX.md](../examples/INDEX.md)**.
