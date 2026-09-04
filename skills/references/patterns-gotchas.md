# MapLibre GL JS Production Patterns & Gotchas ⚠️⚡

> Architectural guidelines, critical failure modes, performance benchmarks, and production gotchas for deploying **MapLibre GL JS** applications with MapTiler Cloud basemaps.

---

## 1. Top 7 Critical Gotchas & Architectural Pitfalls

### 1. `[lng, lat]` vs `[lat, lng]` Coordinate Inversion Trap
* **Gotcha**: Leaflet uses `[lat, lng]`. MapLibre GL JS, GeoJSON, and Turf.js strictly enforce **`[longitude, latitude]`** order.
* **Symptom**: Map centers in Antarctica or oceans, or throws coordinate out of range errors (`latitude must be between -90 and 90`).
* **Fix**: Always specify `[longitude, latitude]`:
  ```javascript
  // CORRECT: [lng, lat]
  map.setCenter([8.5417, 47.3769]); // Zurich
  ```

---

### 2. Asynchronous Style Loading Race Conditions
* **Gotcha**: Calling `map.addSource()` or `map.addLayer()` immediately after `new maplibregl.Map()` will throw:
  `Error: Style is not done loading`.
* **Fix**: Always defer custom layers until the style has finished downloading:
  ```javascript
  // Pattern A: On initial map load
  map.on('load', () => {
    map.addSource('custom-data', { ... });
  });

  // Pattern B: Safe helper check
  function ensureLayer(map, layerConfig) {
    if (map.isStyleLoaded()) {
      map.addLayer(layerConfig);
    } else {
      map.once('load', () => map.addLayer(layerConfig));
    }
  }
  ```

---

### 3. Reactive State Proxy Wrapping in React / Vue
* **Gotcha**: Putting the MapLibre `map` instance into reactive framework state (e.g. React `useState()`, Vue `ref()`, Pinia store) wraps the instance in a JavaScript `Proxy`.
* **Symptom**: Massive FPS drop, memory leaks, and cryptic WebGL crashes (`TypeError: Cannot read properties of undefined`).
* **Fix**: Use non-reactive containers:
  * **React**: Use `useRef(null)`.
  * **Vue 3**: Use `shallowRef()` or `markRaw(map)`.
  * **Svelte**: Use a standard `let map` variable outside stores.

```typescript
// React Example (CORRECT)
const mapContainer = useRef<HTMLDivElement | null>(null);
const mapInstance = useRef<maplibregl.Map | null>(null);

useEffect(() => {
  if (!mapContainer.current) return;
  mapInstance.current = new maplibregl.Map({ ... });

  return () => {
    mapInstance.current?.remove(); // Cleanup WebGL context
  };
}, []);
```

---

### 4. High-Frequency GeoJSON Animation Trap
* **Gotcha**: Calling `source.setData()` at 60 FPS inside `requestAnimationFrame` forces the browser's web workers to re-triangulate and re-tessellate geometries on every single frame, causing stutter and CPU exhaustion.
* **Fix**:
  * **For moving vehicle icons**: Use DOM `maplibregl.Marker` and call `marker.setLngLat([lng, lat])` (zero tile tessellation overhead).
  * **For hover/selection polygon highlights**: Use `map.setFeatureState()` which modifies uniforms directly on the GPU without touching the geometry buffer.

---

### 5. The `generateId` / `promoteId` Requirement for `feature-state`
* **Gotcha**: Calling `map.setFeatureState({ source: 'places', id: feat.id }, { hover: true })` does nothing if features in GeoJSON lack a top-level numeric `id`.
* **Symptom**: Polygon hover effects fail to illuminate.
* **Fix**:
  ```javascript
  map.addSource('places', {
    type: 'geojson',
    data: '/api/places.geojson',
    generateId: true // Generates sequential unique numeric IDs
    // OR promote an existing property:
    // promoteId: 'osm_id'
  });
  ```

---

### 6. Canvas Export / Screenshot Blank Image Trap
* **Gotcha**: Trying to export map canvas images via `map.getCanvas().toDataURL()` returns a completely blank or black image.
* **Why**: WebGL automatically clears the drawing buffer after each frame render to conserve GPU memory.
* **Fix**: Pass `preserveDrawingBuffer: true` in the constructor:
  ```javascript
  const map = new maplibregl.Map({
    container: 'map',
    style: styleUrl,
    preserveDrawingBuffer: true // Required for image capture
  });

  function exportPng() {
    const dataUrl = map.getCanvas().toDataURL('image/png');
    // Download or save dataUrl
  }
  ```

---

### 7. PMTiles / Custom Protocol `slice of null` Crash
* **Gotcha**: Forgetting to register the PMTiles protocol handler before loading a style that uses a `pmtiles://` source causes an unhandled rejection.
* **Fix**: Register the protocol globally before initializing `Map`:
  ```javascript
  import { Protocol } from 'pmtiles';
  const protocol = new Protocol();
  maplibregl.addProtocol('pmtiles', protocol.tile);
  ```

---

## 2. WebGL Context Loss & Memory Management

In dynamic web applications where maps are frequently created and destroyed (e.g. tabs, modals, dynamic routes), failing to dispose of WebGL instances will quickly trigger:
`WARNING: Too many active WebGL contexts. Oldest context will be lost`.

### Safe Teardown Protocol
```javascript
function teardownMap(map) {
  if (!map) return;
  // 1. Remove all active interval/RAF timers
  // 2. Remove all custom controls
  // 3. Destroy WebGL canvas and terminate web worker pools
  map.remove();
}
```

### Listening for WebGL Context Loss & Recovery
```javascript
map.getCanvas().addEventListener('webglcontextlost', (event) => {
  event.preventDefault();
  console.warn('WebGL context lost! Waiting for restoration...');
});

map.getCanvas().addEventListener('webglcontextrestored', () => {
  console.log('WebGL context restored. Reloading style...');
  map.setStyle(map.getStyle());
});
```

---

## 3. High-Performance Mobile Optimization Checklist

1. **Enable Cooperative Gestures**: Prevents page scroll trapping on touch screens:
   ```javascript
   cooperativeGestures: true
   ```
2. **Cap Maximum Pitch**: High pitch angles (> 70°) render vast horizons requiring extra tile requests. Cap pitch on mobile:
   ```javascript
   maxPitch: 60
   ```
3. **Limit Terrain Exaggeration**: On low-powered mobile GPUs, reduce DEM resolution or keep `exaggeration` around `1.0` to avoid fill-rate bottlenecks.
4. **Use Symbol Clustering**: Aggregate dense point clouds at zoom levels < 14 using `cluster: true` and `clusterRadius: 50`.
