# MapLibre GL JS + MapTiler — Common Patterns & Gotchas

Quick reference for solving common issues and implementing standard patterns.

---

## Gotchas

### 1. Map Container Must Have Dimensions

**Problem:** Map shows as blank/empty.

**Solution:** The container element must have explicit width and height.

```css
/* Option 1: Fill viewport */
#map { position: absolute; inset: 0; }

/* Option 2: Fixed size */
#map { width: 800px; height: 600px; }

/* Option 3: Full viewport height */
#map { width: 100%; height: 100vh; }
```

### 2. Coordinates Are [lng, lat] Not [lat, lng]

**Problem:** Map shows wrong location or markers appear in the ocean.

**Solution:** MapLibre uses `[longitude, latitude]` order — same as GeoJSON.

```js
// WRONG (Google Maps / Leaflet order)
center: [50.1167, 14.4178]

// CORRECT (MapLibre / GeoJSON order)
center: [14.4178, 50.1167]   // [lng, lat] — Prague
```

This applies everywhere: `setLngLat()`, `flyTo()`, `fitBounds()`, GeoJSON coordinates, marker positions.

### 3. "Style not loaded" Errors

**Problem:** Adding layers before map is ready throws errors.

**Solution:** Wait for `load` event or check `isStyleLoaded()`.

```js
// Option 1: Wait for load (recommended)
map.on('load', () => {
  map.addSource(...);
  map.addLayer(...);
});

// Option 2: Guard check
function addLayerSafe(config) {
  if (map.isStyleLoaded()) {
    map.addLayer(config);
  } else {
    map.once('load', () => map.addLayer(config));
  }
}
```

### 4. Layers Disappear After Style Change

**Problem:** Custom layers vanish when calling `setStyle()`.

**Solution:** Re-add layers after the new style loads.

```js
map.setStyle('https://api.maptiler.com/maps/satellite/style.json?key=YOUR_MAPTILER_KEY');

map.once('styledata', () => {
  addMyCustomLayers();
});
```

### 5. Duplicate Source/Layer Errors

**Problem:** "Source/Layer already exists" when re-adding.

**Solution:** Remove before adding.

```js
function safeAddSource(id, config) {
  if (map.getSource(id)) map.removeSource(id);
  map.addSource(id, config);
}

function safeAddLayer(id, config) {
  if (map.getLayer(id)) map.removeLayer(id);
  // Must also remove source if re-creating
  map.addLayer({ id, ...config });
}
```

### 6. Data Layers Cover Labels

**Problem:** Fill layers or lines render on top of place names and road labels.

**Solution:** Use the `beforeId` parameter to insert below labels. However, **label layer IDs vary between MapTiler styles** — not all styles have `'waterway-label'` or other specific label layers. Always detect available label layers at runtime rather than hardcoding.

```js
// WRONG: polygon covers all labels
map.addLayer({
  id: 'my-polygon',
  type: 'fill',
  source: 'my-source',
  paint: { 'fill-color': '#ff0000', 'fill-opacity': 0.5 }
});

// SAFE: detect first label layer and insert before it
const layers = map.getStyle().layers;
const firstLabelLayer = layers.find(l => l.type === 'symbol' && /label/.test(l.id));
map.addLayer({
  id: 'my-polygon',
  type: 'fill',
  source: 'my-source',
  paint: { 'fill-color': '#ff0000', 'fill-opacity': 0.5 }
}, firstLabelLayer ? firstLabelLayer.id : undefined);
```

**Tip:** To see what label layers a style has, run: `map.getStyle().layers.filter(l => l.id.includes('label')).map(l => l.id)`

### 7. Line Gradient Not Working

**Problem:** `line-gradient` paint property has no effect.

**Solution:** Set `lineMetrics: true` on the GeoJSON source. This is required for MapLibre to compute line-progress values.

```js
map.addSource('route', {
  type: 'geojson',
  lineMetrics: true,   // REQUIRED for line-gradient
  data: routeGeoJSON
});
```

### 8. text-font Errors

**Problem:** `'text-font': ['Arial']` causes rendering errors.

