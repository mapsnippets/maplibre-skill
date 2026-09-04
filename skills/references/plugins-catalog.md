# MapLibre GL JS Plugins Catalog 🔌

> The comprehensive catalog of third-party plugins, controls, layer extensions, utility libraries, and framework integrations for **MapLibre GL JS v6.7.0**.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Geocoding & Search Plugins

| Plugin | Package / Repository | Description & Best Practice |
| :--- | :--- | :--- |
| **MapTiler Geocoding Control** *(Recommended)* | `@maptiler/geocoding-control` | Official MapTiler search and reverse-geocoding control. Features fast predictive autocomplete, POI categorization, fuzzy matching, and bbox filtering. Seamlessly integrates as an `IControl`. |
| **MapLibre GL Geocoder** | `@maplibre/maplibre-gl-geocoder` | Standard geocoder control for MapLibre GL JS. Compatible with Pelias, Nominatim, and custom geocoding API backends. |
| **Nominatim Geocoder** | `maplibre-gl-nominatim` | Lightweight geocoder control directly querying OpenStreetMap Nominatim API. |
| **Algolia Places** | `places.js` | Fast address autocomplete integration for search bars. |

### MapTiler Geocoding Control Integration
```html
<script src="https://cdn.maptiler.com/maptiler-geocoding-control/v1.3.0/maplibregl.umd.js"></script>
<link href="https://cdn.maptiler.com/maptiler-geocoding-control/v1.3.0/style.css" rel="stylesheet" />

<script>
  const gc = new maptilergeocoding.GeocodingControl({
    apiKey: 'YOUR_MAPTILER_API_KEY'
  });
  map.addControl(gc, 'top-left');
</script>
```

---

## 2. User Interface & Map Controls

| Plugin | Package / Repository | Description |
| :--- | :--- | :--- |
| **MapLibre GL Inspect** | `maplibre-gl-inspect` | Visual debugging tool for inspecting vector tile features, attributes, and source-layer boundaries in real time. |
| **MapLibre GL Export** | `@watergis/maplibre-gl-export` | High-DPI map export tool enabling users to export current map views to PDF, PNG, and SVG formats. |
| **MapLibre GL Opacity** | `maplibre-gl-opacity` | Layer opacity sliders and visibility toggle control for comparative analysis. |
| **MapLibre GL Compare** | `@maplibre/maplibre-gl-compare` | Side-by-side or split-screen swipe map comparison slider between two synchronized map instances. |
| **MapLibre GL Basemapper** | `maplibre-gl-basemapper` | Interactive basemap switcher control for cycling through multiple vector and satellite styles. |
| **MapLibre GL Directions** | `@maplibre/maplibre-gl-directions` | Routing, turn-by-turn navigation, and waypoint management control. |
| **MapLibre GL Ruler / Measure** | `maplibre-gl-measures` | Interactive distance and area measurement tool with snapping support. |
| **MapLibre GL Legend** | `@watergis/maplibre-gl-legend` | Dynamic map legend generated automatically from active layer style definitions. |
| **MapLibre GL Fullscreen** | Native `maplibregl.FullscreenControl` | Built-in fullscreen toggle control. |
| **MapLibre GL Scale** | Native `maplibregl.ScaleControl` | Built-in dynamic metric / imperial distance scale indicator. |

---

## 3. Layer Types & 3D Visualizations

| Plugin | Package / Repository | Description |
| :--- | :--- | :--- |
| **MapLibre Contour** | `maplibre-contour` | Client-side contour line and elevation isoline generation from Terrain-RGB DEM tiles. |
| **Three.js Custom Layer** | `three` + `CustomLayerInterface` | Full 3D rendering pipeline for glTF/GLB models, ambient shadows, animations, and custom shaders synchronized with camera view matrices. |
| **PMTiles Protocol** | `pmtiles` | Serverless single-file archive format for vector and raster tiles. Enables zero-backend global map hosting from S3 or Cloudflare R2. |
| **COG Protocol (Cloud Optimized GeoTIFF)** | `@geotiff/geotiff` / `cog-protocol` | Directly stream and decode tiled GeoTIFF raster imagery into MapLibre raster sources without tile servers. |
| **MapLibre ArcGIS Tiled Service** | `maplibre-gl-arcgis-tiled-map-service` | Bridge for displaying Esri ArcGIS REST tiled map services directly inside MapLibre. |
| **Babylon.js Integration** | `babylonjs` | Alternative 3D game engine integration rendering photorealistic 3D assets on top of MapLibre coordinates. |

---

## 4. Drawing & Geometry Editing Tools

| Plugin | Package / Repository | Description |
| :--- | :--- | :--- |
| **Terra Draw** *(Modern Standard)* | `terra-draw` | Modern, framework-agnostic vector drawing library supporting points, lines, polygons, circles, rectangles, freehand drawing, and vertex editing. |
| **Mapbox GL Draw** | `@mapbox/mapbox-gl-draw` | Classic drawing plugin for creating and editing geometry features. Note: requires MapLibre compatibility aliases. |
| **Turf.js** | `@turf/turf` | Essential client-side spatial analysis library (buffers, unions, intersections, convex hulls, point-in-polygon, length, area). |

### Terra Draw Quickstart
```javascript
import { TerraDraw, TerraDrawMapLibreGLAdapter, TerraDrawPolygonMode, TerraDrawPointMode } from 'terra-draw';

const draw = new TerraDraw({
  adapter: new TerraDrawMapLibreGLAdapter({ map }),
  modes: [new TerraDrawPointMode(), new TerraDrawPolygonMode()]
});
draw.start();
draw.setMode('polygon');
```

---

## 5. Development & Performance Tools

| Plugin | Package / Repository | Description |
| :--- | :--- | :--- |
| **MapLibre Performance Metrics** | Native `map.showTileBoundaries`, `map.showCollisionBoxes` | Built-in debugging flags for visualizing tile request boundaries, collision trees, and overdraw. |
| **Supercluster** | `supercluster` | High-speed geospatial point clustering engine used internally by GeoJSON sources and available for custom clustering pipelines. |
| **Earcut** | `earcut` | Fastest polygon triangulation library used for extrusions and custom WebGL geometries. |

---

## 6. Framework Integrations

| Framework | Library / Wrapper | Description |
| :--- | :--- | :--- |
| **React** | `react-map-gl/maplibre` | The industry-standard React wrapper. Provides `<Map>`, `<Source>`, `<Layer>`, `<Marker>`, and `<Popup>` with declarative hooks (`useMap`). |
| **React (Alternative)** | `@vis.gl/react-maplibre` | Modern lightweight React wrapper maintained by the vis.gl ecosystem. |
| **Vue.js** | `vue-maplibre-gl` / `v-mapbox` | Reactive Vue 3 composition-api components for MapLibre maps and layers. |
| **Svelte** | `svelte-maplibre` | Declarative Svelte 4/5 components (`<MapLibre>`, `<GeoJSON>`, `<FillLayer>`). |
| **Angular** | `ngx-maplibre-gl` | Native Angular module with directives and observable bindings. |

---

## 7. Protocol Extensions (`maplibregl.addProtocol`)

MapLibre supports custom protocol schemes using `maplibregl.addProtocol(customScheme, callback)`:

```javascript
import { Protocol } from 'pmtiles';

// Register pmtiles:// protocol globally
const protocol = new Protocol();
maplibregl.addProtocol('pmtiles', protocol.tile);

// Now load PMTiles anywhere in styles or sources
map.addSource('my-pmtiles-source', {
  type: 'vector',
  url: 'pmtiles://https://r2-bucket.example.com/planet.pmtiles'
});
```
