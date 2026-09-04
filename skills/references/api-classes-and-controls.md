# MapLibre GL JS — Core Classes, Controls & API Methods Reference 🏛️

> The authoritative API reference for `maplibregl` classes, UI controls, camera operations, coordinate mathematics, and query methods.

---

## 1. UI Controls (`maplibregl.*Control`)

Add controls to the map using `map.addControl(control, position)`.
Available positions: `'top-left'`, `'top-right'`, `'bottom-left'`, `'bottom-right'`.

### A. NavigationControl
Provides zoom buttons and a compass rotation indicator:
```javascript
const nav = new maplibregl.NavigationControl({
  showCompass: true,     // Display rotation compass (default: true)
  showZoom: true,        // Display zoom +/- buttons (default: true)
  visualizePitch: true   // Tilt compass when pitch changes (default: false)
});
map.addControl(nav, 'top-right');
```

### B. GeolocateControl
Tracks user location via the browser Geolocation API:
```javascript
const geolocate = new maplibregl.GeolocateControl({
  positionOptions: {
    enableHighAccuracy: true
  },
  trackUserLocation: true,    // Continuously follow user movement
  showAccuracyCircle: true,   // Display 95% confidence radius circle
  showUserLocation: true      // Render glowing user position dot
});
map.addControl(geolocate, 'top-right');

geolocate.on('geolocate', (e) => {
  console.log('Coordinates:', e.coords.longitude, e.coords.latitude);
});
```

### C. ScaleControl
Displays a dynamic distance scale bar:
```javascript
const scale = new maplibregl.ScaleControl({
  maxWidth: 100,             // Max width in pixels (default: 100)
  unit: 'metric'             // 'metric' (km/m), 'imperial' (mi/ft), or 'nautical' (nm)
});
map.addControl(scale, 'bottom-left');
```

### D. FullscreenControl
Toggles fullscreen DOM mode for the map:
```javascript
const fullscreen = new maplibregl.FullscreenControl({
  container: document.querySelector('body') // Element to expand (default: map container)
});
map.addControl(fullscreen, 'top-right');
```

### E. AttributionControl
Renders copyright and data attributions:
```javascript
const attribution = new maplibregl.AttributionControl({
  compact: true,             // Collapse into an (i) info icon on mobile
  customAttribution: '<a href="https://example.com">&copy; Custom Data</a>'
});
map.addControl(attribution, 'bottom-right');
```

---

## 2. `maplibregl.Map` Core Methods

### A. Camera & Viewport Animations
* **`map.flyTo(options)`**: Smooth cinematic flight with arc zoom.
  ```javascript
  map.flyTo({
    center: [14.4378, 50.0755],
    zoom: 14,
    pitch: 45,
    bearing: 90,
    speed: 1.2,       // Curve speed multiplier
    curve: 1.42,      // Flight curve rate
    essential: true   // Respects prefers-reduced-motion if false
  });
  ```
* **`map.easeTo(options)`**: Linear or easing camera transition.
* **`map.jumpTo(options)`**: Instant camera teleportation without animation.
* **`map.panTo(lngLat, options)`**: Pan map center to `[lng, lat]`.
* **`map.panBy([x, y], options)`**: Pan viewport by pixel delta.
* **`map.zoomTo(zoom, options)`**: Set zoom level.
* **`map.fitBounds(bounds, options)`**: Fit bounding box in view:
  ```javascript
  map.fitBounds([
    [14.2, 49.9], // Southwest [lng, lat]
    [14.7, 50.2]  // Northeast [lng, lat]
  ], {
    padding: { top: 50, bottom: 50, left: 30, right: 30 },
    maxZoom: 16,
    duration: 1500
  });
  ```

### B. Spatial Feature Querying
* **`map.queryRenderedFeatures(pointOrBox, options)`**:
  Query visible rendered vector/GeoJSON features at a pixel coordinate or bounding box:
  ```javascript
  map.on('click', (e) => {
    const bbox = [[e.point.x - 5, e.point.y - 5], [e.point.x + 5, e.point.y + 5]];
    const features = map.queryRenderedFeatures(bbox, {
      layers: ['poi-layer', 'transportation-layer']
    });
    if (features.length) {
      console.log('Clicked feature:', features[0].properties);
    }
  });
  ```
