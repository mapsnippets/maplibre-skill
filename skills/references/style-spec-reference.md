# MapLibre Style Specification Reference 🎨

> The definitive technical reference for the **MapLibre Style Specification (v8)**, covering root properties, sources, all 9 layer types, expressions, 3D terrain, sky, and projections.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Root Properties

A MapLibre style document is a JSON object defining the visual appearance of a map:

```json
{
  "version": 8,
  "name": "MapTiler Custom Vector Style",
  "metadata": { "maputnik:renderer": "mbgljs" },
  "center": [14.42076, 50.08804],
  "zoom": 12,
  "bearing": 0,
  "pitch": 45,
  "light": { "anchor": "viewport", "color": "#ffffff", "intensity": 0.4 },
  "terrain": { "source": "maptiler-terrain", "exaggeration": 1.5 },
  "projection": { "type": "mercator" },
  "sprite": "https://api.maptiler.com/maps/streets-v4/sprite",
  "glyphs": "https://api.maptiler.com/fonts/{fontstack}/{range}.pbf?key={key}",
  "sources": {},
  "layers": []
}
```

### Key Root Fields
* **`version`** *(Number, Required)*: Must be `8`.
* **`name`** *(String)*: Human-readable name of the style.
* **`sources`** *(Object, Required)*: Map of source IDs to source definition objects.
* **`layers`** *(Array, Required)*: Ordered list of style layer objects. Rendered bottom-to-top.
* **`sprite`** *(String)*: URL template for the style's sprite JSON and PNG images.
* **`glyphs`** *(String)*: URL template for loading Signed Distance Field (SDF) glyph sets in PBF format: `https://api.maptiler.com/fonts/{fontstack}/{range}.pbf?key=YOUR_KEY`.
* **`terrain`** *(Object)*: Enables 3D digital elevation model terrain across the map:
  ```json
  "terrain": { "source": "maptiler-dem", "exaggeration": 1.2 }
  ```
* **`projection`** *(Object, MapLibre v4+)*: Map projection configuration.
  * `"type": "mercator"` — Standard Web Mercator (default).
  * `"type": "globe"` — 3D interactive spherical globe at low zoom levels.
* **`sky`** *(Object, MapLibre v4+)*: Atmospheric sky rendering and fog styling:
  ```json
  "sky": {
    "sky-color": "#0084FF",
    "horizon-color": "#ffffff",
    "fog-color": "#1e293b",
    "fog-ground-blend": 0.5
  }
  ```

---

## 2. Source Specifications

### A. Vector Tile Source (`vector`)
```json
"maptiler-planet": {
  "type": "vector",
  "url": "https://api.maptiler.com/tiles/v4/tiles.json?key=YOUR_KEY"
}
```
* Or via direct `tiles` array:
```json
"custom-vector": {
  "type": "vector",
  "tiles": ["https://tiles.example.com/{z}/{x}/{y}.pbf"],
  "minzoom": 0,
  "maxzoom": 14
}
```

### B. Raster-DEM (3D Elevation Source)
```json
"maptiler-terrain": {
  "type": "raster-dem",
  "url": "https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_KEY",
  "tileSize": 256,
  "encoding": "mapbox"
}
```
* **`encoding`**: `"mapbox"` (Mapbox/MapTiler RGB formula: `elevation = -10000 + ((R * 256 * 256 + G * 256 + B) * 0.1)`) or `"terrarium"`.

### C. GeoJSON Source (`geojson`)
```json
"earthquakes": {
  "type": "geojson",
  "data": "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson",
  "cluster": true,
  "clusterMaxZoom": 14,
  "clusterRadius": 50,
  "generateId": true
}
```

### D. Raster Tile Source (`raster`)
```json
"satellite-tiles": {
  "type": "raster",
  "url": "https://api.maptiler.com/tiles/satellite-v4/tiles.json?key=YOUR_KEY",
  "tileSize": 512
}
```

---

## 3. The 9 Layer Types & Properties

Every layer requires:
```json
{
  "id": "unique-layer-id",
  "type": "fill | line | symbol | circle | heatmap | fill-extrusion | raster | hillshade | background",
  "source": "source-id",
  "source-layer": "name-of-vector-sublayer",
  "minzoom": 0,
  "maxzoom": 24,
  "filter": ["==", "$type", "Polygon"],
  "layout": {},
  "paint": {}
}
```

### 1. `fill` (2D Polygons)
* **`paint`**:
  * `fill-color`: Color or expression.
  * `fill-opacity`: Number between `0` and `1`.
  * `fill-outline-color`: Color for polygon boundary stroke.
  * `fill-pattern`: Name of an image from the sprite.

### 2. `line` (Polylines & Outlines)
* **`layout`**:
  * `line-cap`: `"butt"` | `"round"` | `"square"`
  * `line-join`: `"bevel"` | `"round"` | `"miter"`
* **`paint`**:
  * `line-color`: Color or expression.
  * `line-width`: Stroke thickness in pixels.
  * `line-dasharray`: Array of dash/gap lengths `[2, 4]`.
  * `line-gradient`: Expression evaluating to color ramp along line length.

