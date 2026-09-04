# MapLibre GL JS Executable Examples 🚀

Production-grade, standalone HTML examples demonstrating core architectural features of **MapLibre GL JS (v4+)** with **MapTiler Cloud** vector basemaps, 3D terrain, data-driven styling, clustering, and custom streaming protocols.

---

## 📁 Examples Inventory

| File | Feature Demonstration | Key APIs & Techniques |
| :--- | :--- | :--- |
| [`01_basic_vector_map.html`](01_basic_vector_map.html) | Modern MapLibre Initialization & Controls | `maplibregl.Map`, MapTiler `streets-v4`, `NavigationControl`, `FullscreenControl`, `ScaleControl`, markers, popups. |
| [`02_3d_terrain_and_buildings.html`](02_3d_terrain_and_buildings.html) | 3D Elevation & Building Extrusions | `map.setTerrain()`, MapTiler `terrain-rgb-v2`, `hillshade` layer, `fill-extrusion` layer with height interpolation. |
| [`03_geojson_data_driven_styling.html`](03_geojson_data_driven_styling.html) | Dynamic Expressions & Feature States | Data-driven color ramps (`interpolate`), GPU hover effects (`map.setFeatureState`), dynamic tooltips and legends. |
| [`04_marker_clustering_and_popups.html`](04_marker_clustering_and_popups.html) | Native GeoJSON Point Clustering | `cluster: true`, step-based cluster circle sizes/colors, count labels, cluster expansion zoom on click, popups. |
| [`05_custom_protocol_pmtiles.html`](05_custom_protocol_pmtiles.html) | Custom Protocol Handlers & PMTiles | `maplibregl.addProtocol()`, PMTiles single-file streaming, custom GeoJSON protocol resolvers. |

---

## 🛠️ How to Run

1. Serve the files using any local HTTP server:
   ```bash
   npx serve .
   # or Python
   python -m http.server 8080
   ```
2. Pass your MapTiler API Key in the query string:
   ```text
   http://localhost:8080/01_basic_vector_map.html?key=YOUR_MAPTILER_API_KEY
   ```
