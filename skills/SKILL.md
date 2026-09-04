---
name: maplibre
description: >-
  Expert coding skill for building web maps with MapLibre GL JS (v3-v5). USE WHEN the user wants to create a map, add an interactive map to a web app, display locations or routes, render geographic data, build a store locator, add markers, popups, heatmaps, or clustering, show GeoJSON on a map, create data-driven styling or visual expressions, render 3D terrain, globe view, or 3D buildings, switch to satellite imagery, animate camera movement (flyTo/fitBounds), add drawing/measuring tools, integrate maps in React, Next.js, Vue, or Svelte, or optimize map performance. Also USE WHEN the user mentions MapLibre, maplibre-gl, vector map, WebGL map, or MapTiler vector basemaps.
license: MIT
metadata:
  author: mapsnippets
  homepage: https://mapsnippets.org/
---

# MapLibre GL JS — Agent Skill 🗺️⚡

> The authoritative AI coding standard for building fast, hardware-accelerated vector web maps with **MapLibre GL JS (v3–v5)** using MapTiler as the primary basemap and geospatial data source.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## ⚡ Architectural Scope & Data Reference Invariants

* **Native Library Focus:** This skill focuses strictly on pure, native **MapLibre GL JS** (`maplibregl.Map`, layers, sources, style specification, WebGL context, expressions, controls). All generated code must be 100% native MapLibre code without proprietary SDK wrappers.
* **MapTiler as Data Source:** MapTiler Cloud provides vector tile styles, raster tiles, 3D Terrain-RGB DEM, and geocoding services.
* **Upstream Reference Authority:** All MapTiler style endpoints, raster tiles, terrain DEMs, and vector tile schemas (Planet v4) conform to the authoritative specifications established in the [maptiler/maptiler-skills](https://github.com/maptiler/maptiler-skills) repository. For MapTiler SDK JS wrappers (`@maptiler/sdk`), native mobile SDKs, or on-premise infrastructure, refer to `maptiler-skills`.

---

## Reference Guides & Executable Examples

Deep architectural and schema reference files live under `references/` and should be loaded on demand:
- [references/INDEX.md](references/INDEX.md) — **Master topic index & router** for all MapLibre references and guides.
- [references/api-classes-and-controls.md](references/api-classes-and-controls.md) — Core `maplibregl` classes (`Map`, `Marker`, `Popup`), UI controls (`Navigation`, `Geolocate`, `Scale`, `Fullscreen`), and coordinate types.
- [references/plugins-catalog.md](references/plugins-catalog.md) — Complete catalog of plugins, controls, layer extensions, drawing tools, and framework bridges.
- [references/style-spec-reference.md](references/style-spec-reference.md) — Exhaustive MapLibre Style Specification v8 reference (sources, all 9 layer types, expressions, light, terrain).
- [references/architecture-and-guides.md](references/architecture-and-guides.md) — Deep technical guides on WebGL context management (16 context limit), custom layers (Three.js), and protocol extensions.
- [references/official-examples-catalog.md](references/official-examples-catalog.md) — Complete directory of 70+ official MapLibre examples grouped by category.
- [references/vector-tile-schemas.md](references/vector-tile-schemas.md) — Planet v4 source layers (`transportation`, `building`, `water`, `place`, `poi`, `boundary`, `contour`) and exact field attributes.
- [references/basemaps-and-terrain.md](references/basemaps-and-terrain.md) — Map styles (`streets-v4`, `outdoor-v4`, `satellite-v4`, `dataviz-v4-dark`), high-DPI raster tiles, and 3D Terrain-RGB DEM.
- [references/geocoding-and-services.md](references/geocoding-and-services.md) — Forward/reverse geocoding, search autocomplete, and point elevation REST endpoints.
- [references/patterns-gotchas.md](references/patterns-gotchas.md) — Common lifecycle, coordinate inversion, and context loss gotchas.
- [references/frameworks.md](references/frameworks.md) — React, Next.js, Vue, and Svelte integration patterns.
- [examples/README.md](examples/README.md) — **Standalone executable HTML examples** (vector basemaps, 3D terrain/extrusions, GeoJSON styling, clustering, PMTiles).


> **Important for code generation:** When generating code, always write complete, self-contained HTML files. Do not output code as inline text or markdown code blocks without creating a file.

> [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/docs/) v4.7.1 · [NPM](https://www.npmjs.com/package/maplibre-gl) · [GitHub](https://github.com/maplibre/maplibre-gl-js) · [MapTiler MapLibre Docs](https://docs.maptiler.com/maplibre-gl-js/)

MapLibre GL JS is an open-source TypeScript library for rendering interactive vector maps using WebGL/WebGPU. This skill covers using MapLibre GL JS with **MapTiler Cloud** as the data and basemap provider for vector styles, tiles, 3D terrain, and geocoding services.

---

## 1. Why MapLibre GL JS + MapTiler

MapLibre GL JS is the community-maintained fork of Mapbox GL JS v1 — fully open-source (BSD-3-Clause), with no proprietary restrictions. Combined with MapTiler Cloud as the data provider:

- **Vector tile styles** — Streets, Satellite, Outdoor, Topo, Dataviz, and 16+ styles as style.json
- **Source/layer architecture** — add GeoJSON, vector, raster, image sources with typed layers
- **Expression-based styling** — data-driven colors, sizes, filters using the expression DSL
- **3D terrain** — MapTiler terrain-rgb tiles for realistic elevation rendering
- **Globe projection** — built-in globe view at low zoom levels
- **Built-in clustering** — GeoJSON source-level clustering, no plugins needed
- **Fill-extrusion layers** — 3D building extrusions from vector tile data
- **Heatmap layers** — native heatmap rendering, no plugins
- **Geocoding API** — forward/reverse search via MapTiler REST endpoints
- **60fps WebGL rendering** — hardware-accelerated, smooth camera animations

**When to use MapLibre GL JS vs Leaflet:** Use MapLibre for vector tiles, 3D terrain, globe view, data-driven styling, fill-extrusions, or 1000+ points. Use Leaflet for lightweight raster maps, maximum plugin ecosystem, or simple 2D use cases.

---

## 2. Setup

### CDN (recommended for quick demos)

```html
<script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
<link href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" rel="stylesheet" />
```

### NPM

```bash
npm install maplibre-gl
```

```js
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';
```

### API Key

**Do NOT hardcode a fake API key.** Ask the user for theirs, or instruct them to get one at https://cloud.maptiler.com/account/keys/

Use `YOUR_MAPTILER_KEY` as placeholder in examples.

### Minimal Map

```js
const map = new maplibregl.Map({
  container: 'map',                      // DOM element or ID
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
  center: [14.4178, 50.1167],            // [lng, lat] — NOT [lat, lng]!
  zoom: 12
});
```

> **Critical:** The container element must have explicit dimensions (e.g., `height: 100vh`), otherwise the map is invisible.

> **Critical:** MapLibre uses **`[lng, lat]`** coordinate order — same as GeoJSON, opposite of Leaflet.

---

## 3. Core Concepts

### Map Constructor Options

```js
const map = new maplibregl.Map({
  container: 'map',
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
  center: [lng, lat],               // [lng, lat] — NOT [lat, lng]!
  zoom: 12,
  pitch: 0,                          // 0-85 degrees (camera tilt)
  bearing: 0,                        // map rotation in degrees
  minZoom: 0,
  maxZoom: 22,
  maxPitch: 85,
  hash: false,                       // sync viewport with URL hash
  antialias: true,                   // smoother edges (slight perf cost)
  attributionControl: true,
  cooperativeGestures: false,        // Cmd+scroll to zoom
  maxBounds: [[sw_lng, sw_lat], [ne_lng, ne_lat]],  // optional constraint
});
```

### MapTiler Vector Style URLs

MapTiler provides style.json files that configure sources, layers, fonts, and sprites — everything MapLibre needs.

| Style | URL path |
|-------|----------|
| Streets v4 | `maps/streets-v4/style.json` |
| Streets v4 Dark | `maps/streets-v4-dark/style.json` |
| Streets v4 Pastel | `maps/streets-v4-pastel/style.json` |
| Satellite v4 | `maps/satellite-v4/style.json` |
| Hybrid v4 | `maps/hybrid-v4/style.json` |
| Outdoor v4 | `maps/outdoor-v4/style.json` |
| Topo v4 | `maps/topo-v4/style.json` |
| Dataviz v4 | `maps/dataviz-v4/style.json` |
| Dataviz v4 Dark | `maps/dataviz-v4-dark/style.json` |
| Dataviz v4 Light | `maps/dataviz-v4-light/style.json` |
| Base v4 | `maps/base-v4/style.json` |
| Base v4 Dark | `maps/base-v4-dark/style.json` |
| Base v4 Light | `maps/base-v4-light/style.json` |
| Bright v4 | `maps/bright-v4/style.json` |
| Ocean | `maps/ocean/style.json` |
| Winter v4 | `maps/winter-v4/style.json` |
| Landscape | `maps/landscape/style.json` |
| Backdrop | `maps/backdrop/style.json` |

All URLs are prefixed with `https://api.maptiler.com/` and suffixed with `?key=YOUR_MAPTILER_KEY`.

```js
// Example: switching to satellite
map.setStyle('https://api.maptiler.com/maps/satellite-v4/style.json?key=YOUR_MAPTILER_KEY');
```

> Full style URL reference: `references/basemaps-and-terrain.md`

### Source and Layer Architecture

MapLibre uses a **source → layer** model. Sources hold data; layers define how to render it.

```js
// 1. Add a source (data)
map.addSource('my-points', {
  type: 'geojson',
  data: {
    type: 'FeatureCollection',
    features: [
      { type: 'Feature', geometry: { type: 'Point', coordinates: [14.4178, 50.1167] }, properties: { name: 'Prague' } }
    ]
  }
});

// 2. Add a layer (rendering)
map.addLayer({
  id: 'my-points-layer',
  type: 'circle',
  source: 'my-points',
  paint: {
    'circle-radius': 8,
    'circle-color': '#FF0000'
  }
});
```

**Source types:** `geojson`, `vector`, `raster`, `raster-dem`, `image`, `video`
**Layer types:** `circle`, `line`, `fill`, `fill-extrusion`, `symbol`, `heatmap`, `raster`, `hillshade`, `background`

> Full source/layer reference: `references/sources-layers.md`

### Attribution

**Always include attribution.** MapTiler styles include it automatically via the style.json. If building custom styles, add:

```js
new maplibregl.Map({
  // ...
  attributionControl: true  // default
});
```

---

## 4. Common Recipes

### Markers and Popups

```js
// Basic marker with popup
new maplibregl.Marker({ color: '#FF0000' })
  .setLngLat([14.4178, 50.1167])    // [lng, lat]!
  .setPopup(new maplibregl.Popup({ offset: 25 }).setHTML('<h3>Prague</h3><p>Capital of Czech Republic</p>'))
  .addTo(map);

// Custom HTML marker
const el = document.createElement('div');
el.className = 'custom-marker';
el.style.width = '30px';
el.style.height = '30px';
el.style.backgroundImage = 'url(marker.png)';
el.style.backgroundSize = 'cover';

new maplibregl.Marker({ element: el })
  .setLngLat([14.4178, 50.1167])
  .addTo(map);

// Standalone popup
new maplibregl.Popup({ closeOnClick: false })
  .setLngLat([14.4178, 50.1167])
  .setHTML('<h3>Hello!</h3>')
  .addTo(map);
```

### GeoJSON Source and Layers

```js
map.on('load', () => {
  // Add GeoJSON source
  map.addSource('places', {
    type: 'geojson',
    data: 'https://example.com/places.geojson'  // URL or inline object
  });

  // Render as circles
  map.addLayer({
    id: 'places-circles',
    type: 'circle',
    source: 'places',
    paint: {
      'circle-radius': 6,
      'circle-color': '#0891b2',
      'circle-stroke-width': 2,
      'circle-stroke-color': '#ffffff'
    }
  });

  // Render labels
  map.addLayer({
    id: 'places-labels',
    type: 'symbol',
    source: 'places',
    layout: {
      'text-field': ['get', 'name'],
      'text-font': ['Noto Sans Regular'],
      'text-offset': [0, 1.5],
      'text-anchor': 'top',
      'text-size': 12
    },
    paint: {
      'text-color': '#333333',
      'text-halo-color': '#ffffff',
      'text-halo-width': 1
    }
  });
});
```

### Data-Driven Styling with Expressions

```js
// Color circles by property value
map.addLayer({
  id: 'earthquakes',
  type: 'circle',
  source: 'earthquakes',
  paint: {
    // Size by magnitude
    'circle-radius': [
      'interpolate', ['linear'], ['get', 'magnitude'],
      1, 3,
      5, 15,
      8, 40
    ],
    // Color by magnitude (step expression)
    'circle-color': [
      'step', ['get', 'magnitude'],
      '#51bbd6',    // < 3
      3, '#f1f075', // 3-5
      5, '#f28cb1',  // 5-7
      7, '#e31a1c'   // 7+
    ],
    'circle-opacity': 0.8
  }
});
```

> Full expression reference: `references/expressions.md`

### Forward Geocoding (MapTiler API)

```js
async function geocodeForward(query) {
  const url = `https://api.maptiler.com/geocoding/${encodeURIComponent(query)}.json?key=YOUR_MAPTILER_KEY`;
  const response = await fetch(url);
  const data = await response.json();

  if (data.features.length > 0) {
    const coords = data.features[0].geometry.coordinates; // [lng, lat]
    map.flyTo({ center: coords, zoom: 14 });

    new maplibregl.Marker()
      .setLngLat(coords)
      .setPopup(new maplibregl.Popup().setHTML(`<b>${data.features[0].place_name}</b>`))
      .addTo(map);
  }
}
```

### Reverse Geocoding (MapTiler API)

```js
map.on('click', async (e) => {
  const { lng, lat } = e.lngLat;
  const url = `https://api.maptiler.com/geocoding/${lng},${lat}.json?key=YOUR_MAPTILER_KEY`;
  const response = await fetch(url);
  const data = await response.json();

  if (data.features.length > 0) {
    new maplibregl.Popup()
      .setLngLat(e.lngLat)
      .setHTML(data.features[0].place_name)
      .addTo(map);
  }
});
```

### Marker Clustering (Built-in)

No plugins needed — MapLibre clusters at the GeoJSON source level.

```js
map.on('load', () => {
  map.addSource('points', {
    type: 'geojson',
    data: pointsGeoJSON,
    cluster: true,
    clusterMaxZoom: 14,
    clusterRadius: 50
  });

  // Cluster circles
  map.addLayer({
    id: 'clusters',
    type: 'circle',
    source: 'points',
    filter: ['has', 'point_count'],
    paint: {
      'circle-color': ['step', ['get', 'point_count'], '#51bbd6', 100, '#f1f075', 750, '#f28cb1'],
      'circle-radius': ['step', ['get', 'point_count'], 20, 100, 30, 750, 40]
    }
  });

  // Cluster count labels
  map.addLayer({
    id: 'cluster-count',
    type: 'symbol',
    source: 'points',
    filter: ['has', 'point_count'],
    layout: {
      'text-field': ['get', 'point_count_abbreviated'],
      'text-font': ['Noto Sans Regular'],
      'text-size': 12
    }
  });

  // Individual points
  map.addLayer({
    id: 'unclustered-point',
    type: 'circle',
    source: 'points',
    filter: ['!', ['has', 'point_count']],
    paint: {
      'circle-color': '#11b4da',
      'circle-radius': 6,
      'circle-stroke-width': 1,
      'circle-stroke-color': '#fff'
    }
  });

  // Click cluster to zoom in
  map.on('click', 'clusters', (e) => {
    const features = map.queryRenderedFeatures(e.point, { layers: ['clusters'] });
    const clusterId = features[0].properties.cluster_id;
    map.getSource('points').getClusterExpansionZoom(clusterId, (err, zoom) => {
      if (err) return;
      map.easeTo({ center: features[0].geometry.coordinates, zoom });
    });
  });
});
```

### Heatmap Layer

```js
map.addLayer({
  id: 'heatmap',
  type: 'heatmap',
  source: 'earthquakes',
  paint: {
    'heatmap-weight': ['interpolate', ['linear'], ['get', 'magnitude'], 0, 0, 6, 1],
    'heatmap-intensity': ['interpolate', ['linear'], ['zoom'], 0, 1, 9, 3],
    'heatmap-color': [
      'interpolate', ['linear'], ['heatmap-density'],
      0, 'rgba(33,102,172,0)',
      0.2, 'rgb(103,169,207)',
      0.4, 'rgb(209,229,240)',
      0.6, 'rgb(253,219,199)',
      0.8, 'rgb(239,138,98)',
      1, 'rgb(178,24,43)'
    ],
    'heatmap-radius': ['interpolate', ['linear'], ['zoom'], 0, 2, 9, 20],
    'heatmap-opacity': 0.8
  }
});
```

### 3D Terrain

```js
map.on('load', () => {
  // Add MapTiler terrain source
  map.addSource('terrain', {
    type: 'raster-dem',
    url: 'https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_MAPTILER_KEY',
    tileSize: 256
  });

  // Enable terrain
  map.setTerrain({ source: 'terrain', exaggeration: 1.5 });

  // Optional: add sky layer for atmosphere
  map.addLayer({
    id: 'sky',
    type: 'sky',
    paint: {
      'sky-type': 'atmosphere',
      'sky-atmosphere-sun': [0.0, 90.0],
      'sky-atmosphere-sun-intensity': 15
    }
  });
});
```

### 3D Building Extrusions

MapTiler vector styles provide building footprints in the primary vector tile source (such as `maptiler_planet`). Detect the vector source at runtime:

```js
map.on('load', () => {
  // Detect the active vector tile source name
  const sources = map.getStyle().sources;
  let vectorSource = null;
  for (const [name, src] of Object.entries(sources)) {
    if (src.type === 'vector') { vectorSource = name; break; }
  }
  if (!vectorSource) return;

  map.addLayer({
    id: '3d-buildings',
    source: vectorSource,
    'source-layer': 'building',
    type: 'fill-extrusion',
    minzoom: 14,
    paint: {
      'fill-extrusion-color': [
        'interpolate', ['linear'], ['get', 'render_height'],
        0, '#e0e0e0',
        50, '#c0c0c0',
        100, '#a0a0a0',
        200, '#808080'
      ],
      'fill-extrusion-height': ['get', 'render_height'],
      'fill-extrusion-base': ['get', 'render_min_height'],
      'fill-extrusion-opacity': 0.8
    }
  });
});
```

### Globe Projection

```js
const map = new maplibregl.Map({
  container: 'map',
  style: 'https://api.maptiler.com/maps/satellite-v4/style.json?key=YOUR_MAPTILER_KEY',
  center: [0, 20],
  zoom: 1.5,
  projection: 'globe'     // 'mercator' (default) or 'globe'
});

// Optional: atmosphere
map.on('load', () => {
  map.setFog({
    color: 'rgb(186, 210, 235)',
    'high-color': 'rgb(36, 92, 223)',
    'horizon-blend': 0.02,
    'space-color': 'rgb(11, 11, 25)',
    'star-intensity': 0.6
  });
});
```

### Camera Animations

```js
// Smooth fly animation
map.flyTo({
  center: [14.4178, 50.1167],
  zoom: 15,
  pitch: 60,
  bearing: 30,
  duration: 3000,
  essential: true       // not affected by prefers-reduced-motion
});

// Ease (linear interpolation)
map.easeTo({
  center: [14.4178, 50.1167],
  zoom: 14,
  duration: 1000
});

// Instant jump
map.jumpTo({
  center: [14.4178, 50.1167],
  zoom: 12
});

// Fit to bounds
map.fitBounds(
  [[12.0, 48.5], [18.9, 51.1]],  // [[sw_lng, sw_lat], [ne_lng, ne_lat]]
  { padding: 50, duration: 1000 }
);
```

### Line Layer with Gradient

```js
map.addSource('route', {
  type: 'geojson',
  lineMetrics: true,   // REQUIRED for line-gradient
  data: routeGeoJSON
});

map.addLayer({
  id: 'route-line',
  type: 'line',
  source: 'route',
  layout: {
    'line-join': 'round',
    'line-cap': 'round'
  },
  paint: {
    'line-width': 6,
    'line-gradient': [
      'interpolate', ['linear'], ['line-progress'],
      0, '#0000ff',
      0.5, '#00ff00',
      1, '#ff0000'
    ]
  }
});
```

### Custom Control

```js
class CoordinateControl {
  onAdd(map) {
    this._map = map;
    this._container = document.createElement('div');
    this._container.className = 'maplibregl-ctrl maplibregl-ctrl-group';
    this._container.style.padding = '6px 10px';
    this._container.style.background = 'white';
    this._container.textContent = '-';

    map.on('mousemove', (e) => {
      this._container.textContent =
        `${e.lngLat.lng.toFixed(4)}, ${e.lngLat.lat.toFixed(4)}`;
    });

    return this._container;
  }

  onRemove() {
    this._container.parentNode.removeChild(this._container);
    this._map = undefined;
  }
}

map.addControl(new CoordinateControl(), 'bottom-left');
```

> **Working HTML examples** (complete, copy-paste ready):
> `scripts/basic-map.html`, `scripts/markers-popups.html`, `scripts/geojson-layers.html`,
> `scripts/geocoding-search.html`, `scripts/clustering.html`, `scripts/heatmap.html`,
> `scripts/3d-terrain.html`, `scripts/globe-projection.html`

---

## 5. Framework Integration

### React (react-map-gl)

```bash
npm install react-map-gl maplibre-gl
```

```jsx
import Map, { Marker, Popup, NavigationControl } from 'react-map-gl/maplibre';
import 'maplibre-gl/dist/maplibre-gl.css';

function MapView() {
  return (
    <Map
      initialViewState={{ longitude: 14.4178, latitude: 50.1167, zoom: 12 }}
      style={{ width: '100%', height: '400px' }}
      mapStyle="https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY"
    >
      <NavigationControl position="top-right" />
      <Marker longitude={14.4178} latitude={50.1167} color="#FF0000" />
    </Map>
  );
}
```

**Next.js App Router:** Add `"use client";` at the top. For SSR: `dynamic(() => import('./Map'), { ssr: false })`.

### Vue 3

```bash
npm install maplibre-gl
```

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const container = ref(null);
let map = null;  // plain let, NOT ref() — Vue reactivity on map causes issues

onMounted(() => {
  map = new maplibregl.Map({
    container: container.value,
    style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
    center: [14.4178, 50.1167],
    zoom: 12
  });
});

onUnmounted(() => { map?.remove(); map = null; });
</script>

<template>
  <div ref="container" style="width: 100%; height: 400px" />
</template>
```

> Svelte, Angular, and advanced patterns: `references/frameworks.md`

---

## 6. MapTiler Cloud APIs with MapLibre

Unlike MapTiler SDK, raw MapLibre does not have built-in API wrappers. Use `fetch()` to call MapTiler REST endpoints directly.

| API | Endpoint | Purpose |
|-----|----------|---------|
| Geocoding (forward) | `geocoding/{query}.json` | Search places by name |
| Geocoding (reverse) | `geocoding/{lng},{lat}.json` | Coordinates to address |
| Static Maps | `maps/{style}/static/{lng},{lat},{zoom}/{width}x{height}.png` | Map image URLs |
| Terrain tiles | `tiles/terrain-rgb-v2/tiles.json` | Elevation data (raster-dem) |
| Elevation | `tiles/terrain-rgb-v2/{z}/{x}/{y}.webp` | Individual terrain tiles |

All endpoints are at `https://api.maptiler.com/` with `?key=YOUR_MAPTILER_KEY`.

```js
// Forward geocoding
const response = await fetch(
  `https://api.maptiler.com/geocoding/${encodeURIComponent(query)}.json?key=YOUR_MAPTILER_KEY&limit=5`
);
const data = await response.json();
// data.features[0].geometry.coordinates → [lng, lat]
// data.features[0].place_name → "Prague, Czech Republic"
```

> Full API reference: `references/geocoding-and-services.md`
> Upstream platform reference: [`maptiler/maptiler-skills`](https://github.com/maptiler/maptiler-skills)

---

## 7. Critical Gotchas

| Problem | Fix |
|---------|-----|
| Map invisible | Container needs explicit height (`height: 100vh` or `position: absolute; inset: 0`) |
| Wrong location | Coordinates are `[lng, lat]` not `[lat, lng]` |
| "Style not loaded" error | Add layers inside `map.on('load', ...)` or check `map.isStyleLoaded()` |
| Layers vanish after `setStyle()` | Re-add custom layers in `map.once('styledata', ...)` |
| Data layers cover labels | Use `beforeId` parameter — detect label layers at runtime (see patterns-gotchas.md) |
| Duplicate source/layer errors | Remove before re-adding: `if (map.getLayer(id)) map.removeLayer(id)` |
| Slow with many points | Enable `cluster: true` on the GeoJSON source |
| Memory leaks in SPA | Always call `map.remove()` on unmount |
| Using `mapboxgl` namespace | Use `maplibregl` — this is MapLibre, not Mapbox! |
| Line gradient not working | Set `lineMetrics: true` on the GeoJSON source |
| `text-font` error | Use `'Noto Sans Regular'` — available in MapTiler styles |
| Terrain not showing | Add `raster-dem` source first, then call `map.setTerrain()` inside `load` event |

> All gotchas + reusable patterns: `references/patterns-gotchas.md`

---

## 8. Events

### Key Events

```js
// Wait for style + tiles to load (REQUIRED before adding layers)
map.on('load', () => {
  map.addSource(...);
  map.addLayer(...);
});