### 3. `symbol` (Text Labels & Icons)
* **`layout`**:
  * `icon-image`: Sprite icon name or formatted expression.
  * `icon-size`: Scale factor (e.g. `1.0`).
  * `icon-allow-overlap`: Boolean (prevent hiding on collision).
  * `text-field`: String template `"{name:en}"` or formatted expression.
  * `text-font`: Array of font names `["Open Sans Semibold", "Arial Unicode MS Bold"]`.
  * `text-size`: Font size in points/pixels.
  * `text-anchor`: `"center"` | `"top"` | `"bottom"` | `"left"` | `"right"`
  * `text-variable-anchor`: Array of fallback anchors `["top", "bottom", "left", "right"]`.
* **`paint`**:
  * `text-color`: Color.
  * `text-halo-color`: Glow/outline color around text.
  * `text-halo-width`: Glow thickness in pixels (recommended: `1.5`–`2.0`).

### 4. `circle` (Vector Point Circles)
* **`paint`**:
  * `circle-radius`: Circle radius in pixels (supports data-driven expressions).
  * `circle-color`: Fill color.
  * `circle-stroke-color`: Stroke color.
  * `circle-stroke-width`: Stroke width in pixels.
  * `circle-opacity`: Opacity.

### 5. `heatmap` (Kernel Density Gradient Surfaces)
* **`paint`**:
  * `heatmap-weight`: Expression measuring point intensity (default `1`).
  * `heatmap-intensity`: Multiplier controlling hotspot magnitude across zoom levels.
  * `heatmap-color`: Color ramp expression using `["interpolate", ["linear"], ["heatmap-density"], 0, "rgba(...)", 1, "rgb(...)"]`.
  * `heatmap-radius`: Kernel radius in pixels per point.
  * `heatmap-opacity`: Surface transparency.

### 6. `fill-extrusion` (3D Extruded Buildings & Volumes)
* **`paint`**:
  * `fill-extrusion-color`: Building wall color.
  * `fill-extrusion-height`: Height in meters (from property e.g. `["get", "render_height"]`).
  * `fill-extrusion-base`: Base elevation in meters (from property e.g. `["get", "render_min_height"]`).
  * `fill-extrusion-opacity`: Wall opacity.

### 7. `raster` (Satellite Imagery / Scanned Maps)
* **`paint`**:
  * `raster-opacity`: Layer transparency.
  * `raster-contrast`: Contrast adjustment (`-1` to `1`).
  * `raster-saturation`: Saturation (`-1` to `1`).
  * `raster-resampling`: `"linear"` (smooth) or `"nearest"` (crisp pixels/DEM).

### 8. `hillshade` (Digital Elevation Shading)
* **`source`**: Must point to a `raster-dem` source.
* **`paint`**:
  * `hillshade-illumination-direction`: Sun angle in degrees (`0` to `359`, default `315`).
  * `hillshade-exaggeration`: Relief intensity multiplier (`0` to `1`).
  * `hillshade-shadow-color`: Shaded slope color.
  * `hillshade-highlight-color`: Lit slope color.

### 9. `background` (Canvas Backdrop)
* **`paint`**:
  * `background-color`: Base color rendered beneath all layers (e.g. ocean or space).

---

## 4. MapLibre Expressions DSL

Expressions allow dynamic, data-driven, and zoom-dependent styling directly in WebGL shaders.

### 1. Data Retrieval
* `["get", "property_name"]` — Read attribute from feature.
* `["has", "property_name"]` — Test attribute existence.

### 2. Zoom & Camera Interpolation
```json
"circle-radius": [
  "interpolate", ["linear"], ["zoom"],
  5, 2,
  12, 8,
  16, 20
]
```

### 3. Step Functions (Discontinuous Ramps)
```json
"circle-color": [
  "step", ["get", "point_count"],
  "#00D2FF",   // Default (count < 10)
  10, "#0084FF", // 10 <= count < 50
  50, "#FF6B00"  // count >= 50
]
```

### 4. Decision Logic (`case` & `match`)
```json
"line-color": [
  "match", ["get", "class"],
  "motorway", "#FF3B30",
  "primary", "#FF9500",
  "secondary", "#FFCC00",
  "#FFFFFF" // Fallback default
]
```

### 5. String & Formatting Expressions
```json
"text-field": [
  "format",
  ["get", "name"], { "font-scale": 1.2, "text-color": "#000000" },
  "\n", {},
  ["get", "population"], { "font-scale": 0.8, "text-color": "#666666" }
]
```

---

## 5. MapTiler Recommended Basemap Style URLs

Always use official MapTiler Cloud vector styles with standard API key parameters:

| Style | Style URL Template |
| :--- | :--- |
| **Streets v4** | `https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_KEY` |
| **Dataviz v4 Dark** | `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=YOUR_KEY` |
| **Dataviz v4 Light** | `https://api.maptiler.com/maps/dataviz-v4-light/style.json?key=YOUR_KEY` |
| **Outdoor v4** | `https://api.maptiler.com/maps/outdoor-v4/style.json?key=YOUR_KEY` |
| **Satellite v4** | `https://api.maptiler.com/maps/satellite-v4/style.json?key=YOUR_KEY` |
| **Satellite Hybrid v4** | `https://api.maptiler.com/maps/hybrid-v4/style.json?key=YOUR_KEY` |
| **Base v4** | `https://api.maptiler.com/maps/base-v4/style.json?key=YOUR_KEY` |
| **Topo v4** | `https://api.maptiler.com/maps/topo-v4/style.json?key=YOUR_KEY` |

> Reference: For full style catalog and options, see [basemaps-and-terrain.md](basemaps-and-terrain.md).
