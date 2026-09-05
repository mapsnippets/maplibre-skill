---
name: maplibre
description: >-
  Expert coding skill for building web maps with MapLibre GL JS. USE WHEN the user wants to create a map, add an interactive map to a web app, display locations or routes, render geographic data, build a store locator, add markers, popups, heatmaps, or clustering, show GeoJSON on a map, create data-driven styling or visual expressions, render 3D terrain, globe view, or 3D buildings, switch to satellite imagery, animate camera movement (flyTo/fitBounds), add drawing/measuring tools, integrate maps in React, Next.js, Vue, or Svelte, or optimize map performance. Also USE WHEN the user mentions MapLibre, maplibre-gl, vector map, WebGL map, or MapTiler vector basemaps.
license: MIT
metadata:
  author: mapsnippets
  homepage: https://mapsnippets.org/
---

# MapLibre GL JS — Agent Skill 🗺️⚡

> The authoritative AI coding standard for building fast, hardware-accelerated vector web maps with **MapLibre GL JS** using MapTiler as the primary basemap and geospatial data source.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## ⚡ Architectural Scope & Data Reference Invariants

* **Native Library Focus:** This skill focuses strictly on pure, native **MapLibre GL JS** (`maplibregl.Map`, layers, sources, style specification, WebGL context, expressions, controls). All generated code must be 100% native MapLibre code without proprietary SDK wrappers.
* **MapTiler as Data Source:** MapTiler Cloud provides vector tile styles, raster tiles, 3D Terrain-RGB DEM, and geocoding services.

---

## ⚡ Critical Invariants & Rules

Follow these rules on every MapLibre GL JS code generation to prevent bugs:

### 1. 🌐 Pure Native Library Imports
* Always use standard MapLibre GL JS packages and CSS:
  ```html
  <!-- ESM / Modern Browser Module -->
  <script type="module">
    import * as maplibregl from "https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs";
  </script>
  <link href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" rel="stylesheet" />
  ```
  ```javascript
  // Bundler (npm)
  import maplibregl from "maplibre-gl";
  import "maplibre-gl/dist/maplibre-gl.css";
  ```

### 2. 🌍 Coordinate Order: `[longitude, latitude]`
* MapLibre strictly uses **`[longitude, latitude]`** order (matching GeoJSON specifications).
* **Never** pass `[lat, lng]` (Leaflet convention) — this places the map in Antarctica or the middle of the ocean.

### 3. 📐 Mandatory Explicit Container Dimensions
* The map container element (`<div id="map"></div>`) **must** have explicit CSS dimensions (e.g. `height: 100vh; width: 100%; position: relative;`). If dimensions are missing, the map renders with 0px height and appears completely blank.

### 4. ⏳ Lifecycle Invariant: Wait for `map.on('load', ...)`
* Never call `map.addSource()`, `map.addLayer()`, or `map.setPaintProperty()` immediately after initializing `new maplibregl.Map()`.
* Always wrap custom data and layer setup inside the `map.on('load', ...)` callback to ensure the style schema is initialized:
  ```javascript
  map.on('load', () => {
    map.addSource('places', { type: 'geojson', data: geojsonData });
    map.addLayer({ id: 'places-circle', type: 'circle', source: 'places', paint: { 'circle-color': '#0084FF' } });
  });
  ```

### 5. 🧹 Component Lifecycle & WebGL Context Teardown
* Browsers enforce a strict limit of **16 active WebGL contexts**. In single-page applications (React, Next.js, Vue, Svelte), always invoke `map.remove()` on component teardown/unmount:
  ```javascript
  // React cleanup
  useEffect(() => {
    const map = new maplibregl.Map({ container: mapRef.current, ... });
    return () => map.remove();
  }, []);
  ```

### 6. 🔑 Free Basemap API Key Prompting Invariant
* If the user does not supply an API key, use `YOUR_MAPTILER_KEY` as the placeholder in code AND include a friendly reminder:
  > *"To display the vector basemap and 3D terrain, get a free MapTiler API key (100,000 monthly requests) at: https://docs.maptiler.com/cloud/api/authentication-key/"*

---

## ⚡ Fast Search Topic Router

To quickly find the exact MapLibre implementation guide or API specification, use direct directory routing:

