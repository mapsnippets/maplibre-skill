# MapLibre Sources & Layers Technical Reference 📦

> Comprehensive architectural reference for all MapLibre GL JS source types (`vector`, `raster`, `raster-dem`, `geojson`, `image`, `video`), protocol streaming handlers, and layer ordering architectures.

---

## 1. Source Types & Configuration Dictionary

### A. Vector Tile Source (`vector`)
Delivers binary protocol buffer (`.pbf` / `.mvt`) vector tiles containing geometry slices and tabular attribute data.

```javascript
map.addSource('maptiler-planet', {
  type: 'vector',
  url: `https://api.maptiler.com/tiles/v4/tiles.json?key=${MAPTILER_KEY}`,
  promoteId: 'osm_id' // Uses specific property as unique feature ID for feature-state
});
```

* **Configuration Options Table:**

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **`url`** | `String` | `undefined` | TileJSON resource URL. |
| **`tiles`** | `Array<String>` | `undefined` | Array of tile URL templates containing `{z}`, `{x}`, and `{y}`. |
| **`minzoom`** | `Number` | `0` | Minimum zoom level for which tiles are requested. |
| **`maxzoom`** | `Number` | `22` | Maximum zoom level for which tiles are requested. |
| **`bounds`** | `Array<Number>` | `[-180,-85,180,85]` | Bounding box coordinates `[w, s, e, n]`. |
| **`scheme`** | `String` | `'xyz'` | Tile coordinate addressing (`'xyz'` or `'tms'`). |
| **`promoteId`**| `String \| Object` | `undefined` | Field name promoted to top-level feature ID (mandatory for feature-state when IDs are inside properties). |
| **`attribution`**| `String` | `undefined` | Copyright and attribution string. |

---

### B. GeoJSON Source (`geojson`)
Stores client-side GeoJSON feature collections with automatic GPU tile clustering and line metrics generation.

```javascript
map.addSource('earthquakes', {
  type: 'geojson',
  data: 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson',
  cluster: true,
  clusterRadius: 50,
  clusterMaxZoom: 14,
  clusterProperties: {
    max_mag: ['max', ['get', 'mag']],
    total_fatalities: ['+', ['coalesce', ['get', 'deaths'], 0]]
  },
  generateId: true,
  lineMetrics: false
});
```

* **Key GeoJSON Performance Options:**
  * **`cluster`** *(Boolean, Default `false`)*: Enables serverless Supercluster point aggregation.
  * **`clusterRadius`** *(Number, Default `50`)*: Radius in pixels within which points are grouped.
  * **`clusterMaxZoom`** *(Number, Default `maxzoom - 1`)*: Maximum zoom level on which clustering is performed.
  * **`clusterProperties`** *(Object)*: Custom aggregation expressions accumulated across clustered points.
  * **`generateId`** *(Boolean, Default `false`)*: Automatically generates sequential numeric IDs on features lacking IDs (vital for `map.setFeatureState`).
  * **`lineMetrics`** *(Boolean, Default `false`)*: Computes progress metrics along lines (MANDATORY for `line-gradient`).
  * **`tolerance`** *(Number, Default `0.375`)*: Douglas-Peucker simplification tolerance in pixels.

---

### C. Raster-DEM Source (`raster-dem`)
Delivers RGB-encoded elevation terrain models used for 3D terrain meshing and hillshading.

```javascript
map.addSource('terrain-rgb', {
  type: 'raster-dem',
  url: `https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=${MAPTILER_KEY}`,
  tileSize: 512,
  encoding: 'mapbox'
});
```

* **`encoding`**:
  * `'mapbox'`: Mapbox / MapTiler RGB formula:
    $$	ext{Elevation (m)} = -10000 + ((R 	imes 256 	imes 256 + G 	imes 256 + B) 	imes 0.1)$$
  * `'terrarium'`: AWS Terrarium RGB formula:
    $$	ext{Elevation (m)} = (R 	imes 256 + G + B / 256) - 32768$$

---

### D. Image Source (`image`)
Drapes a georeferenced static image (drone orthophoto, historical map, blueprint) across coordinates:

```javascript
map.addSource('orthophoto', {
  type: 'image',
  url: 'https://images.example.com/site-plan.png',
  coordinates: [
    [-122.465, 37.800], // Top-Left [lng, lat]
    [-122.420, 37.800], // Top-Right [lng, lat]
    [-122.420, 37.770], // Bottom-Right [lng, lat]
    [-122.465, 37.770]  // Bottom-Left [lng, lat]
  ]
});
```

---

## 2. The "Sandwich Pattern" (Layer Ordering Architecture)

When inserting custom polygon or heatmap layers into a map, placing them at the top of the stack obscures road numbers, street labels, and city names.

The **Sandwich Pattern** inspects the style layer hierarchy and inserts custom vector layers immediately underneath the first text label layer:

```javascript
function addLayerBelowLabels(map, layerConfig) {
  const layers = map.getStyle().layers;
  let firstLabelId = undefined;

  for (const layer of layers) {
    if (layer.type === 'symbol') {
      firstLabelId = layer.id;
      break;
    }
  }

  // Inserts layer right beneath the first label!
  map.addLayer(layerConfig, firstLabelId);
}
```

---

## 3. Preserving Custom Layers Across Style Swaps

When changing basemap styles via `map.setStyle()`, MapLibre destroys all user-added sources and layers.

To switch basemaps seamlessly while preserving custom datasets:

```javascript
function switchBasemapPreservingLayers(map, newStyleUrl) {
  // 1. Snapshot user custom sources & layers
  const customSources = {};
  const customLayers = [];

  const style = map.getStyle();
  for (const layer of style.layers) {
    if (layer.id.startsWith('user-')) {
      customLayers.push(layer);
      if (!customSources[layer.source]) {
        customSources[layer.source] = map.getSource(layer.source).serialize();
      }
    }
  }

  // 2. Listen for the new style load
  map.once('style.load', () => {
    // Re-add custom sources
    for (const [id, config] of Object.entries(customSources)) {
      if (!map.getSource(id)) {
        map.addSource(id, config);
      }
    }
    // Re-add custom layers
    for (const layer of customLayers) {
      if (!map.getLayer(layer.id)) {
        map.addLayer(layer);
      }
    }
  });

  // 3. Apply new style
  map.setStyle(newStyleUrl);
}
```
