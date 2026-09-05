# Basemaps, Styles & 3D Terrain Reference (Planet v4)

This reference provides the production-ready style JSON URLs, raster XYZ tile endpoints, and 3D Terrain-RGB configuration.

---

## 1. Vector Map Styles (`style.json`)

Use these vector styles with MapLibre GL JS (`maplibregl.Map`):

| Style Name | Style URL | Recommended Use Case |
| :--- | :--- | :--- |
| **Streets v4** | `https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY` | General purpose navigation, city maps, POIs |
| **Streets v4 Dark** | `https://api.maptiler.com/maps/streets-v4-dark/style.json?key=YOUR_API_KEY` | Night mode, high-contrast dark theme |
| **Streets v4 Pastel** | `https://api.maptiler.com/maps/streets-v4-pastel/style.json?key=YOUR_API_KEY` | Vintage / soft pastel streets theme |
| **Outdoor v4** | `https://api.maptiler.com/maps/outdoor-v4/style.json?key=YOUR_API_KEY` | Hiking, cycling, topographic contours & hillshading |
| **Outdoor v4 Dark** | `https://api.maptiler.com/maps/outdoor-v4-dark/style.json?key=YOUR_API_KEY` | Night mode trails and terrain |
| **Satellite v4** | `https://api.maptiler.com/maps/satellite-v4/style.json?key=YOUR_API_KEY` | High-resolution satellite imagery |
| **Satellite Hybrid v4** | `https://api.maptiler.com/maps/hybrid-v4/style.json?key=YOUR_API_KEY` | High-resolution satellite imagery with streets & labels |
| **Dataviz v4 Dark** | `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=YOUR_API_KEY` | Minimalist contrast theme optimized for dense data overlays |
| **Dataviz v4 Light** | `https://api.maptiler.com/maps/dataviz-v4-light/style.json?key=YOUR_API_KEY` | Clean light background for analytical data overlays |
| **Dataviz v4** | `https://api.maptiler.com/maps/dataviz-v4/style.json?key=YOUR_API_KEY` | Balanced neutral backdrop for data visualizations |
| **Topo v4** | `https://api.maptiler.com/maps/topo-v4/style.json?key=YOUR_API_KEY` | Traditional topographic cartography with contours |
| **Base v4** | `https://api.maptiler.com/maps/base-v4/style.json?key=YOUR_API_KEY` | Clean muted background for custom thematic layers |
| **Base v4 Dark** | `https://api.maptiler.com/maps/base-v4-dark/style.json?key=YOUR_API_KEY` | Dark muted background for custom thematic layers |
| **Base v4 Light** | `https://api.maptiler.com/maps/base-v4-light/style.json?key=YOUR_API_KEY` | Light minimalist background for custom thematic layers |
| **Bright v4** | `https://api.maptiler.com/maps/bright-v4/style.json?key=YOUR_API_KEY` | Vibrant, colorful presentation style |
| **Winter v4** | `https://api.maptiler.com/maps/winter-v4/style.json?key=YOUR_API_KEY` | Ski slopes, winter pistes, and snow terrain |
| **Ocean** | `https://api.maptiler.com/maps/ocean/style.json?key=YOUR_API_KEY` | Nautical bathymetry and oceanic features |

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

---

## 4. Specialized MapTiler Vector Data Tilesets

Beyond standard street and satellite basemaps, MapTiler Cloud delivers dedicated vector data tilesets for thematic styling, regional analysis, and administrative overlays:

| Tileset Name | TileJSON Endpoint | Layers & Content | Use Cases |
| :--- | :--- | :--- | :--- |
| **MapTiler Countries** | `https://api.maptiler.com/tiles/countries/tiles.json?key=YOUR_API_KEY` | `administrative` (level 0 sovereign countries, level 1 subdivisions/states), `postal` | Thematic choropleth maps, national boundary overlays, regional demographics |
| **MapTiler Contours** | `https://api.maptiler.com/tiles/contours/tiles.json?key=YOUR_API_KEY` | `contour` (elevation isolines, index contours, height values in meters) | Topographic hiking overlays, terrain contour analysis |
| **MapTiler Cadastre** | `https://api.maptiler.com/tiles/cadastre/tiles.json?key=YOUR_API_KEY` | `parcel`, `zoning` (official property boundaries, cadastral identifiers) | Real estate, land registry, parcel boundary inspection |

