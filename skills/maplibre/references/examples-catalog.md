# MapLibre GL JS Recipe Catalog & API Cross-Reference 📚🛠️

> An encyclopedic technical directory connecting every MapLibre GL JS API capability, class, and shader expression directly to its verified, production-grade standalone recipe in `skills/examples/`. Engineered with modern MapTiler Planet v4 styles.

---

## 1. Core Map Initialization & Global Basemaps

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Vector Map Quickstart** | `maplibregl.Map`, `NavigationControl`, `FullscreenControl`, MapTiler `streets-v4`. | [`display-vector-map.md`](../examples/display-vector-map.md) |
| **Dynamic Style Switcher** | Runtime basemap switching with `map.setStyle()` and custom UI buttons. | [`switch-map-styles.md`](../examples/switch-map-styles.md) |
| **Interactive 3D Globe** | WebGL spherical projection via `map.setProjection({ type: 'globe' })`. | [`globe-projection.md`](../examples/globe-projection.md) |

```javascript
// Minimal Vector Map Initialization
const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: [8.5417, 47.3769],
  zoom: 12
});
```

---

## 2. Camera Navigation & Temporal Animation

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Cinematic FlyTo Flights** | `map.flyTo()`, bearing, pitch, flight curve arcs, and speed throttling. | [`fly-to-camera.md`](../examples/fly-to-camera.md) |
| **Route Marker Interpolation** | 60 FPS marker interpolation along a GeoJSON line string using Turf.js. | [`animate-point-along-route.md`](../examples/animate-point-along-route.md) |
| **Continuous 360° Orbit** | Smooth rotational camera animation using `map.rotateTo()` in a RAF loop. | [`camera-orbit-360.md`](../examples/camera-orbit-360.md) |
| **Scrollytelling FlyTo** | Scroll-driven story chapters linked to camera viewport transitions. | [`scroll-driven-fly-to.md`](../examples/scroll-driven-fly-to.md) |
| **Asymmetric Sidebar Padding** | Viewport bounds fitting with asymmetric offsets via `map.fitBounds()`. | [`fit-bounds-padding.md`](../examples/fit-bounds-padding.md) |
| **Interactive Path Ruler** | Click-to-measure geodesic path distance tool built with Turf.js. | [`turf-distance-measurement.md`](../examples/turf-distance-measurement.md) |
| **High-Accuracy Geolocation** | `maplibregl.GeolocateControl` with continuous tracking and device heading. | [`custom-geolocate-control.md`](../examples/custom-geolocate-control.md) |

---

## 3. 3D Terrain, Building Extrusions & Atmosphere

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **3D DEM Terrain-RGB** | `map.setTerrain()`, `raster-dem` source, dynamic hillshading. | [`3d-terrain-elevation.md`](../examples/3d-terrain-elevation.md) |
| **3D Building Extrusions** | Volumetric `fill-extrusion` layers, `render_height`, directional lighting. | [`3d-buildings-extrusion.md`](../examples/3d-buildings-extrusion.md) |
| **Directional Sunlight** | `map.setLight()` with spherical azimuth and polar coordinate vectors. | [`sun-lighting-and-shadows.md`](../examples/sun-lighting-and-shadows.md) |
| **Multidirectional Hillshade** | Multi-vector slope illumination using `hillshade-method: 'multidirectional'`. | [`multidirectional-hillshade.md`](../examples/multidirectional-hillshade.md) |
| **Three.js Custom WebGL Layer** | Embedding Three.js 3D models using `MercatorCoordinate` projection. | [`custom-layer-threejs.md`](../examples/custom-layer-threejs.md) |
| **Three.js Clamped to Terrain** | Anchoring 3D models accurately to 3D elevation with `queryTerrainElevation`. | [`threejs-3d-model-on-terrain.md`](../examples/threejs-3d-model-on-terrain.md) |
| **3D Models on Globe** | Rendering Three.js GLTF models on the 3D globe projection. | [`add-3d-model-globe.md`](../examples/add-3d-model-globe.md) |
| **Color Relief Hypsometric Ramp**| Elevation color ramps on DEM rasters using custom expressions. | [`color-relief-layer.md`](../examples/color-relief-layer.md) |

---