* **`map.querySourceFeatures(sourceId, options)`**:
  Query features in a source across all loaded tiles (regardless of visibility).

### C. Pixel & Coordinate Conversion
* **`map.project(lngLat)`**: Converts `[lng, lat]` coordinates to screen pixel coordinates `Point { x, y }`.
* **`map.unproject(point)`**: Converts screen pixel coordinates `[x, y]` to geographic `LngLat { lng, lat }`.

### D. Custom Image & Sprite Loading
```javascript
map.loadImage('https://docs.maptiler.com/assets/marker.png', (error, image) => {
  if (error) throw error;
  if (!map.hasImage('custom-marker')) {
    map.addImage('custom-marker', image, { pixelRatio: 2 });
  }
});
```

### E. Lifecycle & Cleanup
* **`map.remove()`**: **Mandatory cleanup method** in Single Page Applications (React `useEffect`, Vue `onUnmounted`, Svelte `onDestroy`). Tears down the WebGL context, detaches event listeners, and prevents browser context loss crashes.

---

## 3. UI Markers & Popups

### A. Marker (`maplibregl.Marker`)
```javascript
// 1. Default SVG Pin
const marker = new maplibregl.Marker({
  color: '#0084FF',
  draggable: true
})
  .setLngLat([14.4378, 50.0755])
  .addTo(map);

marker.on('dragend', () => {
  const newPos = marker.getLngLat();
  console.log('Dragged to:', newPos.lng, newPos.lat);
});

// 2. Custom HTML Element Marker
const el = document.createElement('div');
el.className = 'custom-pulsing-marker';
el.style.width = '24px';
el.style.height = '24px';
el.style.background = '#00D2FF';
el.style.borderRadius = '50%';

new maplibregl.Marker({ element: el, anchor: 'center' })
  .setLngLat([14.4378, 50.0755])
  .setPopup(new maplibregl.Popup({ offset: 25 }).setHTML('<h3>Custom Pin</h3>'))
  .addTo(map);
```

### B. Popup (`maplibregl.Popup`)
```javascript
const popup = new maplibregl.Popup({
  closeButton: true,
  closeOnClick: false,
  maxWidth: '320px',
  offset: 15
})
  .setLngLat([14.4378, 50.0755])
  .setHTML(`
    <div style="padding: 8px;">
      <h4 style="margin: 0 0 4px;">Prague</h4>
      <p style="margin: 0; color: #666;">Capital of Czech Republic</p>
    </div>
  `)
  .addTo(map);
```

---

## 4. Coordinate Mathematics & Geometry Types

* **`maplibregl.LngLat(lng, lat)`**:
  * `lngLat.wrap()`: Normalizes longitude to `[-180, 180]`.
  * `lngLat.toArray()`: Returns `[lng, lat]`.
  * `lngLat.distanceTo(otherLngLat)`: Great-circle distance in meters.
* **`maplibregl.LngLatBounds(sw, ne)`**:
  * `bounds.extend(lngLat)`: Expands bounding box to include coordinate.
  * `bounds.getCenter()`: Returns center `LngLat`.
  * `bounds.contains(lngLat)`: Returns `true` if coordinate is inside.
* **`maplibregl.MercatorCoordinate(x, y, z)`**:
  * Project WGS84 geographic coordinates to WebGL normalized 3D space (`[0, 1]`):
  * `MercatorCoordinate.fromLngLat([lng, lat], altitudeInMeters)`.
  * Essential for custom Three.js layers and WebGL shader matrix integration.

---

## 5. Evented Base Class (`maplibregl.Evented`)

The base class for `Map`, `Marker`, `Popup`, and `GeolocateControl` providing asynchronous event dispatching:
```javascript
// Register event listener
map.on('click', (e) => { ... });

// One-time listener
map.once('load', () => { ... });

// Remove listener
map.off('click', handler);

// Custom Evented emitter
class CustomPlugin extends maplibregl.Evented {
  notify(data) {
    this.fire('customEvent', data);
  }
}
```