**Solution:** Use fonts available in the MapTiler style. Most MapTiler styles include:

```js
'text-font': ['Noto Sans Regular']   // safe default
'text-font': ['Noto Sans Bold']
'text-font': ['Noto Sans Italic']
```

### 9. Memory Leaks in SPAs (React/Vue/Angular)

**Problem:** App slows down after navigating between pages with maps.

**Solution:** Always call `map.remove()` when component unmounts.

```js
// React
useEffect(() => {
  const map = new maplibregl.Map({ ... });
  return () => map.remove();  // CRITICAL
}, []);

// Vue
onUnmounted(() => { map?.remove(); map = null; });
```

### 10. Using mapboxgl Instead of maplibregl

**Problem:** Code uses `mapboxgl.Map()` or `mapboxgl.Marker()`.

**Solution:** MapLibre is a separate library. Always use the `maplibregl` namespace.

```js
// WRONG
const map = new mapboxgl.Map({ ... });

// CORRECT
const map = new maplibregl.Map({ ... });
```

### 11. Terrain Not Showing

**Problem:** `setTerrain()` called but map remains flat.

**Solution:** Add the raster-dem source first, and call `setTerrain()` inside the `load` event.

```js
map.on('load', () => {
  map.addSource('terrain', {
    type: 'raster-dem',
    url: 'https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_MAPTILER_KEY',
    tileSize: 256
  });

  map.setTerrain({ source: 'terrain', exaggeration: 1.5 });
});
```

Also ensure `pitch > 0` to see the 3D effect.

### 12. Feature State Not Working

**Problem:** `setFeatureState()` / `getFeatureState()` returns nothing.

**Solution:** Features need IDs. Either your GeoJSON features must have `id` properties, or enable `generateId: true` on the source.

```js
map.addSource('points', {
  type: 'geojson',
  data: geojson,
  generateId: true   // auto-assigns numeric IDs
});
```

---

## Common Patterns

### Pattern: Popup on Layer Click

```js
map.on('click', 'my-layer', (e) => {
  const feature = e.features[0];
  const coords = feature.geometry.coordinates.slice();

  // Adjust for antimeridian wrapping
  while (Math.abs(e.lngLat.lng - coords[0]) > 180) {
    coords[0] += e.lngLat.lng > coords[0] ? 360 : -360;
  }

  new maplibregl.Popup()
    .setLngLat(coords)
    .setHTML(`<h3>${feature.properties.name}</h3>`)
    .addTo(map);
});
```

### Pattern: Hover Effect with Feature State

```js
let hoveredId = null;

map.on('mousemove', 'my-layer', (e) => {
  if (e.features.length > 0) {
    if (hoveredId !== null) {
      map.setFeatureState({ source: 'my-source', id: hoveredId }, { hover: false });
    }
    hoveredId = e.features[0].id;
    map.setFeatureState({ source: 'my-source', id: hoveredId }, { hover: true });
  }
});

map.on('mouseleave', 'my-layer', () => {
  if (hoveredId !== null) {
    map.setFeatureState({ source: 'my-source', id: hoveredId }, { hover: false });
    hoveredId = null;
  }
});

// In layer paint — respond to feature state
paint: {
  'fill-color': [
    'case',
    ['boolean', ['feature-state', 'hover'], false],
    '#ff0000',
    '#0000ff'
  ]
}
```

### Pattern: Hover Cursor Change

```js
map.on('mouseenter', 'my-layer', () => {
  map.getCanvas().style.cursor = 'pointer';
});

map.on('mouseleave', 'my-layer', () => {
  map.getCanvas().style.cursor = '';
});
```

### Pattern: Layer Visibility Toggle

```js
function toggleLayer(layerId, visible) {
  if (map.getLayer(layerId)) {
    map.setLayoutProperty(layerId, 'visibility', visible ? 'visible' : 'none');
  }
}
```

### Pattern: Update GeoJSON Data

```js
function updateSourceData(sourceId, newData) {
  const source = map.getSource(sourceId);
  if (source) {
    source.setData(newData);
  }
}
```

