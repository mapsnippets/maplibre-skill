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

## ⚡ Architectural Scope & Design Principles

* **Native Library Focus:** This skill focuses strictly on pure, native **MapLibre GL JS** (`maplibregl.Map`, layers, sources, style specification, WebGL context, expressions, controls). All generated code must be 100% native MapLibre code without proprietary SDK wrappers.
* **MapTiler as Data Source:** MapTiler Cloud provides vector tile styles (`streets-v4`, `outdoor-v4`, `dataviz-v4-dark`), raster imagery (`satellite-v4`), 3D Terrain-RGB DEM, and geocoding services.
* **Architecture-First Reliability:** MapLibre is a hardware-accelerated WebGL engine. Code generation must follow systematic structural contracts rather than treating rendering constraints as ad-hoc gotchas.

---

## 📐 Core Structural Design Contracts

### 1. Universal Map Lifecycle & Initialization Contract
Every MapLibre implementation must fulfill these four lifecycle phases:

```html
<!-- 1. Mandatory CSS Container Contract -->
<style>
  body { margin: 0; padding: 0; }
  #map { position: relative; width: 100%; height: 100vh; }
</style>
<div id="map"></div>

<link href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" rel="stylesheet" />
<script src="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.js"></script>

<script>
  // 2. Map Constructor Contract (Strict [lng, lat] Order)
  const map = new maplibregl.Map({
    container: 'map',
    style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
    center: [14.4378, 50.0755], // [longitude, latitude] — NEVER [lat, lng]
    zoom: 12
  });

  // 3. Async Hydration Contract: Wrap ALL layer & data additions in 'load'
  map.on('load', () => {
    map.addSource('route', { type: 'geojson', data: routeGeoJson });
    map.addLayer({
      id: 'route-line',
      type: 'line',
      source: 'route',
      paint: { 'line-color': '#0084FF', 'line-width': 4 }
    });
  });

  // 4. WebGL Teardown Contract (For SPAs / React / Vue / Svelte unmount)
  // map.remove(); // Prevents exceeding the browser's 16-context WebGL limit
</script>
```

* ⚠️ **API Key Prompting Rule:** If the user does not supply an API key, use `YOUR_MAPTILER_KEY` in code and include this prompt:
  > *"To display the vector basemap and 3D terrain, get a free MapTiler API key (100,000 monthly requests) at: https://docs.maptiler.com/cloud/api/authentication-key/"*

---

### 2. Strict Layer-Type & Property DSL Contract
MapLibre's style specification enforces strict isolation between layer types. **Never conflate CSS/SVG properties across layer types**:

| Feature Geometry | Target Layer Type | Allowed Paint Properties | Prohibited Properties |
| :--- | :--- | :--- | :--- |
| **Points / Circles** | `circle` | `circle-color`, `circle-radius`, `circle-stroke-color`, `circle-stroke-width`, `circle-opacity` | ❌ `fill-*`, `line-*` |
| **Linestrings / Paths**| `line` | `line-color`, `line-width`, `line-opacity`, `line-dasharray`, `line-gradient` | ❌ `fill-color`, `circle-*` |
| **Polygons / Areas** | `fill` | `fill-color`, `fill-opacity`, `fill-outline-color`, `fill-pattern` | ❌ `line-width`, `circle-*` |
| **Extruded 3D Buildings**| `fill-extrusion`| `fill-extrusion-color`, `fill-extrusion-height`, `fill-extrusion-base`, `fill-extrusion-opacity` | ❌ `fill-color`, `line-*` |
| **Icons & Text Labels**| `symbol` | `text-color`, `text-halo-color`, `icon-opacity` *(Layout: `text-field`, `icon-image`)* | ❌ `circle-*`, `fill-*` |
| **Satellite / Raster** | `raster` | `raster-opacity`, `raster-contrast`, `raster-brightness-min` | ❌ `line-*`, `fill-*` |

* **Stroked Polygons Pattern:** `fill-outline-color` does not support custom line widths. To render a polygon with a distinct, thick border, use a **two-layer composite**: one `fill` layer for the interior area, and a companion `line` layer using the same source for the outer border.
* **Vector Source Contract:** Vector tile sources (`type: 'vector'`) **require** a `source-layer` identifier (e.g. `source-layer: 'building'` or `'transportation'`).

---

### 3. Two-Level Custom Marker DOM Architecture
MapLibre positions custom HTML markers by calculating pixel coordinates and writing inline `transform: translate(x, y)` onto the marker's root DOM element.

