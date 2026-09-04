# MapLibre GL JS Events & Lifecycle Reference 📡

> Complete technical dictionary for all **MapLibre GL JS events**, payload signatures, lifecycle phases, and event-driven patterns.

---

## 1. Event Category Overview

MapLibre GL JS organizes events into 4 primary domains:
1. **Lifecycle & Render Events**: Map initialization, style loading, data arrival, and WebGL rendering.
2. **Camera & Movement Events**: Viewport panning, zooming, rotation, and pitch changes.
3. **User Interaction Events**: Mouse clicks, touches, hovers, drags, and gesture events.
4. **Layer-Scoped Events**: Spatial interaction filtered strictly to geometries in a specific layer.

---

## 2. Lifecycle & Render Events

| Event Name | Trigger Condition | Common Production Usage |
| :--- | :--- | :--- |
| **`load`** | Fires immediately after all initial resources (style, sprites, fonts) have loaded and the first visual frame renders. | Adding custom sources, layers, markers, and controls. |
| **`idle`** | Fires when the map enters an idle state: no animations in progress, all tiles downloaded, and rendering complete. | Taking automated UI test snapshots, hiding loading spinners. |
| **`render`** | Fires immediately after the map canvas finishes drawing a frame to the screen. | Synchronizing custom WebGL or canvas overlays. |
| **`error`** | Fires when an error occurs (e.g. 404 tile request, malformed GeoJSON, invalid style expression). | Error boundary logging and toast notifications. |
| **`data`** | Fires when any map data (style, source, tile) begins loading or finishes loading. | Tracking granular asset download progress. |
| **`dataloading`**| Fires when data begins loading. | Displaying network progress bars. |
| **`styledata`** | Fires when the map's style loads or changes. | Synchronizing UI theme toggles. |
| **`sourcedata`** | Fires when one of the map's sources loads or changes, or when a tile finishes loading. | Detecting when a specific GeoJSON source is ready for queries. |
| **`styleimagemissing`**| Fires when a symbol layer requests an icon from the sprite that does not exist. | Generating dynamic canvas icons on-the-fly via `map.addImage()`. |
| **`remove`** | Fires immediately after `map.remove()` is called. | Cleaning up parent component memory. |
| **`webglcontextlost`**| Fires when the browser's GPU context is killed. | Preventing uncaught WebGL errors. |
| **`webglcontextrestored`**| Fires when GPU context is recovered. | Calling `map.setStyle()` to re-instantiate WebGL pipelines. |

---

## 3. Camera Movement Events

Camera events fire during user gestures (drag, scroll) and programmatic animations (`flyTo`, `easeTo`):

| Event | Description |
| :--- | :--- |
| **`movestart`** | Fired just before the map begins moving. |
| **`move`** | Fired repeatedly at 60 FPS while the map center changes. |
| **`moveend`** | Fired when camera movement concludes. |
| **`zoomstart`** | Fired when camera zoom starts changing. |
| **`zoom`** | Fired repeatedly during zoom level changes. |
| **`zoomend`** | Fired when zooming concludes. |
| **`rotatestart`**| Fired when camera bearing begins changing. |
| **`rotate`** | Fired during camera bearing rotation. |
| **`rotateend`** | Fired when camera rotation ends. |
| **`pitchstart`** | Fired when camera tilt/pitch begins changing. |
| **`pitch`** | Fired repeatedly as camera tilt changes. |
| **`pitchend`** | Fired when camera tilt movement ends. |

```javascript
// Synchronizing a coordinate HUD during camera movement
map.on('move', () => {
  const center = map.getCenter();
  const zoom = map.getZoom().toFixed(2);
  document.getElementById('hud').textContent = 
    `Lng: ${center.lng.toFixed(4)}, Lat: ${center.lat.toFixed(4)} | Zoom: ${zoom}`;
});
```

---

## 4. User Interaction & Layer-Scoped Events

### Global vs Layer-Scoped Listeners
* **Global Listener**: `map.on('click', (e) => { ... })` — Fires anywhere on the canvas.
* **Layer-Scoped Listener**: `map.on('click', 'layer-id', (e) => { ... })` — Fires ONLY when the user clicks directly on geometry belonging to `'layer-id'`.

### Layer Mouse Hover Cursor Pattern
```javascript
const layerId = 'poi-markers';

// Change cursor to pointer on hover
map.on('mouseenter', layerId, () => {
  map.getCanvas().style.cursor = 'pointer';
});

// Reset cursor on exit
map.on('mouseleave', layerId, () => {
  map.getCanvas().style.cursor = '';
});

// Click popups on layer features
map.on('click', layerId, (e) => {
  const feature = e.features[0];
  new maplibregl.Popup()
    .setLngLat(e.lngLat)
    .setHTML(`<strong>${feature.properties.name}</strong>`)
    .addTo(map);
});
```

---

## 5. Event Object Signatures

### `MapMouseEvent` Payload
Available on `click`, `mousedown`, `mouseup`, `mousemove`:
* **`e.point`**: Screen pixel coordinates `{ x: number, y: number }` relative to the map container.
* **`e.lngLat`**: Geographical coordinate `{ lng: number, lat: number }`.
* **`e.originalEvent`**: Native browser `MouseEvent`.
* **`e.features`**: Array of GeoJSON features rendered at `e.point` (only present in layer-scoped callbacks).

### `MapTouchEvent` Payload
Available on `touchstart`, `touchend`, `touchmove`, `touchcancel`:
* **`e.point`**: Screen coordinates of primary touch.
* **`e.points`**: Array of all active touch points.
* **`e.lngLat`**: Geographical coordinate of primary touch.
* **`e.originalEvent`**: Native browser `TouchEvent`.
