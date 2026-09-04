# MapLibre GL JS Reference Index 🗂️⚡

> Master index and topic routing directory for all MapLibre GL JS agent references, API standards, official documentation guides, and MapTiler basemap integrations. Load on demand.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Fast Search & Prefix Conventions

Target your `grep` or file searches in `references/` using these prefixes to load the exact guide needed:

| Category | File Prefix | Contents |
| :--- | :--- | :--- |
| **Official Task Examples** | `examples-maplibre-*` | Atomic, copy-pasteable tutorials with full HTML, CSS, and native `maplibregl` JS (Vector map, FlyTo, 3D Terrain, 3D Buildings, Clustering, Hover state, Gradient line, Three.js, PMTiles) |
| **Core API & Architecture** | `api-*`, `architecture-*`, `style-spec-*` | Specifications for MapLibre classes, controls, style spec v8, and WebGL lifecycle |
| **Basemaps, Schemas & Services**| `basemaps-*`, `vector-tile-*`, `geocoding-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |

---

## 📑 Complete Catalog

### 1. Official Step-by-Step Examples (`examples-maplibre-*`):
1. **[examples-maplibre-display-vector-map.md](examples-maplibre-display-vector-map.md)** — Display vector map, NavigationControl, FullscreenControl, ScaleControl, and Streets v4.
2. **[examples-maplibre-fly-to-camera.md](examples-maplibre-fly-to-camera.md)** — Cinematic camera flight navigation, pitch/bearing 3D tilt, and viewport padding margins.
3. **[examples-maplibre-animate-point-along-route.md](examples-maplibre-animate-point-along-route.md)** — Smooth vehicle motion along a LineString route using `requestAnimationFrame`.
4. **[examples-maplibre-3d-terrain-elevation.md](examples-maplibre-3d-terrain-elevation.md)** — 3D digital elevation models with Terrain-RGB, exaggeration, and hillshading.
5. **[examples-maplibre-3d-buildings-extrusion.md](examples-maplibre-3d-buildings-extrusion.md)** — Extruded 3D buildings (`fill-extrusion`), zoom interpolation, and directional sunlight.
6. **[examples-maplibre-marker-clustering.md](examples-maplibre-marker-clustering.md)** — GeoJSON point clustering, step-function radius/color ramps, and cluster expansion zoom.
7. **[examples-maplibre-hover-feature-state.md](examples-maplibre-hover-feature-state.md)** — 60 FPS polygon hover highlight using `map.setFeatureState` and paint expressions.
8. **[examples-maplibre-gradient-line.md](examples-maplibre-gradient-line.md)** — Linear color gradient along a polyline length with `lineMetrics: true` and `line-gradient`.
9. **[examples-maplibre-spatial-query-inspector.md](examples-maplibre-spatial-query-inspector.md)** — `map.queryRenderedFeatures` bounding box inspection and property readout popups.
10. **[examples-maplibre-custom-html-markers.md](examples-maplibre-custom-html-markers.md)** — Custom HTML markers with animated CSS radar pulse beacons and popups.
11. **[examples-maplibre-custom-layer-threejs.md](examples-maplibre-custom-layer-threejs.md)** — Custom WebGL layer embedding a Three.js 3D model with `MercatorCoordinate`.
12. **[examples-maplibre-pmtiles-protocol.md](examples-maplibre-pmtiles-protocol.md)** — Streaming serverless vector tiles using `maplibregl.addProtocol` and PMTiles.

### 2. Core API Specifications & Guides:
13. **[api-classes-and-controls.md](api-classes-and-controls.md)** — `Map` methods, custom `IControl` interface, runtime styling (`setPaintProperty`), `Marker`, `Popup`.
14. **[style-spec-reference.md](style-spec-reference.md)** — MapLibre Style Specification v8, root properties, 9 layer types, expressions DSL.
15. **[sources-layers.md](sources-layers.md)** — Dynamic source/layer management (`vector`, `raster`, `raster-dem`, `geojson`, `image`, `video`).
16. **[expressions.md](expressions.md)** — Comprehensive expression syntax guide (interpolate, step, case, match, math).
17. **[architecture-and-guides.md](architecture-and-guides.md)** — WebGL 16-context cleanup pattern, Three.js layer integration, custom protocols.
18. **[plugins-catalog.md](plugins-catalog.md)** — Directory of third-party plugins (`@maptiler/geocoding-control`, `terra-draw`, compare).
19. **[frameworks.md](frameworks.md)** — React (`react-map-gl/maplibre`), Next.js SSR fix, Vue 3, Svelte (`svelte-maplibre`), Angular.
20. **[events.md](events.md)** — Pointer tracking, layer events, spatial query listeners.
21. **[patterns-gotchas.md](patterns-gotchas.md)** — Solutions for the top 10 MapLibre bugs (`[lng, lat]` order, missing CSS, context loss).
22. **[prompt-benchmarks.md](prompt-benchmarks.md)** — Standardized MapLibre evaluation prompts and patterns.

### 3. Basemaps, Schemas & Services:
23. **[vector-tile-schemas.md](vector-tile-schemas.md)** — Planet v4 vector tile schema (transportation, building, water, place, poi).
24. **[basemaps-and-terrain.md](basemaps-and-terrain.md)** — Production endpoints for `streets-v4`, `dataviz-v4-dark`, `outdoor-v4`, `satellite-v4`.
25. **[geocoding-and-services.md](geocoding-and-services.md)** — Forward/reverse geocoding, autocomplete search, static maps, and elevation.
