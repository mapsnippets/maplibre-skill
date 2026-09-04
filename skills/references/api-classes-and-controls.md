# MapLibre GL JS Classes, Controls & API Encyclopedia 🏛️

> The authoritative API reference for **MapLibre GL JS (v4–v5+)**, covering `Map`, camera physics, layer & source methods, built-in controls, markers, popups, and spatial mathematics.

---

## 1. `maplibregl.Map`

The central class representing an interactive WebGL map instance.

```javascript
import maplibregl from 'maplibre-gl';

const map = new maplibregl.Map({
  container: 'map',
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_KEY',
  center: [8.5417, 47.3769],
  zoom: 12,
  pitch: 45,
  bearing: 0,
  maxPitch: 85,
  antialias: true
});
```

### Complete Constructor Options Table

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **`container`** | `String \| HTMLElement` | **Required** | The HTML element ID or DOM node where the map will be initialized. |
| **`style`** | `String \| Object` | **Required** | MapLibre style JSON URL or inline JSON style object. |
| **`center`** | `LngLatLike` | `[0, 0]` | Initial geographical center point `[lng, lat]`. |
| **`zoom`** | `Number` | `0` | Initial zoom level (`0`–`24`). |
| **`minZoom`** | `Number` | `0` | Minimum zoom constraint. |
| **`maxZoom`** | `Number` | `24` | Maximum zoom constraint. |
| **`bearing`** | `Number` | `0` | Camera rotation angle in degrees clockwise from North. |
| **`pitch`** | `Number` | `0` | Camera tilt angle in degrees (`0`–`85`). |
| **`minPitch`** | `Number` | `0` | Minimum allowable camera pitch. |
| **`maxPitch`** | `Number` | `85` | Maximum allowable camera pitch (default `60`, up to `85`). |
| **`bounds`** | `LngLatBoundsLike` | `undefined` | Initial viewport bounds to fit upon initialization. |
| **`fitBoundsOptions`**| `Object` | `{}` | Options passed to initial `fitBounds` (e.g. `padding`). |
| **`antialias`** | `Boolean` | `false` | Enables WebGL multisample antialiasing (MSAA) for smoother lines. |
| **`preserveDrawingBuffer`**| `Boolean` | `false` | Required `true` if taking canvas screenshots (`toDataURL()`). |
| **`cooperativeGestures`**| `Boolean` | `false` | Requires Command/Ctrl + scroll to zoom on desktop, two-finger pan on mobile. |
| **`attributionControl`**| `Boolean \| Object` | `true` | When `false`, suppresses the default bottom-right attribution bar. |
| **`hash`** | `Boolean \| String` | `false` | Synchronizes center, zoom, pitch, and bearing with URL hash fragment. |
| **`interactive`** | `Boolean` | `true` | Enables or disables all mouse, touch, and keyboard interactions. |
| **`transformRequest`**| `Function` | `undefined` | Callback invoked before any external HTTP request is made. |

---

## 2. Camera & Animation Methods

### `map.flyTo(options)`
Performs smooth, cinematic flight arcs between locations with automatic zooming out and back in:
```javascript
map.flyTo({
  center: [13.4050, 52.5200], // Berlin
  zoom: 14,
  pitch: 60,
  bearing: -30,
  speed: 1.2,          // Curve flight velocity (default 1.2)
  curve: 1.42,         // Flight path arc height (default 1.42)
  essential: true      // Honors user's prefers-reduced-motion preference
});
```

### `map.easeTo(options)`
Smooth linear or eased transition without high-altitude arc zooming:
```javascript
map.easeTo({
  center: [2.3522, 48.8566],
  zoom: 15,
  duration: 2000,
  easing: (t) => t * (2 - t)
});
```

### `map.fitBounds(bounds, options)`
Pans and zooms the camera to encompass a geographical bounding box:
```javascript
const bounds = [
  [-122.52, 37.70], // Southwest [lng, lat]
  [-122.35, 37.82]  // Northeast [lng, lat]
];

map.fitBounds(bounds, {
  padding: { top: 50, bottom: 50, left: 350, right: 50 }, // Asymmetric padding for UI sidebars
  maxZoom: 16,
  duration: 1500
});
```

---

## 3. Dynamic Layer & Source Methods

