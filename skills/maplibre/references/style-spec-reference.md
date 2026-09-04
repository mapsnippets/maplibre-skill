# MapLibre Style Specification Reference 🎨

> The authoritative, exhaustive technical reference for the **MapLibre Style Specification (v8+)**, covering root properties, light, 3D terrain, sky, projections, and the complete layout & paint properties across all 9 layer types.

---

## 1. Root Properties Specification

A MapLibre style document is a JSON object defining the visual representation of spatial data rendered by WebGL:

```json
{
  "version": 8,
  "name": "MapTiler Modern Planet Style",
  "metadata": {
    "maputnik:renderer": "mbgljs"
  },
  "center": [8.5417, 47.3769],
  "zoom": 12,
  "bearing": 0,
  "pitch": 45,
  "light": {
    "anchor": "viewport",
    "color": "#ffffff",
    "intensity": 0.4,
    "position": [1.15, 210, 30]
  },
  "terrain": {
    "source": "maptiler-dem",
    "exaggeration": 1.2
  },
  "projection": {
    "type": "mercator"
  },
  "sky": {
    "sky-color": "#0084FF",
    "horizon-color": "#ffffff",
    "fog-color": "#0b1118",
    "fog-ground-blend": 0.6
  },
  "sprite": "https://api.maptiler.com/maps/streets-v4/sprite",
  "glyphs": "https://api.maptiler.com/fonts/{fontstack}/{range}.pbf?key={key}",
  "sources": {},
  "layers": []
}
```

### Root Fields Detail Table

| Property | Type | Required | Default | Description |
| :--- | :--- | :---: | :--- | :--- |
| **`version`** | `Number` | **Yes** | `8` | Specification version. Must strictly be `8`. |
| **`name`** | `String` | No | `""` | Human-readable title of the style. |
| **`metadata`** | `Object` | No | `{}` | Arbitrary application-specific or editor metadata. |
| **`center`** | `Array<Number>` | No | `[0, 0]` | Default initial map center `[longitude, latitude]`. |
| **`zoom`** | `Number` | No | `0` | Default initial zoom level (`0`–`24`). |
| **`bearing`** | `Number` | No | `0` | Default initial camera bearing in degrees clockwise from true north. |
| **`pitch`** | `Number` | No | `0` | Default initial camera pitch/tilt in degrees (`0`–`85`). |
| **`light`** | `Object` | No | `{}` | Global directional light source affecting 3D building extrusions. |
| **`terrain`** | `Object` | No | `null` | Digital Elevation Model configuration for hardware-accelerated 3D terrain. |
| **`projection`** | `Object` | No | `{"type": "mercator"}` | Projection geometry (`"mercator"` or `"globe"`). |
| **`sky`** | `Object` | No | `null` | Atmospheric sky dome and horizon distance fog. |
| **`sprite`** | `String` | No | `null` | Base URL URL template for sprite JSON and PNG icons. |
| **`glyphs`** | `String` | No | `null` | URL template for SDF vector font glyphs in `.pbf` format. |
| **`sources`** | `Object` | **Yes** | `{}` | Key-value dictionary of source IDs to source definition objects. |
| **`layers`** | `Array<Object>`| **Yes** | `[]` | Ordered array of style layers rendered from bottom to top. |

---

## 2. 3D Terrain, Sky, Light & Projection

### 3D Digital Elevation Model (`terrain`)
Activates 3D terrain rendering across the entire canvas using raster-dem elevation tiles:
```json
"terrain": {
  "source": "maptiler-dem",
  "exaggeration": 1.5
}
```
* **`source`** *(String, Required)*: ID of a valid `raster-dem` source in the style.
* **`exaggeration`** *(Number, Default `1.0`)*: Multiplier for vertical elevation height. Values > 1 amplify valleys and mountain peaks.

### Atmospheric Sky Dome & Fog (`sky`)
Renders realistic atmospheric perspective when pitching the camera towards the horizon:
```json
"sky": {
  "sky-color": "#0284c7",
  "sky-horizon-blend": 0.8,
  "horizon-color": "#e0f2fe",
  "horizon-fog-blend": 0.5,
  "fog-color": "#0b1118",
  "fog-ground-blend": 0.5,
  "atmosphere-blend": ["interpolate", ["linear"], ["zoom"], 4, 0.1, 8, 0.8]
}
```

