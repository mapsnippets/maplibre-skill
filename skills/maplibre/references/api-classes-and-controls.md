# MapLibre GL JS Classes, Controls & API Encyclopedia 🏛️

> The authoritative API reference for **MapLibre GL JS v6 (v6.7.0 ESM)**, mirroring the canonical taxonomy of [`maplibre.org/maplibre-gl-js/docs/API/`](https://maplibre.org/maplibre-gl-js/docs/API/). Covers Main classes, Controls, Geography, Handlers, Sources, Events, and Global Functions.

---

## 🧭 API Architecture & System Overview

MapLibre GL JS organizes its public API into 6 primary structural domains:

```text
MapLibre GL JS API
├── 1. Main (Map, MercatorCoordinate, Style, EdgeInsets, Hash)
├── 2. Markers & Controls (Navigation, Geolocate, Scale, Attribution, Fullscreen, Terrain, Globe, Logo, Marker, Popup)
├── 3. Geography & Geometry (LngLat, LngLatBounds, Point, EdgeInsets)
├── 4. Handlers (ScrollZoom, DragPan, DragRotate, BoxZoom, DoubleClickZoom, Keyboard, CooperativeGestures, Touch)
├── 5. Sources (GeoJSONSource, VectorTileSource, RasterTileSource, RasterDEMTileSource, ImageSource, VideoSource, CanvasSource)
├── 6. Event Classes (MapMouseEvent, MapTouchEvent, MapWheelEvent, MapSourceDataEvent, MapStyleDataEvent, MapTerrainEvent, ErrorEvent)
└── 7. Global Functions (addProtocol, removeProtocol, prewarm, setWorkerCount, setWorkerUrl, setRTLTextPlugin)
```

---

## 1. Main Classes

### `maplibregl.Map`
The central WebGL map instance managing viewport rendering, camera physics, style lifecycle, sources, and layers.

```javascript
import * as maplibregl from 'maplibre-gl';

const map = new maplibregl.Map({
  container: 'map',
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_KEY',
  center: [8.5417, 47.3769], // [lng, lat]
  zoom: 12,
  pitch: 45,
  bearing: 0,
  maxPitch: 85,
  antialias: true
});
```

#### Key Constructor Options
| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **`container`** | `String \| HTMLElement` | **Required** | The HTML element ID or DOM node for initialization. |
| **`style`** | `String \| Object` | **Required** | Style JSON URL or inline style object. |
| **`center`** | `LngLatLike` | `[0, 0]` | Initial geographical center point `[lng, lat]`. |
| **`zoom`** | `Number` | `0` | Initial zoom level (`0`–`24`). |
| **`minZoom`** | `Number` | `0` | Minimum zoom constraint. |
| **`maxZoom`** | `Number` | `24` | Maximum zoom constraint. |
| **`pitch`** | `Number` | `0` | Camera pitch tilt (`0`–`85`°). |
| **`bearing`** | `Number` | `0` | Camera rotation angle in degrees clockwise from North. |
| **`antialias`** | `Boolean` | `false` | Enables WebGL MSAA antialiasing for crisp lines. |
| **`preserveDrawingBuffer`** | `Boolean` | `false` | Set `true` if taking canvas screenshots (`toDataURL()`). |
| **`cooperativeGestures`** | `Boolean` | `false` | Requires Command/Ctrl + scroll to zoom on desktop, two fingers on mobile. |
| **`transformRequest`** | `Function` | `undefined` | Callback invoked before any external HTTP request is made. |

---

### `maplibregl.MercatorCoordinate`
High-precision projected coordinate system used for custom WebGL and Three.js 3D layers.

* `fromLngLat(lngLatLike, altitude?)`: Projects geographical coordinate into normalized Mercator coordinates `[0..1]`.
* `toLngLat()`: Converts back to `LngLat`.
* `meterInMercatorCoordinateUnits()`: Calculates scaling factor for metric 3D geometries at the given latitude.

---

### `maplibregl.Style`
The internal style manager governing layer order, sources, sprites, glyphs, and runtime modifications. Accessed via `map.style`.

---

## 2. Markers & Controls

All controls implement `maplibregl.IControl` and are positioned via `map.addControl(control, position)` (`'top-left'`, `'top-right'`, `'bottom-left'`, `'bottom-right'`).

### 1. `maplibregl.NavigationControl`
Zoom in/out buttons and a 3D compass orientation reset ring:
```javascript
const nav = new maplibregl.NavigationControl({
  showCompass: true,
  showZoom: true,
  visualizePitch: true // Tilts compass needle to reflect camera pitch
});
map.addControl(nav, 'top-right');
```

### 2. `maplibregl.GeolocateControl`
GPS device tracking with user heading and accuracy circle:
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
const terrain = new maplibregl.TerrainControl({
  source: 'maptiler-terrain',
  exaggeration: 1.2
});
map.addControl(terrain, 'top-right');
```

### 5. `maplibregl.GlobeControl`
Toggle button switching projection between Flat Mercator and 3D Spherical Globe view:
```javascript
const globe = new maplibregl.GlobeControl();
map.addControl(globe, 'top-right');
```

### 6. `maplibregl.FullscreenControl`
Toggles browser-native HTML5 fullscreen display:
```javascript
map.addControl(new maplibregl.FullscreenControl(), 'top-right');
```

### 7. `maplibregl.AttributionControl` & `maplibregl.LogoControl`
Manages mandatory legal attributions and the MapLibre logo display.

---

### `maplibregl.Marker` & `maplibregl.Popup`

```javascript
// Anchored DOM Marker with drag events
const marker = new maplibregl.Marker({
  color: '#0084FF',
  draggable: true,
  rotationAlignment: 'map'
})
  .setLngLat([8.5417, 47.3769])
  .addTo(map);

// Collision-avoiding Popup
const popup = new maplibregl.Popup({ offset: 25, closeButton: false })
  .setHTML('<strong>Zurich Hub</strong>')
  .setLngLat([8.5417, 47.3769])
  .addTo(map);

marker.setPopup(popup);
```

---

## 3. Geography & Geometry

* **`maplibregl.LngLat(lng, lat)`**: Geographical point representation. Methods: `wrap()`, `distanceTo(targetLngLat)`, `toArray()`, `toBounds(radiusInMeters)`.
* **`maplibregl.LngLatBounds(sw, ne)`**: Rectangular geographical bounding box. Methods: `extend(lngLatOrBounds)`, `getCenter()`, `contains(lngLat)`, `toBBoxString()`.
* **`maplibregl.Point(x, y)`**: 2D screen-space pixel vector. Methods: `add()`, `sub()`, `mult()`, `div()`, `dist()`.
* **`maplibregl.EdgeInsets(top, bottom, left, right)`**: Viewport padding offsets.
* **`maplibregl.Hash`**: Synchronizes center, zoom, pitch, and bearing with URL hash fragment.

---

## 4. User Interaction Handlers

Map interaction modes can be dynamically toggled via `map[handlerName].enable()` or `disable()`:

| Handler Property | Class | Functionality |
| :--- | :--- | :--- |
| **`map.scrollZoom`** | `ScrollZoomHandler` | Wheel and trackpad zooming. |
| **`map.boxZoom`** | `BoxZoomHandler` | Shift-drag bounding box zoom. |
| **`map.dragPan`** | `DragPanHandler` | Mouse or finger viewport dragging. |
| **`map.dragRotate`** | `DragRotateHandler` | Right-click/Ctrl-drag camera rotation & pitch. |
| **`map.keyboard`** | `KeyboardHandler` | Arrow keys and +/- navigation. |
| **`map.doubleClickZoom`** | `DoubleClickZoomHandler` | Double-click zooming. |
| **`map.cooperativeGestures`**| `CooperativeGesturesHandler`| Ctrl+scroll desktop prompt & two-finger mobile panning. |
| **`map.touchZoomRotate`** | `TwoFingersTouchZoomRotateHandler` | Multi-touch pinch zoom & rotation. |
| **`map.touchPitch`** | `TwoFingersTouchPitchHandler` | Multi-touch two-finger pitch tilt. |
| **`TouchZoomHandler`** | `TwoFingersTouchZoomHandler` | Dedicated touch pinch zoom handler. |
| **`TouchRotateHandler`** | `TwoFingersTouchRotateHandler` | Dedicated touch two-finger rotate handler. |

---

## 5. Sources & Data Providers

Sources feed raw geospatial data into WebGL render pipelines:

```javascript
// GeoJSON Source with real-time update capability
map.addSource('telemetry', {
  type: 'geojson',
  data: { type: 'FeatureCollection', features: [] },
  cluster: true,
  clusterMaxZoom: 14,
  clusterRadius: 50
});

// Update at runtime:
const source = map.getSource('telemetry');
source.setData(newGeoJsonData);
```

* **`GeoJSONSource`**: In-memory vector features. Supports `setData()`, `getClusterExpansionZoom()`, `getClusterChildren()`, and `getClusterLeaves()`.
* **`VectorTileSource`**: Vector tile streams (MVT/PBF). Supports `setTiles()`, `setUrl()`.
* **`RasterTileSource`**: Standard satellite, aerial, or basemap raster tiles.
* **`RasterDEMTileSource`**: Digital Elevation Model tiles for 3D terrain (`encoding: 'mapbox'` or `'terrarium'`).
* **`ImageSource`**: Georeferenced image coordinates with `setCoordinates()`.
* **`VideoSource`**: Georeferenced HTML5 video element draped onto coordinates.
* **`CanvasSource`**: Procedural HTML5 canvas draping with `getCanvas()`, `play()`, and `pause()`.

---

## 6. Event Classes & Error Handling

All event classes emitted across the map lifecycle (detailed in [events.md](events.md)):

* **Map Lifecycle Events**: `MapLibreEvent`, `MapStyleLoadEvent`, `MapStyleDataEvent`, `MapSourceDataEvent`, `MapContextEvent`, `MapTerrainEvent`, `MapProjectionEvent`
* **Interaction Events**: `MapMouseEvent`, `MapTouchEvent`, `MapWheelEvent`, `MapBoxZoomEvent`, `MapMovementEvent`
* **Component Events**: `MarkerClickEvent`, `MarkerDragEvent`, `PopupEvent`, `GeolocateEvent`, `GeolocatePositionEvent`, `GeolocateErrorEvent`, `FullscreenEvent`
* **Errors**: `ErrorEvent`, `AJAXError` (network/tile download errors), `GPUInitializationError` (WebGL context failures)
* **Base Event Architecture**: `Evented` (event emitter mixin base class)

---

## 7. Global Functions & Lifecycle Management

MapLibre exports several critical global utilities on the top-level `maplibregl` namespace:

### Protocol Handlers (`addProtocol` / `removeProtocol`)
Registers custom URI protocols (e.g. `pmtiles://`, `mbtiles://`):
```javascript
maplibregl.addProtocol('custom', (params, abortController) => {
  return fetch(params.url.replace('custom://', 'https://'), { signal: abortController.signal })
    .then(r => r.arrayBuffer())
    .then(data => ({ data }));
});
```

### Custom Source Types (`addSourceType`)
* **`maplibregl.addSourceType(name, SourceType, callback)`**: Extends MapLibre with proprietary or specialized vector data decoders.

### Performance & Prewarming
* **`maplibregl.prewarm()`**: Initializes WebGL shader programs and Web Workers ahead of map creation to minimize initial render latency.
* **`maplibregl.clearPrewarmedResources()`**: Releases prewarmed memory if a map is not mounted immediately.

### Worker Threads & Concurrency
* **`maplibregl.setWorkerCount(count)`** / **`getWorkerCount()`**: Configures number of background web workers for tile parsing (default based on CPU cores).
* **`maplibregl.setWorkerUrl(url)`** / **`getWorkerUrl()`**: Overrides the web worker script bundle URL.
* **`maplibregl.setMaxParallelImageRequests(num)`** / **`getMaxParallelImageRequests()`**: Controls image fetch concurrency limits.
* **`maplibregl.importScriptInWorkers(url)`**: Imports auxiliary scripts into worker environments.

### Internationalization & Scripts
* **`maplibregl.setRTLTextPlugin(url, callback, deferred)`**: Loads the bidirectional text shaping plugin for Arabic and Hebrew labels.
* **`maplibregl.getRTLTextPluginStatus()`**: Checks whether the RTL plugin is `'unavailable'`, `'loading'`, or `'loaded'`.

### Version & Utilities
* **`maplibregl.getVersion()`**: Returns the MapLibre GL JS version string (e.g. `6.7.0`).
* **`maplibregl.createTileMesh(options)`**: Utility function creating a 3D geometry mesh for tiles.