| Category | Location | Contents |
| :--- | :--- | :--- |
| **Task Examples** | **[examples/INDEX.md](examples/INDEX.md)** | **atomic runnable recipes** with full HTML, CSS, and native JS across 3D Terrain, Globe, FlyTo, Clustering, Feature State, Satellite Hybrid, and Overlays |
| **Core API & Architecture** | **[references/INDEX.md](references/INDEX.md)** | Declarative specifications for `Map` methods, custom `IControl`, runtime styling, Three.js custom layers, WebGL lifecycle |
| **Style Specification** | `references/style-spec-*`, `references/expressions.md` | Exhaustive MapLibre Style Specification v8, all 9 layer types, expressions DSL |
| **Basemaps, Schemas & Services**| `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |
| **Plugins Catalog** | **[references/plugins-catalog.md](references/plugins-catalog.md)** | Third-party plugins (@mapbox/mapbox-gl-draw, @maplibre/maplibre-gl-compare, Three.js, @maptiler/geocoding-control) |
| **Package Versions** | **[references/versions.md](references/versions.md)** | Pinned production releases for MapLibre GL JS (`v6.7.0`) and companion plugins |

---

## 🧪 Runnable Task Examples (`examples/`)

All task examples are self-contained with complete HTML, CSS, and native MapLibre GL JS code (`new maplibregl.Map(...)`) using modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. Browse **[examples/INDEX.md](examples/INDEX.md)** for the complete categorized catalog:

- [examples/display-vector-map.md](examples/display-vector-map.md) — Vector map with MapTiler Streets v4, NavigationControl, and FullscreenControl.
- [examples/switch-map-styles.md](examples/switch-map-styles.md) — Runtime basemap style switcher toggling Streets, Outdoor, and Satellite.
- [examples/globe-projection.md](examples/globe-projection.md) — Interactive 3D globe view projection at low zoom levels.
- [examples/fly-to-camera.md](examples/fly-to-camera.md) — Cinematic camera flight navigation with pitch, bearing, and curve controls.
- [examples/animate-point-along-route.md](examples/animate-point-along-route.md) — Smooth 60 FPS marker interpolation along a GeoJSON line.
- [examples/3d-terrain-elevation.md](examples/3d-terrain-elevation.md) — Hardware-accelerated 3D DEM elevation using Terrain-RGB tiles.
- [examples/3d-buildings-extrusion.md](examples/3d-buildings-extrusion.md) — Vector building footprints extruded to 3D with height expressions.
- [examples/marker-clustering.md](examples/marker-clustering.md) — Native GeoJSON source-level clustering, step styling, and click expansion.
- [examples/hover-feature-state.md](examples/hover-feature-state.md) — 60 FPS polygon boundary hover highlights with `map.setFeatureState`.
- [examples/gradient-line.md](examples/gradient-line.md) — Multi-color gradient routes using `line-gradient` and `lineMetrics`.
- [examples/custom-layer-threejs.md](examples/custom-layer-threejs.md) — Custom WebGL layer embedding a 3D Three.js model with `MercatorCoordinate`.
- [examples/draw-polygon-geojson.md](examples/draw-polygon-geojson.md) — Interactive polygon drawing and GeoJSON coordinate export with `MapboxDraw`.
- [examples/satellite-hybrid-terrain.md](examples/satellite-hybrid-terrain.md) — MapTiler Satellite Hybrid with 3D terrain elevation and vector overlays.
- *...and 19 more task recipes in [examples/INDEX.md](examples/INDEX.md).*

---

## 📚 Core API & Architecture References (`references/`)

Deep architectural and schema reference files live under `references/` and should be loaded on demand:

| Reference Guide | Core Focus Areas |
| :--- | :--- |
| **[references/INDEX.md](references/INDEX.md)** | **Master API Reference index and technical router** |
| **[references/api-catalog.md](references/api-catalog.md)** | **Official MapLibre GL JS API Directory** (60 classes, 15 interfaces, 21 functions mapped to `maplibre.org/maplibre-gl-js/docs/API/`) |
| **[references/versions.md](references/versions.md)** | Standard verified package versions (`maplibre-gl@6.7.0`, `terra-draw`, `@maptiler/geocoding-control`) |
| **[references/api-classes-and-controls.md](references/api-classes-and-controls.md)** | `Map` options, custom `IControl` interface, `Marker`, `Popup`, camera methods |
| **[references/style-spec-reference.md](references/style-spec-reference.md)** | Root style properties, all 9 layer types (`fill`, `line`, `symbol`, `circle`, `fill-extrusion`, `raster`, `hillshade`, `heatmap`, `background`) |
| **[references/sources-layers.md](references/sources-layers.md)** | Source definitions (`vector`, `raster`, `raster-dem`, `geojson`, `image`, `video`), dynamic layer management |
| **[references/expressions.md](references/expressions.md)** | Expression syntax (`interpolate`, `step`, `match`, `case`, `get`, mathematical & color expressions) |
| **[references/architecture-and-guides.md](references/architecture-and-guides.md)** | Custom WebGL layers (Three.js integration), hardware-accelerated shaders, WebGL context limits |
| **[references/frameworks.md](references/frameworks.md)** | Integration guides for React (`react-map-gl/maplibre`), Next.js (SSR hydration guard), Vue 3, Svelte |
| **[references/events.md](references/events.md)** | Complete event system (map lifecycle, pointer tracking, layer-specific event listeners) |
| **[references/patterns-gotchas.md](references/patterns-gotchas.md)** | Top 10 MapLibre pitfalls, debugging techniques, and verified code patterns |
| **[references/vector-tile-schemas.md](references/vector-tile-schemas.md)** | MapTiler Planet v4 vector tile schema (transportation, building, water, place, poi) |
| **[references/basemaps-and-terrain.md](references/basemaps-and-terrain.md)** | Production style URLs for `streets-v4`, `outdoor-v4`, `dataviz-dark`, `satellite-v4`, and Terrain-RGB |
| **[references/geocoding-and-services.md](references/geocoding-and-services.md)** | Forward/reverse geocoding, autocomplete search, static map generator, elevation API |

---

## ⚠️ Critical Gotchas to Avoid

1. **`[lng, lat]` vs `[lat, lng]` Coordinate Inversion:**
   - MapLibre uses `[lng, lat]` (e.g. `[14.4378, 50.0755]` for Prague). Passing `[50.0755, 14.4378]` places the map near the equator or Somalia.
2. **Missing Container Height in CSS:**
   - Always define `#map { width: 100%; height: 100vh; position: relative; }`. If omitted, the map initializes with 0px height and appears blank.
