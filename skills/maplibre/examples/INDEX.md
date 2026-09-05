# MapLibre GL JS Task Examples Index 🧪🗺️

> The authoritative index of **41 atomic, copy-pasteable task implementations** for MapLibre GL JS v6 (v6.7.0 ESM), curated from MapLibre documentation and community recipes and engineered with modern MapTiler Planet v4 basemap styles.

---

## 📑 Examples by Category

### 1. 🗺️ Core & Basemaps
* **[display-vector-map.md](display-vector-map.md)** — Vector map with Streets v4, NavigationControl, and FullscreenControl.
* **[switch-map-styles.md](switch-map-styles.md)** — Runtime basemap style switcher toggling Streets, Outdoor, Satellite, and Dataviz.
* **[globe-projection.md](globe-projection.md)** — Interactive 3D globe view projection at low zoom levels.

### 2. 🎥 Camera & Navigation
* **[fly-to-camera.md](fly-to-camera.md)** — Cinematic camera flights with pitch, bearing, and curve controls.
* **[animate-point-along-route.md](animate-point-along-route.md)** — Smooth 60 FPS marker interpolation along a route with Turf.js.
* **[camera-orbit-360.md](camera-orbit-360.md)** — Continuous 360° camera orbit animation around a coordinate.
* **[scroll-driven-fly-to.md](scroll-driven-fly-to.md)** — Scrollytelling narrative navigation driven by scroll position.
* **[fit-bounds-padding.md](fit-bounds-padding.md)** — Fitting camera viewport bounds with asymmetric UI sidebar padding.
* **[turf-distance-measurement.md](turf-distance-measurement.md)** — Interactive click-to-measure tool calculating geodesic path lengths with Turf.js.
* **[custom-geolocate-control.md](custom-geolocate-control.md)** — High-accuracy GPS geolocation with live heading tracker orientation.

### 3. 🏔️ 3D Terrain, Buildings & Elevation
* **[3d-terrain-elevation.md](3d-terrain-elevation.md)** — 3D DEM elevation using MapTiler Terrain-RGB tiles and dynamic hillshading.
* **[3d-buildings-extrusion.md](3d-buildings-extrusion.md)** — Extruded 3D buildings with height expressions.
* **[sun-lighting-and-shadows.md](sun-lighting-and-shadows.md)** — Directional sun lighting and cast shadows via `map.setLight`.
* **[multidirectional-hillshade.md](multidirectional-hillshade.md)** — Multidirectional hillshading with Terrain-RGB elevation.
* **[custom-layer-threejs.md](custom-layer-threejs.md)** — Custom WebGL layer embedding Three.js 3D models.
* **[threejs-3d-model-on-terrain.md](threejs-3d-model-on-terrain.md)** — Embedding Three.js 3D models accurately clamped to 3D DEM terrain.
* **[add-3d-model-globe.md](add-3d-model-globe.md)** — Rendering 3D GLTF meshes on the 3D globe projection.
* **[color-relief-layer.md](color-relief-layer.md)** — Dynamic elevation color relief hypsometric ramps on DEM.

### 4. 🎨 Data & Vector Styling
* **[geojson-choropleth.md](geojson-choropleth.md)** — Choropleth using data-driven expressions (`step` / `interpolate`).
* **[vector-contour-lines.md](vector-contour-lines.md)** — Vector contour elevation lines with dynamic line labels and intervals.
* **[hover-feature-state.md](hover-feature-state.md)** — 60 FPS polygon hover highlights with `map.setFeatureState`.
* **[click-select-highlight.md](click-select-highlight.md)** — Persistent feature selection and highlighting across vector geometries.
* **[add-layer-below-labels.md](add-layer-below-labels.md)** — Inserting custom vector layers below basemap labels ("Sandwich Pattern").
* **[multiple-geometries-one-source.md](multiple-geometries-one-source.md)** — Mixed points, lines, polygons in a single GeoJSON source with filter-based rendering.
* **[gradient-line.md](gradient-line.md)** — Multi-color gradient routes using `line-gradient` and `lineMetrics`.
* **[filter-features-slider.md](filter-features-slider.md)** — Real-time attribute filtering with range sliders via `map.setFilter`.
* **[satellite-hybrid-terrain.md](satellite-hybrid-terrain.md)** — MapTiler Satellite Hybrid tiles with 3D terrain elevation, hillshading, and vector labels.
* **[streaming-realtime-geojson.md](streaming-realtime-geojson.md)** — Live telemetry updates and smooth source data streaming.

### 5. 📍 Points, Clusters & UI
* **[marker-clustering.md](marker-clustering.md)** — Native GeoJSON source-level clustering, step styling, and click expansion.
* **[cluster-spiderfy.md](cluster-spiderfy.md)** — Point cluster expansion and spiderfy child layouts on click.
* **[custom-html-markers.md](custom-html-markers.md)** — DOM-based `maplibregl.Marker` with pulsing CSS radar beacons.
* **[pulsing-gpu-marker.md](pulsing-gpu-marker.md)** — Animated pulsating radar circle marker rendered with dynamic Canvas/GPU animation.
* **[spatial-query-inspector.md](spatial-query-inspector.md)** — Spatial feature inspection using `queryRenderedFeatures`.
* **[spatial-bbox-feature-filter.md](spatial-bbox-feature-filter.md)** — Interactive bounding box drag to query all rendered vector features.
* **[draw-polygon-geojson.md](draw-polygon-geojson.md)** — Interactive vector polygon digitization and GeoJSON export with Mapbox GL Draw.
* **[heatmap-layer.md](heatmap-layer.md)** — Hardware-accelerated WebGL heatmap density layers.

### 6. 🖼️ Raster, Canvas & Video Overlays
* **[wms-raster-source.md](wms-raster-source.md)** — Integrating enterprise OGC WMS layers as a raster source.
* **[swipe-between-maps.md](swipe-between-maps.md)** — Split-screen comparison slider between vector and satellite styles.
* **[add-canvas-source.md](add-canvas-source.md)** — Procedural animated HTML5 Canvas raster layer projection.
* **[add-image-source-georeferenced.md](add-image-source-georeferenced.md)** — Georeferenced drone orthophoto or historical map image overlay.
* **[video-on-a-map.md](video-on-a-map.md)** — Draped video overlay projected onto coordinates.