### Directional Sun Light (`light`)
Defines the sun direction, color, and intensity for shading 3D extrusions (`fill-extrusion`):
```json
"light": {
  "anchor": "viewport",
  "color": "#ffffff",
  "intensity": 0.45,
  "position": [1.15, 210, 30]
}
```
* **`anchor`**: `"map"` (light position fixed relative to North) or `"viewport"` (light fixed relative to the user's camera).
* **`position`**: Spherical coordinates `[radial coordinate, azimuthal angle (0–360°), polar angle (0–180°)]`.

### Projection Engine (`projection`)
MapLibre GL JS (v4+) supports dynamic projected globe views:
```json
"projection": {
  "type": "globe"
}
```
* `"type": "mercator"`: Standard conformal Web Mercator (EPSG:3857).
* `"type": "globe"`: 3D interactive planetary sphere representation at zoom levels 0–6.

---

## 3. The 9 Layer Specifications in Detail

Every layer object conforms to this foundational schema:
```json
{
  "id": "unique-layer-identifier",
  "type": "fill | line | symbol | circle | heatmap | fill-extrusion | raster | hillshade | background",
  "source": "source-id",
  "source-layer": "vector-sublayer-name",
  "minzoom": 0,
  "maxzoom": 24,
  "filter": ["==", "class", "motorway"],
  "layout": {},
  "paint": {}
}
```

### 1. `background` Layer
Base color rendered underneath all layers. Does not require a `source`.
* **`paint`**:
  * `background-color` *(Color, Default `#000000`)*: Backdrop color.
  * `background-pattern` *(ResolvedImage)*: Sprite icon repeated as pattern.
  * `background-opacity` *(Number, 0–1, Default `1`)*: Opacity factor.

### 2. `fill` Layer (2D Polygons)
Renders filled geometric polygon and multipolygon boundaries (lakes, parks, landuse, countries).
* **`layout`**:
  * `visibility`: `"visible"` | `"none"`
  * `fill-sort-key`: Optional numeric sorting key.
* **`paint`**:
  * `fill-color` *(Color, Default `#000000`)*: Polygon interior color. Supports data-driven expressions.
  * `fill-opacity` *(Number, 0–1, Default `1`)*: Interior transparency.
  * `fill-outline-color` *(Color)*: 1px border stroke (rendered as geometry outline).
  * `fill-pattern` *(ResolvedImage)*: Name of sprite image repeated across polygon area.
  * `fill-antialias` *(Boolean, Default `true`)*: Enables anti-aliasing on boundary edges.
  * `fill-translate` *(Array<Number>, Default `[0, 0]`)*: Pixel offset `[x, y]`.
  * `fill-translate-anchor` *(Enum, Default `"map"`)*: `"map"` or `"viewport"`.

### 3. `line` Layer (Strokes & Polylines)
Renders roads, railways, waterways, boundaries, and routes.
* **`layout`**:
  * `line-cap` *(Enum, Default `"butt"`)*: `"butt"` | `"round"` | `"square"`.
  * `line-join` *(Enum, Default `"miter"`)*: `"bevel"` | `"round"` | `"miter"`.
  * `line-miter-limit` *(Number, Default `2`)*: Sharpness limit for miter joins.
  * `line-round-limit` *(Number, Default `1.05`)*: Simplification threshold.
  * `line-sort-key` *(Number)*: Custom rendering order.
* **`paint`**:
  * `line-color` *(Color, Default `#000000`)*: Stroke color.
  * `line-width` *(Number, Default `1`)*: Stroke thickness in device-independent pixels.
  * `line-opacity` *(Number, 0–1, Default `1`)*: Transparency factor.
  * `line-gap-width` *(Number, Default `0`)*: Gap drawn between two parallel stroke outlines (casing).
  * `line-offset` *(Number, Default `0`)*: Perpendicular offset from line geometry (positive = right, negative = left).
  * `line-blur` *(Number, Default `0`)*: Gaussian blur radius in pixels.
  * `line-dasharray` *(Array<Number>)*: Alternating dash and gap lengths expressed in multiples of `line-width`.
  * `line-gradient` *(Color Expression)*: Expression using `["line-progress"]` to paint multi-stop color ramps along the route (Requires `lineMetrics: true` on source).

### 4. `symbol` Layer (Text & Icons)
Renders text labels and sprite icons anchored to points or oriented along line strings.
* **`layout`**:
  * `symbol-placement` *(Enum, Default `"point"`)*: `"point"` | `"line"` | `"line-center"`.
  * `symbol-spacing` *(Number, Default `250`)*: Distance between repeated symbols along a line in pixels.
  * `symbol-avoid-edges` *(Boolean, Default `false`)*: Suppress symbols near tile boundaries.
  * `symbol-sort-key` *(Number)*: High values take rendering priority.
  * `symbol-z-order` *(Enum, Default `"auto"`)*: `"auto"` | `"viewport-y"` | `"source"`.
  * **Icon Layout Properties:**
    * `icon-image` *(ResolvedImage)*: Sprite icon ID (e.g. `"hospital-15"`).
    * `icon-size` *(Number, Default `1`)*: Icon scale factor.
    * `icon-anchor` *(Enum, Default `"center"`)*: `"center"` | `"top"` | `"bottom"` | `"left"` | `"right"` | `"top-left"` | `"top-right"` | `"bottom-left"` | `"bottom-right"`.
    * `icon-offset` *(Array<Number>, Default `[0, 0]`)*: Pixel offset `[x, y]`.
    * `icon-rotate` *(Number, Default `0`)*: Clockwise rotation in degrees.
    * `icon-allow-overlap` *(Boolean, Default `false`)*: If `true`, icon remains visible even if colliding with others.
    * `icon-ignore-placement` *(Boolean, Default `false`)*: If `true`, other symbols can be placed over this icon.
    * `icon-rotation-alignment` *(Enum, Default `"auto"`)*: `"map"` | `"viewport"` | `"auto"`.
    * `icon-pitch-alignment` *(Enum, Default `"auto"`)*: `"map"` | `"viewport"` | `"auto"`.
  * **Text Layout Properties:**
    * `text-field` *(FormattedString)*: Template string (e.g. `"{name:en}"`) or formatting expression.
    * `text-font` *(Array<String>)*: Font stack (e.g. `["Noto Sans Regular", "Open Sans Bold"]`).
    * `text-size` *(Number, Default `16`)*: Font size in pixels.
    * `text-max-width` *(Number, Default `10`)*: Max line wrap width in `ems`.
    * `text-line-height` *(Number, Default `1.2`)*: Line spacing multiplier.
    * `text-letter-spacing` *(Number, Default `0`)*: Kerning tracking in `ems`.
    * `text-justify` *(Enum, Default `"center"`)*: `"auto"` | `"left"` | `"center"` | `"right"`.
    * `text-anchor` *(Enum, Default `"center"`)*: Anchor point relative to position.
    * `text-variable-anchor` *(Array<Enum>)*: Priority list of dynamic fallback anchors `["top", "bottom", "left", "right"]`.
    * `text-radial-offset` *(Number, Default `0`)*: Distance in `ems` when variable anchor is active.
    * `text-allow-overlap` *(Boolean, Default `false`)*: Disables collision detection for text.
    * `text-transform` *(Enum, Default `"none"`)*: `"none"` | `"uppercase"` | `"lowercase"`.
* **`paint`**:
  * `icon-opacity` *(Number, 0–1, Default `1`)*: Icon transparency.
  * `icon-color` *(Color, Default `#000000`)*: Tint color for SDF icons.
  * `icon-halo-color` *(Color)*: Glow border color around icon.
  * `icon-halo-width` *(Number, Default `0`)*: Glow stroke thickness.
  * `text-color` *(Color, Default `#000000`)*: Typography text fill color.
  * `text-opacity` *(Number, 0–1, Default `1`)*: Typography opacity.
  * `text-halo-color` *(Color, Default `rgba(0,0,0,0)`)*: Typography halo glow color (crucial for legibility over satellite).
  * `text-halo-width` *(Number, Default `0`)*: Halo stroke thickness in pixels (recommended `1.5`–`2.0`).
  * `text-halo-blur` *(Number, Default `0`)*: Halo gaussian blur distance.

### 5. `circle` Layer (Vector Points)
Renders high-performance anti-aliased circles directly on the GPU. Ideal for IoT point data and clusters.
* **`paint`**:
  * `circle-radius` *(Number, Default `5`)*: Radius in screen pixels.
  * `circle-color` *(Color, Default `#000000`)*: Fill color. Supports `interpolate` or `step` expressions.
  * `circle-opacity` *(Number, 0–1, Default `1`)*: Fill opacity.
  * `circle-stroke-color` *(Color, Default `#000000`)*: Outer border color.
  * `circle-stroke-width` *(Number, Default `0`)*: Outer border width in pixels.
  * `circle-stroke-opacity` *(Number, 0–1, Default `1`)*: Stroke opacity.
  * `circle-blur` *(Number, Default `0`)*: 1.0 creates a soft radial blur.
  * `circle-pitch-alignment` *(Enum, Default `"viewport"`)*: `"map"` (flattens onto 3D terrain) or `"viewport"` (faces camera).
  * `circle-pitch-scale` *(Enum, Default `"map"`)*: `"map"` (shrinks as camera tilts away) or `"viewport"` (constant size).

### 6. `heatmap` Layer (Kernel Density Surfaces)
Renders continuous GPU density gradient surfaces dynamically aggregating point features.
* **`paint`**:
  * `heatmap-radius` *(Number, Default `30`)*: Gaussian kernel radius in pixels per point.
  * `heatmap-weight` *(Number, Default `1`)*: Feature point weighting attribute (e.g. earthquake magnitude).
  * `heatmap-intensity` *(Number, Default `1`)*: Multiplier adjusting heatmap brightness across zoom levels.
  * `heatmap-color` *(Color Expression)*: Continuous color ramp expression using `["heatmap-density"]`:
    ```json
    "heatmap-color": [
      "interpolate", ["linear"], ["heatmap-density"],
      0, "rgba(0, 210, 255, 0)",
      0.2, "rgb(0, 210, 255)",
      0.4, "rgb(0, 132, 255)",
      0.8, "rgb(255, 107, 0)",
      1.0, "rgb(239, 68, 68)"
    ]
    ```
  * `heatmap-opacity` *(Number, 0–1, Default `1`)*: Global surface transparency.

### 7. `fill-extrusion` Layer (3D Extruded Buildings)
Extrudes 2D building footprints into volumetric 3D polygonal prisms rendered with depth testing and directional lighting.
* **`paint`**:
  * `fill-extrusion-color` *(Color, Default `#000000`)*: Wall and roof base color.
  * `fill-extrusion-height` *(Number, Default `0`)*: Roof height in meters above terrain ground level.
  * `fill-extrusion-base` *(Number, Default `0`)*: Bottom base elevation in meters (for floating bridges or split floors).
  * `fill-extrusion-opacity` *(Number, 0–1, Default `1`)*: Wall transparency.
  * `fill-extrusion-pattern` *(ResolvedImage)*: Sprite texture mapped across walls.
  * `fill-extrusion-vertical-gradient` *(Boolean, Default `true`)*: Shades wall bottoms darker than tops for enhanced depth perception.

### 8. `raster` Layer (Satellite & Aerial Photography)
Renders raster tile sources (e.g. MapTiler Satellite v4).
* **`paint`**:
  * `raster-opacity` *(Number, 0–1, Default `1`)*: Image layer transparency.
  * `raster-contrast` *(Number, -1 to 1, Default `0`)*: Dynamic range contrast adjustment.
  * `raster-saturation` *(Number, -1 to 1, Default `0`)*: Color saturation boost/desaturation.
  * `raster-brightness-min` *(Number, 0–1, Default `0`)*: Minimum luminance threshold.
  * `raster-brightness-max` *(Number, 0–1, Default `1`)*: Maximum luminance threshold.
  * `raster-hue-rotate` *(Number, Default `0`)*: Hue rotation in degrees.
  * `raster-resampling` *(Enum, Default `"linear"`)*: `"linear"` (bilinear smoothing) or `"nearest"` (preserves raw pixel grid for DEM/discrete rasters).
  * `raster-fade-duration` *(Number, Default `300`)*: Cross-fade transition time between tile LODs in milliseconds.

### 9. `hillshade` Layer (Terrain Shading)
Calculates real-time shaded relief directly from a `raster-dem` source.
* **`source`**: Must be a source of type `raster-dem`.
* **`paint`**:
  * `hillshade-method` *(MapLibre v4+, Default `"standard"`)*: `"standard"` (single sun vector) or `"multidirectional"` (illuminates all slope aspects simultaneously).
  * `hillshade-illumination-direction` *(Number, 0–359, Default `315`)*: Sun azimuth angle in degrees clockwise from North.
  * `hillshade-illumination-anchor` *(Enum, Default `"viewport"`)*: `"map"` or `"viewport"`.
  * `hillshade-exaggeration` *(Number, 0–1, Default `0.5`)*: Relief slope contrast intensity.
  * `hillshade-shadow-color` *(Color, Default `#000000`)*: Tint color applied to shadowed slopes.
  * `hillshade-highlight-color` *(Color, Default `#ffffff`)*: Tint color applied to sun-facing slopes.
  * `hillshade-accent-color` *(Color, Default `#000000`)*: Tint color applied to high-angle ridges.

---

## 4. Official MapTiler Planet v4 Basemap Endpoints

MapLibre styles should always consume modern MapTiler Planet v4 endpoints:

| Style Variant | Style Specification URL Template |
| :--- | :--- |
| **Streets v4** | `https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_API_KEY` |
| **Outdoor v4** | `https://api.maptiler.com/maps/outdoor-v4/style.json?key=YOUR_MAPTILER_API_KEY` |
| **Satellite v4** | `https://api.maptiler.com/maps/satellite-v4/style.json?key=YOUR_MAPTILER_API_KEY` |
| **Dataviz v4 Dark** | `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=YOUR_MAPTILER_API_KEY` |
| **Dataviz v4 Light** | `https://api.maptiler.com/maps/dataviz-v4-light/style.json?key=YOUR_MAPTILER_API_KEY` |
| **Terrain-RGB DEM** | `https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_MAPTILER_API_KEY` |
| **Contours Vector** | `https://api.maptiler.com/tiles/contours/tiles.json?key=YOUR_MAPTILER_API_KEY` |