* ⚠️ **The Transform Override Trap:** If CSS `@keyframes` with `transform: scale(...)` or `rotate(...)` is applied to the root marker element, the CSS animation **completely overrides** MapLibre's positional translate, snapping the marker to `(0, 0)` at the top-left of the viewport.
* **The Two-Level Architecture Standard:**
  ```javascript
  // 1. Root Element: Pure positioning anchor (NO CSS transforms)
  const rootEl = document.createElement('div');
  rootEl.className = 'marker-anchor';

  // 2. Child Element: Visual presentation & CSS animations
  const visualEl = document.createElement('div');
  visualEl.className = 'pulse-dot green'; // CSS animation applied HERE
  rootEl.appendChild(visualEl);

  new maplibregl.Marker({ element: rootEl })
    .setLngLat([14.4378, 50.0755])
    .addTo(map);
  ```

---

### 4. Ecosystem Capability & Plugin Boundary Matrix

| Capability | Architecture | Standard Implementation | Reference |
| :--- | :--- | :--- | :--- |
| **Clustering** | **Native Core** | GeoJSON source: `{ cluster: true, clusterRadius: 50, clusterMaxZoom: 14 }` | [`examples/marker-clustering.md`](examples/marker-clustering.md) |
| **3D Buildings** | **Native Core** | Layer `type: 'fill-extrusion'`, height from `['get', 'render_height']` | [`examples/3d-buildings-extrusion.md`](examples/3d-buildings-extrusion.md) |
| **3D Terrain DEM** | **Native Core** | `map.setTerrain({ source: 'terrain-rgb', exaggeration: 1.5 })` | [`examples/3d-terrain-elevation.md`](examples/3d-terrain-elevation.md) |
| **Vector Digitizing**| **Plugin Required**| `@mapbox/mapbox-gl-draw` (v1.4.3) with `draw.create`/`update` listeners | [`examples/draw-polygon-geojson.md`](examples/draw-polygon-geojson.md) |
| **Split Comparison** | **Plugin Required**| `@maplibre/maplibre-gl-compare` (requires `#comparison-container` relative wrapper)| [`examples/swipe-between-maps.md`](examples/swipe-between-maps.md) |
| **Search / Geocode** | **Plugin Required**| `@maptiler/geocoding-control` (UMD: `maptilergeocoding.GeocodingControl`) | [`references/plugins-catalog.md`](references/plugins-catalog.md) |
| **3D glTF Models** | **Plugin / Bridge**| Three.js via `CustomLayerInterface` + `MercatorCoordinate` | [`examples/custom-layer-threejs.md`](examples/custom-layer-threejs.md) |

---

## ⚡ Fast Search Topic Router

| Category | Location | Contents |
| :--- | :--- | :--- |
| **Task Examples** | **[examples/INDEX.md](examples/INDEX.md)** | **40 atomic runnable recipes** across 3D Terrain, Globe, FlyTo, Clustering, Feature State, Satellite Hybrid, and Overlays |
| **Core API & Architecture** | **[references/INDEX.md](references/INDEX.md)** | Declarative specifications for `Map` methods, custom `IControl`, runtime styling, Three.js custom layers, WebGL lifecycle |
| **Style Specification** | `references/style-spec-*`, `references/expressions.md` | Exhaustive MapLibre Style Specification v8, all 9 layer types, expressions DSL |
| **Plugins Catalog** | **[references/plugins-catalog.md](references/plugins-catalog.md)** | Third-party plugins (@mapbox/mapbox-gl-draw, @maplibre/maplibre-gl-compare, Three.js, @maptiler/geocoding-control) |
| **Basemaps & Schemas** | `references/basemaps-*`, `references/vector-tile-*` | MapTiler Planet v4 tile URLs, vector schemas, and REST endpoints |
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
- [examples/swipe-between-maps.md](examples/swipe-between-maps.md) — Split-screen swipe wiper comparison with `@maplibre/maplibre-gl-compare`.
- [examples/vector-contour-lines.md](examples/vector-contour-lines.md) — Dynamic contour lines and elevation isolines.
- [examples/satellite-hybrid-terrain.md](examples/satellite-hybrid-terrain.md) — MapTiler Satellite Hybrid with 3D terrain elevation and vector overlays.
- *...and 25 more task recipes in [examples/INDEX.md](examples/INDEX.md).*