// Click on map
map.on('click', (e) => {
  console.log('Clicked at:', e.lngLat.lng, e.lngLat.lat);
});

// Click on specific layer
map.on('click', 'my-layer', (e) => {
  const feature = e.features[0];
  new maplibregl.Popup()
    .setLngLat(e.lngLat)
    .setHTML(`<b>${feature.properties.name}</b>`)
    .addTo(map);
});

// Hover effects
map.on('mouseenter', 'my-layer', () => {
  map.getCanvas().style.cursor = 'pointer';
});
map.on('mouseleave', 'my-layer', () => {
  map.getCanvas().style.cursor = '';
});

// Camera events
map.on('moveend', () => {
  console.log('Center:', map.getCenter());
  console.log('Zoom:', map.getZoom());
});
```

> Full events reference: `references/events.md`

---

## 9. Resources

- [MapLibre GL JS Documentation](https://maplibre.org/maplibre-gl-js/docs/)
- [MapLibre GL JS Examples](https://maplibre.org/maplibre-gl-js/docs/examples/)
- [MapTiler MapLibre Guide](https://docs.maptiler.com/maplibre-gl-js/)
- [MapTiler Cloud Console](https://cloud.maptiler.com/)
- [GitHub — MapLibre GL JS](https://github.com/maplibre/maplibre-gl-js)
- [NPM — maplibre-gl](https://www.npmjs.com/package/maplibre-gl)
- [react-map-gl (Visgl)](https://visgl.github.io/react-map-gl/)
- [Style Specification](https://maplibre.org/maplibre-style-spec/)
- [Expression Reference](https://maplibre.org/maplibre-style-spec/expressions/)

## Reference Files

- `references/INDEX.md` — Master topic index and navigation guide
- `references/api-classes-and-controls.md` — Core `maplibregl` classes, UI controls, camera methods, and coordinate types
- `references/basemaps-and-terrain.md` — All MapTiler v4 style.json URLs, raster tiles, and 3D terrain DEM
- `references/sources-layers.md` — Source types, layer types, and common patterns
- `references/expressions.md` — Expression syntax for data-driven styling
- `references/patterns-gotchas.md` — 12 gotchas + 12 reusable code patterns
- `references/events.md` — Lifecycle, camera, interaction, data events
- `references/geocoding-and-services.md` — MapTiler Cloud REST API usage with fetch()
- `references/frameworks.md` — React (react-map-gl), Vue, Svelte, Angular patterns
- `references/vector-tile-schemas.md` — Planet v4 source layers and attribute specifications