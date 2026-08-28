# Basemaps, Styles & 3D Terrain Reference (Planet v4)

This reference provides the production-ready style JSON URLs, raster XYZ tile endpoints, and 3D Terrain-RGB configuration.

---

## 1. Vector Map Styles (`style.json`)

Use these vector styles with MapLibre GL JS (`maplibregl.Map`), Leaflet (`L.maplibreGL`), or OpenLayers (`ol-mapbox-style`):

| Style Name | Style URL | Recommended Use Case |
| :--- | :--- | :--- |
| **Streets v4** | `https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY` | General purpose navigation, city maps, POIs |
| **Streets v4 Dark** | `https://api.maptiler.com/maps/streets-v4-dark/style.json?key=YOUR_API_KEY` | Night mode, data visualizations, glowing overlays |
| **Outdoor v4** | `https://api.maptiler.com/maps/outdoor-v4/style.json?key=YOUR_API_KEY` | Hiking, cycling, topographic contours & hillshading |
| **Satellite v4** | `https://api.maptiler.com/maps/satellite-v4/style.json?key=YOUR_API_KEY` | High-resolution satellite imagery with subtle labels |
| **Dataviz v4 Dark** | `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=YOUR_API_KEY` | Minimalist contrast theme optimized for dense data layers |
| **Topo v4** | `https://api.maptiler.com/maps/topo-v4/style.json?key=YOUR_API_KEY` | Traditional topographic cartography |
| **Base v4** | `https://api.maptiler.com/maps/base-v4/style.json?key=YOUR_API_KEY` | Clean muted background for custom thematic layers |

---

## 2. High-DPI Raster Tiles (512×512)

Use these raster tile endpoints for Leaflet (`L.tileLayer`) or OpenLayers (`ol/source/XYZ`):

### Streets v4 (PNG):
```text
https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY
```

### Satellite v4 (JPG):
```text
https://api.maptiler.com/maps/satellite-v4/{z}/{x}/{y}.jpg?key=YOUR_API_KEY
```

### Outdoor v4 (PNG):
```text
https://api.maptiler.com/maps/outdoor-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY
```

* **Configuration Settings:**
  * Tile size: `512`
  * Zoom offset: `-1`
  * Max zoom: `22`

---

## 3. 3D Terrain & DEM (Terrain-RGB)

MapTiler provides global elevation data encoded in Terrain-RGB format (Red, Green, Blue bytes representing elevation via the equation `height = -10000 + ((R * 256 * 256 + G * 256 + B) * 0.1)`).

* **TileJSON Endpoint:**
```text
https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_API_KEY
```

### MapLibre GL JS Native 3D Terrain Configuration:
```javascript
map.on("load", () => {
  map.addSource("terrain", {
    type: "raster-dem",
    url: "https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_API_KEY",
    tileSize: 512
  });
  map.setTerrain({ source: "terrain", exaggeration: 1.5 });
});
```
