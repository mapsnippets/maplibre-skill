# MapLibre GL JS & Ecosystem Versions 📦⚡

This guide lists the current production versions of MapLibre GL JS, verified official companion plugins, compatible spatial utilities, and MapTiler Planet v4 style endpoints. Use these versions when creating HTML scripts, `package.json` dependencies, or CDN links.

---

## 1. Core Library & Companion Plugins Matrix

| Library / Package | Current Version | Ingestion / Type | Primary Purpose | Used in Recipe / Guide |
| :--- | :--- | :--- | :--- | :--- |
| **maplibre-gl** | `6.7.0` | ESM Module (`.mjs`) / NPM | Core WebGL2 vector mapping engine | Core basemaps & all 40 recipes |
| **pmtiles** | `3.2.0` | ESM / Protocol Handler | Serverless cloud-optimized archive tile extraction | `pmtiles-protocol.md` |
| **maplibre-contour** | `0.1.0` | ESM / DemSource Plugin | Real-time client-side contour line & hillshade generation | `vector-contour-lines.md` |
| **@mapbox/mapbox-gl-draw** | `1.4.3` | UMD / ESM | Vector geometry drawing, editing, and CAD digitization | `plugins-catalog.md` |
| **terra-draw** | `1.0.0` | ESM / NPM | Modern multi-engine map drawing library | `plugins-catalog.md` |
| **@turf/turf** | `7.2.0` | ESM / Standalone Script | Advanced geospatial calculations, clipping, and buffers | `turf-distance-measurement.md` |
| **@maptiler/geocoding-control** | `2.1.4` | ESM / IControl | Address search, forward/reverse geocoding autocomplete | `geocoding-and-services.md` |
| **three** | `0.184.0` | ESM | Custom WebGL 3D mesh layers rendered into the map context | `custom-layer-threejs.md` |
| **react-map-gl** | `7.1.7` | ESM / React Bindings | React component wrapper for MapLibre (`react-map-gl/maplibre`) | `frameworks.md` |

> [!IMPORTANT]
> **MapLibre GL JS v6 ESM Standard:**
> In `v6.0.0+`, MapLibre removed the deprecated UMD bundle (`dist/maplibre-gl.js`). Always ingest MapLibre as an ES module:
> ```html
> <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
> <script type="module">
>   import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs';
>   // Initialize map
> </script>
> ```
> In modern bundlers (Vite, Webpack, Next.js, Rollup):
> ```bash
> npm install maplibre-gl@6.7.0
> ```
> ```javascript
> import * as maplibregl from 'maplibre-gl';
> import 'maplibre-gl/dist/maplibre-gl.css';
> ```

---

## 2. Official CDN Endpoints

### MapLibre GL JS v6.7.0
* **ESM Module:** `https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs`
* **CSS Stylesheet:** `https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css`
* **JsDelivr Fallback:** `https://cdn.jsdelivr.net/npm/maplibre-gl@6.7.0/dist/maplibre-gl.mjs`

### Companion Plugins (CDN)
* **PMTiles v3.2.0:** `https://unpkg.com/pmtiles@3.2.0/dist/pmtiles.js`
* **MapLibre Contour v0.1.0:** `https://unpkg.com/maplibre-contour@0.1.0/dist/index.min.js`
* **Mapbox GL Draw v1.4.3:**
  - JS: `https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.4.3/mapbox-gl-draw.js`
  - CSS: `https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.4.3/mapbox-gl-draw.css`
* **Turf.js v7.2.0:** `https://cdn.jsdelivr.net/npm/@turf/turf@7.2.0/turf.min.js`
* **Three.js v0.184.0:** `https://unpkg.com/three@0.184.0/build/three.module.js`

---

## 3. MapTiler Planet v4 Basemap Registry

Use these modern V4 style IDs in your `style` URL: `https://api.maptiler.com/maps/<style-id>/style.json?key=YOUR_API_KEY`

| Style ID (`<style-id>`) | Variants | Category | Best Use Case |
| :--- | :--- | :--- | :--- |
| **`streets-v4`** | `streets-v4-dark`, `streets-v4-pastel` | Vector | Default general-purpose street map with clear highway hierarchy |
| **`outdoor-v4`** | `outdoor-v4-dark` | Vector + DEM | Topographic hiking/trail map with hillshading and contour lines |
| **`satellite-v4`** | `satellite-v4-dark` | Raster Orthophoto | High-resolution satellite imagery without labels |
| **`hybrid-v4`** | `hybrid-v4-dark` | Hybrid | Satellite imagery overlaid with vector roads, borders, and labels |
| **`dataviz-v4-dark`**| `dataviz-v4-light` | Minimal Vector | Clean, muted basemap optimized for dashboards, charts, and GeoJSON overlays |
| **`base-v4`** | `base-v4-dark`, `base-v4-light`, `base-v4-ai` | Minimal Vector | Minimalist base layout (replaces deprecated `basic-v2`) |
| **`winter-v4`** | `winter-v4-dark` | Vector + DEM | Ski resorts, snow sports, piste ratings, and lifts |
| **`ocean-v4`** | `ocean-v4-dark` | Vector Bathymetry | Marine navigation, bathymetric contours, and depths |
| **`landscape-v4`** | `landscape-v4-dark`, `landscape-v4-vivid` | Vector + DEM | Physical geography, land cover classification, and natural features |
| **`topo-v4`** | `topo-v4-dark`, `topo-v4-pastel` | Topographic | High-detail topographic survey styling |

---

## 4. Legacy-to-Modern Style Translation Table

> [!WARNING]
> All `v2` style variants are deprecated. Always replace legacy strings with their modern **V4** production counterparts:

| Deprecated Key (v2) | Modern Replacement (v4) | Notes / Action |
| :--- | :--- | :--- |
| `streets-v2` | `streets-v4` | Full Planet v4 schema update |
| `streets-v2-dark` / `-night` | `streets-v4-dark` | Dark mode street basemap |
| `streets-v2-pastel` | `streets-v4-pastel` | Low-contrast pastel palette |
| `basic-v2` | `base-v4` | Replace deprecated basic-v2 with modern base-v4 |
| `basic-v2-dark` | `base-v4-dark` | Dark minimalist base |
| `basic-v2-light` | `base-v4-light` | Light minimalist base |
| `outdoor-v2` | `outdoor-v4` | Detailed hiking contours & peaks |
| `outdoor-v2-dark` | `outdoor-v4-dark` | Dark mode outdoor map |
| `satellite` / `satellite-v2` | `satellite-v4` | Clean cloudless satellite imagery |
| `hybrid` / `hybrid-v2` | `hybrid-v4` | Satellite with roads and labels |
| `dataviz` / `dataviz-dark` | `dataviz-v4-dark` | High-contrast data visualization |
| `dataviz-light` | `dataviz-v4-light` | Clean light data visualization |
| `toner-v2` | `base-v4-dark` | High-contrast monochrome dark |
| `voyager-v2` | `streets-v4-pastel` or `base-v4-light` | Muted neutral tones |
| `topo-v2` | `topo-v4` | Standard topographic relief |
