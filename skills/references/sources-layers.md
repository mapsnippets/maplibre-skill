# MapLibre GL JS — Sources & Layers Reference

Complete reference for source types, layer types, and common source/layer patterns.

> [Style Specification](https://maplibre.org/maplibre-style-spec/) · [Sources](https://maplibre.org/maplibre-style-spec/sources/) · [Layers](https://maplibre.org/maplibre-style-spec/layers/)

---

## Source Types

### GeoJSON Source

The most common source type. Accepts inline data or a URL.

```js
map.addSource('my-data', {
  type: 'geojson',
  data: {
    type: 'FeatureCollection',
    features: [...]
  },
  // Optional clustering
  cluster: true,
  clusterMaxZoom: 14,
  clusterRadius: 50,
  clusterProperties: {
    'sum': ['+', ['get', 'value']]  // aggregate properties
  },
  // Required for line-gradient
  lineMetrics: true,
  // Generate IDs for feature-state
  generateId: true
});

// Update data dynamically
map.getSource('my-data').setData(newGeoJSON);
```

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `data` | object/string | — | GeoJSON object or URL |
| `cluster` | boolean | false | Enable clustering |
| `clusterMaxZoom` | number | — | Max zoom for clustering |
| `clusterRadius` | number | 50 | Cluster radius in pixels |
| `clusterMinPoints` | number | 2 | Min points to form cluster |
| `clusterProperties` | object | — | Aggregate expressions |
| `lineMetrics` | boolean | false | Required for line-gradient |
| `generateId` | boolean | false | Auto-generate feature IDs |
| `maxzoom` | number | 18 | Max zoom for source tiles |
| `tolerance` | number | 0.375 | Simplification tolerance |
| `buffer` | number | 128 | Tile buffer in pixels |

### Vector Source

Server-side vector tiles (MapTiler styles include these automatically).

> **Note:** MapTiler v2 styles use `maptiler_planet` as the vector source name. Older styles used `openmaptiles`. When adding layers that reference the style's built-in vector source, detect the source name at runtime (see Fill-Extrusion example below).

```js
map.addSource('my-vector', {
  type: 'vector',
  url: 'https://api.maptiler.com/tiles/v3/tiles.json?key=YOUR_MAPTILER_KEY'
});

// Access specific source-layer
map.addLayer({
  id: 'buildings',
  source: 'my-vector',
  'source-layer': 'building',  // layer within the vector tile
  type: 'fill',
  paint: { 'fill-color': '#ccc' }
});
```

### Raster Source

For raster tile imagery.

```js
map.addSource('satellite', {
  type: 'raster',
  url: 'https://api.maptiler.com/tiles/satellite-v2/tiles.json?key=YOUR_MAPTILER_KEY',
  tileSize: 256
});

map.addLayer({
  id: 'satellite-layer',
  type: 'raster',
  source: 'satellite'
});
```

### Raster-DEM Source

For elevation/terrain data.

```js
map.addSource('terrain', {
  type: 'raster-dem',
  url: 'https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_MAPTILER_KEY',
  tileSize: 256
});

// Enable 3D terrain
map.setTerrain({ source: 'terrain', exaggeration: 1.5 });

// Hillshade layer
map.addLayer({
  id: 'hillshade',
  type: 'hillshade',
  source: 'terrain',
  paint: {
    'hillshade-exaggeration': 0.5,
    'hillshade-shadow-color': '#473B24'
  }
});
```

### Image Source

For georeferenced images.

```js
map.addSource('overlay', {
  type: 'image',
  url: 'https://example.com/overlay.png',
  coordinates: [
    [14.35, 50.15],  // top-left [lng, lat]
    [14.50, 50.15],  // top-right
    [14.50, 50.05],  // bottom-right
    [14.35, 50.05]   // bottom-left
  ]
});

map.addLayer({
  id: 'overlay-layer',
  type: 'raster',
  source: 'overlay',
  paint: { 'raster-opacity': 0.7 }
});
```

### Video Source

```js
map.addSource('video', {
  type: 'video',
  urls: ['video.mp4', 'video.webm'],
  coordinates: [
    [14.35, 50.15], [14.50, 50.15],
    [14.50, 50.05], [14.35, 50.05]
  ]
});
```

---

## Layer Types

### Circle Layer

For point features rendered as circles.

```js
map.addLayer({
  id: 'points',
  type: 'circle',
  source: 'my-source',
  paint: {
    'circle-radius': 8,
    'circle-color': '#0891b2',
    'circle-opacity': 0.8,
    'circle-stroke-width': 2,
    'circle-stroke-color': '#ffffff',
    'circle-blur': 0,
    'circle-translate': [0, 0]     // pixel offset [x, y]
  }
});
```

### Line Layer

For LineString and Polygon outlines.

```js
map.addLayer({
  id: 'route',
  type: 'line',
  source: 'route-source',
  layout: {
    'line-join': 'round',     // 'bevel', 'round', 'miter'
    'line-cap': 'round',      // 'butt', 'round', 'square'
    'line-sort-key': 0
  },
  paint: {
    'line-color': '#0066FF',
    'line-width': 4,
    'line-opacity': 1,
    'line-dasharray': [2, 4],  // dash pattern
    'line-gap-width': 0,       // gap for casing lines
    'line-blur': 0,
    'line-gradient': [          // requires lineMetrics: true on source
      'interpolate', ['linear'], ['line-progress'],
      0, 'blue', 0.5, 'green', 1, 'red'
    ]
  }
});
```

### Fill Layer

For polygons.

```js
map.addLayer({
  id: 'regions',
  type: 'fill',
  source: 'regions-source',
  paint: {
    'fill-color': '#FF0000',
    'fill-opacity': 0.5,
    'fill-outline-color': '#000000',
    'fill-antialias': true,
    'fill-pattern': 'pattern-name'  // from sprite sheet
  }
});  // Optionally pass a beforeId to insert below labels (detect label layer at runtime)
```

### Fill-Extrusion Layer

For 3D extruded polygons (buildings, etc.). MapTiler v2 styles use `maptiler_planet` as the vector source; older styles used `openmaptiles`. Detect at runtime:

```js
// Detect the vector source name from the current style
const sources = map.getStyle().sources;
let vectorSource = null;
for (const [name, src] of Object.entries(sources)) {
  if (src.type === 'vector') { vectorSource = name; break; }
}

if (vectorSource) {
  map.addLayer({
    id: '3d-buildings',
    source: vectorSource,
    'source-layer': 'building',
    type: 'fill-extrusion',
    minzoom: 14,
    paint: {
      'fill-extrusion-color': '#aaa',
      'fill-extrusion-height': ['get', 'render_height'],
      'fill-extrusion-base': ['get', 'render_min_height'],
      'fill-extrusion-opacity': 0.6,
      'fill-extrusion-vertical-gradient': true
    }
  });
}
```

### Symbol Layer

For text labels and icons.

```js
map.addLayer({
  id: 'labels',
  type: 'symbol',
  source: 'my-source',
  layout: {
    // Text
    'text-field': ['get', 'name'],
    'text-font': ['Noto Sans Regular'],
    'text-size': 14,
    'text-anchor': 'top',           // 'center', 'left', 'right', 'top', 'bottom'
    'text-offset': [0, 0.8],
    'text-max-width': 10,
    'text-allow-overlap': false,
    // Icon
    'icon-image': 'marker-15',      // from sprite sheet
    'icon-size': 1.5,
    'icon-anchor': 'bottom',
    'icon-allow-overlap': false,
    // Collision
    'symbol-placement': 'point',    // 'point', 'line', 'line-center'
    'symbol-sort-key': ['get', 'priority']
  },
  paint: {
    'text-color': '#333333',
    'text-halo-color': '#ffffff',
    'text-halo-width': 1,
    'icon-opacity': 1
  }
});
```

### Heatmap Layer

For density visualization.

```js
map.addLayer({
  id: 'heatmap',
  type: 'heatmap',
  source: 'points-source',
  paint: {
    'heatmap-weight': ['get', 'intensity'],    // or expression
    'heatmap-intensity': 1,
    'heatmap-color': [
      'interpolate', ['linear'], ['heatmap-density'],
      0, 'rgba(0,0,255,0)', 0.2, 'royalblue',
      0.4, 'cyan', 0.6, 'lime',
      0.8, 'yellow', 1, 'red'
    ],
    'heatmap-radius': 30,
    'heatmap-opacity': 0.8
  }
});
```

### Raster Layer

For raster tile sources.

```js
map.addLayer({
  id: 'satellite',
  type: 'raster',
  source: 'satellite-source',
  paint: {
    'raster-opacity': 1,
    'raster-brightness-min': 0,
    'raster-brightness-max': 1,
    'raster-contrast': 0,
    'raster-saturation': 0,
    'raster-hue-rotate': 0,
    'raster-fade-duration': 300
  }
});
```

### Hillshade Layer

For terrain shading from raster-dem sources.

```js
map.addLayer({
  id: 'hillshade',
  type: 'hillshade',
  source: 'terrain-dem',
  paint: {
    'hillshade-exaggeration': 0.5,
    'hillshade-illumination-direction': 335,
    'hillshade-illumination-anchor': 'viewport',
    'hillshade-shadow-color': '#473B24',
    'hillshade-highlight-color': '#ffffff',
    'hillshade-accent-color': '#000000'
  }
});
```

### Sky Layer

For atmospheric sky rendering (with 3D terrain).

```js
map.addLayer({
  id: 'sky',
  type: 'sky',
  paint: {
    'sky-type': 'atmosphere',         // 'gradient' or 'atmosphere'
    'sky-atmosphere-sun': [0, 90],
    'sky-atmosphere-sun-intensity': 15,
    'sky-atmosphere-color': 'rgba(135, 206, 235, 1)',
    'sky-opacity': 1
  }
});
```

---

## Layer Ordering

Layers are rendered in array order — last layer is on top. Use `beforeId` to control ordering.

**Important:** Label layer IDs vary between MapTiler styles. Do not hardcode a specific label layer name. Detect available label layers at runtime:

```js
// Find the first label layer in the current style
const firstLabel = map.getStyle().layers.find(
  l => l.type === 'symbol' && /label/.test(l.id)
);

// Insert your layer below labels
map.addLayer({ id: 'my-fill', ... }, firstLabel ? firstLabel.id : undefined);
```

### Move and Remove

```js
map.moveLayer('my-layer', 'before-this-layer');
map.removeLayer('my-layer');
map.removeSource('my-source');
```

---

## Source/Layer Management Patterns

### Safe Add (avoid duplicates)

```js
function safeAddLayer(id, sourceConfig, layerConfig) {
  if (map.getLayer(id)) map.removeLayer(id);
  if (map.getSource(id)) map.removeSource(id);

  map.addSource(id, sourceConfig);
  map.addLayer({ id, source: id, ...layerConfig });
}
```

### Update GeoJSON Data

```js
const source = map.getSource('my-source');
if (source) {
  source.setData(newGeoJSON);
}
```

### Toggle Layer Visibility

```js
function toggleLayer(layerId, visible) {
  map.setLayoutProperty(layerId, 'visibility', visible ? 'visible' : 'none');
}
```

### Change Paint Property at Runtime

```js
map.setPaintProperty('my-layer', 'circle-color', '#FF0000');
map.setPaintProperty('my-layer', 'circle-radius', 12);
```

### Filter Layer Features

```js
// Show only features where type === 'restaurant'
map.setFilter('poi-layer', ['==', ['get', 'type'], 'restaurant']);

// Clear filter
map.setFilter('poi-layer', null);
```