## 4. Vector Styling, Shader Expressions & Serverless Streaming

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Data-Driven Choropleth** | GPU polygon styling using `interpolate` and `step` expressions. | [`geojson-choropleth.md`](../examples/geojson-choropleth.md) |
| **Vector Elevation Contours** | MapTiler Contours tileset with dynamic line labels and interval styling. | [`vector-contour-lines.md`](../examples/vector-contour-lines.md) |
| **60 FPS GPU Hover Effect** | Dynamic polygon boundary highlight via `map.setFeatureState()`. | [`hover-feature-state.md`](../examples/hover-feature-state.md) |
| **Persistent Feature Select** | Click selection toggling feature states across vector geometries. | [`click-select-highlight.md`](../examples/click-select-highlight.md) |
| **Layer Below Labels (Sandwich)**| Layer hierarchy inspection to keep street text on top of vector fills. | [`add-layer-below-labels.md`](../examples/add-layer-below-labels.md) |
| **Multi-Geometry Single Source**| Mixed points, lines, and polygons in one GeoJSON source with `$type` filters. | [`multiple-geometries-one-source.md`](../examples/multiple-geometries-one-source.md) |
| **Multi-Stop Route Gradient** | Continuous color ramps on line geometry via `line-gradient` and `lineMetrics`. | [`gradient-line.md`](../examples/gradient-line.md) |
| **Real-Time Attribute Sliders** | Client-side attribute filtering with range sliders via `map.setFilter()`. | [`filter-features-slider.md`](../examples/filter-features-slider.md) |
| **Serverless PMTiles Streaming** | Custom protocol streaming of archive vector tiles with `addProtocol`. | [`pmtiles-protocol.md`](../examples/pmtiles-protocol.md) |
| **Live Telemetry Streaming** | Continuous polling and smooth source updates with `source.setData()`. | [`streaming-realtime-geojson.md`](../examples/streaming-realtime-geojson.md) |

---

## 5. Points, Clustering, Heatmaps & Spatial Queries

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **Native Point Clustering** | GeoJSON source-level clustering, step styling, and cluster expansion. | [`marker-clustering.md`](../examples/marker-clustering.md) |
| **Cluster Spiderfy Layout** | Point cluster expansion and spiderfy child layouts on click. | [`cluster-spiderfy.md`](../examples/cluster-spiderfy.md) |
| **Custom HTML Radar Markers** | DOM-based `maplibregl.Marker` with pulsing CSS radar beacons. | [`custom-html-markers.md`](../examples/custom-html-markers.md) |
| **GPU Pulsing Radar Beacon** | 60 FPS animated pulsating icon rendered dynamically on the GPU via Canvas. | [`pulsing-gpu-marker.md`](../examples/pulsing-gpu-marker.md) |
| **Point Spatial Feature Inspector** | Spatial click queries on vector layers via `queryRenderedFeatures`. | [`spatial-query-inspector.md`](../examples/spatial-query-inspector.md) |
| **Bounding Box Drag Filter** | Rubber-band bounding box selection querying all vector features inside. | [`spatial-bbox-feature-filter.md`](../examples/spatial-bbox-feature-filter.md) |
| **Hardware Heatmap Layer** | WebGL kernel density surfaces with `heatmap-density` ramps. | [`heatmap-layer.md`](../examples/heatmap-layer.md) |

---

## 6. Raster, Canvas & Video Overlays

| Task & Architecture | Primary APIs & Techniques | Production Recipe |
| :--- | :--- | :--- |
| **OGC WMS Raster Integration** | Direct integration of enterprise WMS radar/weather layers as raster sources. | [`wms-raster-source.md`](../examples/wms-raster-source.md) |
| **Split-Screen Map Swipe** | Synchronized side-by-side layer comparison slider with clipping masks. | [`swipe-between-maps.md`](../examples/swipe-between-maps.md) |
| **Animated Canvas Layer** | Draping procedural HTML5 Canvas animations directly onto coordinates. | [`add-canvas-source.md`](../examples/add-canvas-source.md) |
| **Georeferenced Orthophoto** | Projecting drone aerial orthophotos onto coordinates using `image` sources. | [`add-image-source-georeferenced.md`](../examples/add-image-source-georeferenced.md) |
| **Georeferenced Video Overlay** | Projecting live or looped MP4 video streams directly onto coordinates. | [`video-on-a-map.md`](../examples/video-on-a-map.md) |