### Pattern: Fit Map to GeoJSON Bounds

```js
function fitToGeoJSON(geojson) {
  const bounds = new maplibregl.LngLatBounds();

  geojson.features.forEach(feature => {
    if (feature.geometry.type === 'Point') {
      bounds.extend(feature.geometry.coordinates);
    } else if (feature.geometry.type === 'LineString') {
      feature.geometry.coordinates.forEach(coord => bounds.extend(coord));
    } else if (feature.geometry.type === 'Polygon') {
      feature.geometry.coordinates[0].forEach(coord => bounds.extend(coord));
    }
  });

  map.fitBounds(bounds, { padding: 50 });
}
```

### Pattern: Geocoding + Fly To + Marker

```js
async function searchAndFlyTo(query) {
  const response = await fetch(
    `https://api.maptiler.com/geocoding/${encodeURIComponent(query)}.json?key=YOUR_MAPTILER_KEY&limit=1`
  );
  const data = await response.json();
  if (data.features.length === 0) return;

  const coords = data.features[0].geometry.coordinates;

  map.flyTo({ center: coords, zoom: 14, essential: true });

  new maplibregl.Marker({ color: '#0891b2' })
    .setLngLat(coords)
    .setPopup(new maplibregl.Popup().setHTML(`<b>${data.features[0].place_name}</b>`))
    .addTo(map);
}
```

### Pattern: Debounced Move Handler

```js
let moveTimeout;

map.on('moveend', () => {
  clearTimeout(moveTimeout);
  moveTimeout = setTimeout(() => {
    const center = map.getCenter();
    const zoom = map.getZoom();
    loadDataForView(center, zoom);
  }, 200);
});
```

### Pattern: Save/Restore Map State

```js
function saveMapState() {
  return {
    center: map.getCenter().toArray(),
    zoom: map.getZoom(),
    pitch: map.getPitch(),
    bearing: map.getBearing()
  };
}

function restoreMapState(state) {
  map.jumpTo({
    center: state.center,
    zoom: state.zoom,
    pitch: state.pitch,
    bearing: state.bearing
  });
}

localStorage.setItem('mapState', JSON.stringify(saveMapState()));
const saved = localStorage.getItem('mapState');
if (saved) restoreMapState(JSON.parse(saved));
```

### Pattern: Resize Handler

```js
// Call when container size changes
const resizeObserver = new ResizeObserver(() => {
  map.resize();
});
resizeObserver.observe(document.getElementById('map'));
```

### Pattern: Query Features at Point

```js
map.on('click', (e) => {
  // All features at click point
  const allFeatures = map.queryRenderedFeatures(e.point);

  // Features from specific layer
  const layerFeatures = map.queryRenderedFeatures(e.point, {
    layers: ['my-layer']
  });

  console.log('Features:', layerFeatures);
});
```

### Pattern: Add Image to Map (for icon-image)

```js
map.loadImage('https://example.com/icon.png', (error, image) => {
  if (error) throw error;
  map.addImage('my-icon', image);

  map.addLayer({
    id: 'icons',
    type: 'symbol',
    source: 'my-source',
    layout: {
      'icon-image': 'my-icon',
      'icon-size': 0.5
    }
  });
});
```

---

## Debugging Tips

### Log All Map Events

```js
['load', 'styledata', 'error', 'click', 'moveend'].forEach(event => {
  map.on(event, (e) => console.log(`Event: ${event}`, e));
});
```

### Check What Layers Exist

```js
console.log('Layers:', map.getStyle().layers.map(l => l.id));
```

### Check What Sources Exist

```js
console.log('Sources:', Object.keys(map.getStyle().sources));
```

### Inspect Features at Point

```js
map.on('click', (e) => {
  const features = map.queryRenderedFeatures(e.point);
  console.log('Features at click:', features);
});
```

### Find Label Layers for beforeId

```js
const labelLayers = map.getStyle().layers
  .filter(l => l.id.includes('label'))
  .map(l => l.id);
console.log('Label layers:', labelLayers);
```