### Layer Mutation
* **`map.addLayer(layerObject, beforeId?)`**: Inserts a new style layer. Passing `beforeId` places the new layer underneath an existing layer (crucial for keeping labels on top).
* **`map.removeLayer(id)`**: Destroys the layer.
* **`map.getLayer(id)`**: Returns the layer configuration object.
* **`map.moveLayer(id, beforeId?)`**: Re-orders layer in the rendering stack.
* **`map.setFilter(layerId, filterExpression)`**: Updates attribute filter without reloading source data.
* **`map.setPaintProperty(layerId, name, value)`**: Updates paint property dynamically.
* **`map.setLayoutProperty(layerId, name, value)`**: Updates layout property (e.g. `'visibility'`, `'none'`).

### High-Speed Feature State (GPU Hover/Select)
* **`map.setFeatureState({ source, sourceLayer?, id }, stateObject)`**: Updates GPU shader uniform values for a specific feature ID at 60 FPS without re-parsing vector tiles.
* **`map.getFeatureState({ source, sourceLayer?, id })`**: Reads current runtime state.
* **`map.removeFeatureState({ source, sourceLayer?, id }, key?)`**: Clears runtime feature state.

---

## 4. Built-in Map Controls

All controls inherit from `maplibregl.IControl` and are added via `map.addControl(control, position)`:
* Positions: `'top-left'`, `'top-right'`, `'bottom-left'`, `'bottom-right'`.

### 1. `maplibregl.NavigationControl`
Provides zoom in/out buttons and a 3D compass orientation reset ring:
```javascript
const nav = new maplibregl.NavigationControl({
  showCompass: true,
  showZoom: true,
  visualizePitch: true // Tilts compass needle to reflect camera pitch
});
map.addControl(nav, 'top-right');
```

### 2. `maplibregl.GeolocateControl`
High-accuracy GPS location tracker with heading indicators:
```javascript
const geolocate = new maplibregl.GeolocateControl({
  positionOptions: { enableHighAccuracy: true },
  trackUserLocation: true,
  showUserHeading: true,
  fitBoundsOptions: { maxZoom: 16 }
});
map.addControl(geolocate, 'top-right');
```

### 3. `maplibregl.ScaleControl`
Dynamic metric, imperial, or nautical scale bar:
```javascript
const scale = new maplibregl.ScaleControl({
  maxWidth: 150,
  unit: 'metric' // 'metric' | 'imperial' | 'nautical'
});
map.addControl(scale, 'bottom-left');
```

### 4. `maplibregl.TerrainControl`
One-click UI button toggling 3D terrain on and off:
```javascript
const terrainControl = new maplibregl.TerrainControl({
  source: 'maptiler-terrain',
  exaggeration: 1.2
});
map.addControl(terrainControl, 'top-right');
```

---

## 5. `Marker` & `Popup` Components

### `maplibregl.Marker`
Anchors interactive HTML DOM nodes onto geographical coordinates:
```javascript
// Custom glowing DOM element
const el = document.createElement('div');
el.className = 'radar-beacon';

const marker = new maplibregl.Marker({
  element: el,
  anchor: 'bottom',
  draggable: true,
  rotationAlignment: 'map'
})
  .setLngLat([8.5417, 47.3769])
  .addTo(map);

marker.on('dragend', () => {
  const lngLat = marker.getLngLat();
  console.log('New marker coordinate:', lngLat);
});
```

### `maplibregl.Popup`
Anchored informational bubble cards with automatic collision pan:
```javascript
const popup = new maplibregl.Popup({
  closeButton: true,
  closeOnClick: false,
  maxWidth: '320px',
  offset: 25
})
  .setLngLat([8.5417, 47.3769])
  .setHTML(`
    <div style="font-family: sans-serif; padding: 4px;">
      <strong style="color: #0084FF;">Zurich Head Office</strong>
      <p style="margin: 4px 0 0; color: #64748b; font-size: 12px;">MapTiler Engineering Hub</p>
    </div>
  `)
  .addTo(map);

// Bind directly to marker
marker.setPopup(popup);
```

---

## 6. Custom Streaming Protocol Handlers (`addProtocol`)

Enables custom URL schemes (e.g. `pmtiles://`, `cog://`, `custom://`):
```javascript
maplibregl.addProtocol('custom-source', (params, abortController) => {
  return fetch(params.url.replace('custom-source://', 'https://'), {
    signal: abortController.signal
  })
    .then((res) => res.arrayBuffer())
    .then((data) => ({ data: data }));
});
```