3. **Calling Layer APIs Before `load` Event:**
   - Never call `map.addSource` or `map.addLayer` before `map.on('load')` fires. Doing so throws `'Style is not done loading'` error.
4. **WebGL Context Leaks in SPAs:**
   - Always call `map.remove()` on component unmount (React `useEffect` cleanup) to avoid exceeding the 16 WebGL context browser limit.
5. **Layer `source-layer` Missing on Vector Sources:**
   - Vector tile sources (`type: "vector"`) require a `source-layer` property (e.g. `source-layer: "building"` or `"water"`). Omitting it prevents features from rendering.
6. **Custom Animated Marker CSS Transform Trap:**
   - Never apply CSS `@keyframes` with `transform: scale(...)` or `rotate(...)` directly to the root element passed to `new maplibregl.Marker({ element })`. CSS animation transforms override MapLibre's inline `translate(x, y)` positioning, causing markers to snap to `(0, 0)` at the top-left corner. Always apply transform animations to an **inner child element**.
7. **Split-Screen Swipe Comparison Requirements:**
   - Swipe comparison requires `@maplibre/maplibre-gl-compare` (`maplibregl.Compare`) with two synchronized maps (`#before` and `#after`) inside `#comparison-container`. The container **must** have `position: relative; overflow: hidden;` and child `.map` containers **must** have `position: absolute; top: 0; bottom: 0; width: 100%;`.
8. **Strict Layer-Type Paint Property Mismatches:**
   - Paint properties are strictly layer-type specific. A `line` layer requires `line-color` (never `fill-color`), a `fill` layer requires `fill-color`, and a `circle` layer requires `circle-color`. Passing a mismatched paint property causes MapLibre to throw `unknown property` and fail to render the layer.