# MapLibre GL JS Official Task Examples Index 🧪🗺️

> The authoritative index of **30 atomic, copy-pasteable task implementations** for MapLibre GL JS, extracted directly from official MapLibre documentation and engineered with modern MapTiler Planet v4 vector and raster styles.

---

## 📑 Examples by Category

### 1. 🗺️ Core & Basemaps
* **[display-vector-map.md](display-vector-map.md)** — Basic vector map with MapTiler Streets v4, NavigationControl, and FullscreenControl.
* **[switch-map-styles.md](switch-map-styles.md)** — Runtime basemap style switcher toggling Streets, Outdoor, and Satellite.
* **[globe-projection.md](globe-projection.md)** — Interactive 3D globe view projection at low zoom levels.

### 2. 🚀 Camera & Navigation
* **[fly-to-camera.md](fly-to-camera.md)** — Cinematic camera flight navigation with pitch, bearing, and curve controls.
* **[animate-point-along-route.md](animate-point-along-route.md)** — Smooth 60 FPS marker interpolation along a GeoJSON line.
* **[camera-orbit-360.md](camera-orbit-360.md)** — Continuous 360° camera orbit animation around a target coordinate.
* **[scroll-driven-fly-to.md](scroll-driven-fly-to.md)** — Scrollytelling narrative chapter navigation driven by article scroll position.
* **[fit-bounds-padding.md](fit-bounds-padding.md)** — Fitting camera viewport bounds with asymmetric UI sidebar padding.

### 3. 🏔️ 3D Terrain, Buildings & Elevation
* **[3d-terrain-elevation.md](3d-terrain-elevation.md)** — Hardware-accelerated 3D DEM elevation using Terrain-RGB tiles.
* **[3d-buildings-extrusion.md](3d-buildings-extrusion.md)** — Vector building footprints extruded to 3D with height expressions.
* **[sun-lighting-and-shadows.md](sun-lighting-and-shadows.md)** — Dynamic directional sun lighting and cast shadows across 3D geometries.
* **[custom-layer-threejs.md](custom-layer-threejs.md)** — Custom WebGL layer embedding a 3D Three.js model with `MercatorCoordinate`.
* **[add-3d-model-globe.md](add-3d-model-globe.md)** — Rendering 3D GLTF meshes on the 3D globe projection.
* **[color-relief-layer.md](color-relief-layer.md)** — Dynamic elevation color relief hypsometric ramps on DEM tiles.

### 4. 🎨 Data & Vector Styling
* **[geojson-choropleth.md](geojson-choropleth.md)** — Demographic choropleth using data-driven style expressions (`step` / `interpolate`).
* **[hover-feature-state.md](hover-feature-state.md)** — 60 FPS polygon boundary hover highlights with `map.setFeatureState`.
* **[click-select-highlight.md](click-select-highlight.md)** — Persistent feature selection and highlighting across vector geometries.
* **[gradient-line.md](gradient-line.md)** — Multi-color gradient routes using `line-gradient` and `lineMetrics`.
* **[filter-features-slider.md](filter-features-slider.md)** — Real-time attribute filtering with range sliders via `map.setFilter`.
* **[pmtiles-protocol.md](pmtiles-protocol.md)** — Streaming serverless vector tiles using `maplibregl.addProtocol` and PMTiles.
* **[streaming-realtime-geojson.md](streaming-realtime-geojson.md)** — Live vehicle telemetry streaming and smooth source updates via `setData`.

### 5. 📍 Points, Clusters & UI
* **[marker-clustering.md](marker-clustering.md)** — Native GeoJSON source-level clustering, step styling, and click expansion.
* **[cluster-spiderfy.md](cluster-spiderfy.md)** — Point cluster expansion and spiderfy child layouts on click.
* **[custom-html-markers.md](custom-html-markers.md)** — DOM-based `maplibregl.Marker` with pulsing CSS radar beacons.
* **[spatial-query-inspector.md](spatial-query-inspector.md)** — Spatial feature inspection on click or drag using `queryRenderedFeatures`.
* **[heatmap-layer.md](heatmap-layer.md)** — Hardware-accelerated WebGL heatmap density layers.

### 6. 🪟 Raster, Canvas & Video Overlays
* **[swipe-between-maps.md](swipe-between-maps.md)** — Split-screen swipe comparison slider between vector and satellite basemaps.
* **[add-canvas-source.md](add-canvas-source.md)** — Procedural animated HTML5 Canvas raster layer projection.
* **[add-image-source-georeferenced.md](add-image-source-georeferenced.md)** — Georeferenced drone orthophoto or historical map image overlay.
* **[video-on-a-map.md](video-on-a-map.md)** — Draped video overlay projected onto coordinates.
