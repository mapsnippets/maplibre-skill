# MapLibre GL JS — Core Classes, Controls & API Methods Reference 🏛️

> Authoritative API reference for `maplibregl` classes, UI controls, custom `IControl` extensions, camera navigation, runtime styling methods, feature state, coordinate mathematics, and query methods.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Built-in UI Controls (`maplibregl.*Control`)

Add controls to the map using `map.addControl(control, position)`.
Available positions: `'top-left'`, `'top-right'`, `'bottom-left'`, `'bottom-right'`.

### A. NavigationControl
Provides zoom buttons and an interactive compass pitch/rotation ring:
```javascript
const nav = new maplibregl.NavigationControl({
  showCompass: true,     // Display rotation compass (default: true)
  showZoom: true,        // Display zoom +/- buttons (default: true)
  visualizePitch: true   // Tilt compass when map pitch changes (default: false)
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
  console.log('User coordinates:', e.coords.longitude, e.coords.latitude);
});
```

### C. ScaleControl
Displays dynamic distance scale bar:
```javascript
const scale = new maplibregl.ScaleControl({
  maxWidth: 100,             // Max width in pixels (default: 100)
  unit: 'metric'             // 'metric' (km/m), 'imperial' (mi/ft), or 'nautical' (nm)
});
map.addControl(scale, 'bottom-left');
```

### D. FullscreenControl & AttributionControl
```javascript
// Fullscreen button
map.addControl(new maplibregl.FullscreenControl(), 'top-right');

// Attribution control with compact folding for mobile
map.addControl(new maplibregl.AttributionControl({
  compact: true,
  customAttribution: '<a href="https://mapsnippets.org/" target="_blank">&copy; MapSnippets</a>'
}), 'bottom-right');
```

---

## 2. Custom UI Controls (`IControl` Interface)

Every custom control in MapLibre implements the `IControl` interface with two lifecycle methods: `onAdd(map)` and `onRemove()`.

```javascript
class ResetNorthControl {
  onAdd(map) {
    this._map = map;
    this._container = document.createElement('div');
    this._container.className = 'maplibregl-ctrl maplibregl-ctrl-group';
    
    const button = document.createElement('button');
    button.type = 'button';
    button.title = 'Reset Bearing to North';
    button.innerHTML = '🧭';
    button.style.fontSize = '14px';
    button.style.width = '29px';
    button.style.height = '29px';
    button.style.cursor = 'pointer';

    button.addEventListener('click', () => {
      this._map.resetNorthPitch({ duration: 800 });
    });

    this._container.appendChild(button);
    return this._container;
  }

  onRemove() {
    this._container.parentNode.removeChild(this._container);
    this._map = undefined;
  }
}

// Add custom control
map.addControl(new ResetNorthControl(), 'top-right');
```

---

## 3. `maplibregl.Map` Core Methods

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
* **`map.easeTo(options)`**: Linear or easing camera transition without arc zoom.
* **`map.jumpTo(options)`**: Instant camera repositioning without animation.
* **`map.fitBounds(bounds, options)`**: Fits bounding box in view:
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

### B. Dynamic Styling & Runtime Mutation
* **`map.setPaintProperty(layerId, name, value)`**: Update paint properties without re-parsing style.
  ```javascript
  map.setPaintProperty('buildings-3d', 'fill-extrusion-opacity', 0.85);
  map.setPaintProperty('route-line', 'line-color', '#FF6B00');
  ```
* **`map.setLayoutProperty(layerId, name, value)`**: Toggle visibility or text/icon layout.
  ```javascript
  // Toggle layer visibility
  const visibility = map.getLayoutProperty('labels-layer', 'visibility');
  map.setLayoutProperty('labels-layer', 'visibility', visibility === 'none' ? 'visible' : 'none');
  ```
* **`map.setFilter(layerId, filterExpression)`**: Dynamically filter visible features.
  ```javascript
  map.setFilter('airports', ['==', ['get', 'type'], 'international']);
  ```

### C. High-Performance Feature State (Hover & Selection)
**Never update GeoJSON sources on mousemove!** Use feature-state for 60 FPS updates:
```javascript
let hoveredFeatureId = null;

map.on('mousemove', 'parcels-fill', (e) => {
  if (e.features.length > 0) {
    if (hoveredFeatureId !== null) {
      map.setFeatureState(
        { source: 'parcels', id: hoveredFeatureId },
        { hover: false }
      );
    }
    hoveredFeatureId = e.features[0].id;
    map.setFeatureState(
      { source: 'parcels', id: hoveredFeatureId },
      { hover: true }
    );
  }
});

map.on('mouseleave', 'parcels-fill', () => {
  if (hoveredFeatureId !== null) {
    map.setFeatureState(
      { source: 'parcels', id: hoveredFeatureId },
      { hover: false }
    );
  }
  hoveredFeatureId = null;
});
```

### D. 3D Terrain, Sky & Globe Runtime Methods
```javascript
// 1. Enable 3D Terrain with Raster-DEM
map.setTerrain({ source: 'maptiler-terrain', exaggeration: 1.5 });

// 2. Disable 3D Terrain
// map.setTerrain(null);

// 3. Switch Projection to 3D Globe (MapLibre v4+)
map.setProjection({ type: 'globe' });

// 4. Configure Atmospheric Sky & Fog
map.setSky({
  'sky-color': '#0084FF',
  'horizon-color': '#ffffff',
  'fog-color': '#1e293b',
  'fog-ground-blend': 0.5
});
```

### E. Spatial Feature Querying
* **`map.queryRenderedFeatures(pointOrBox, options)`**:
  Query visible rendered vector/GeoJSON features at a pixel coordinate or bounding box:
  ```javascript
  map.on('click', (e) => {
    const bbox = [[e.point.x - 5, e.point.y - 5], [e.point.x + 5, e.point.y + 5]];
    const features = map.queryRenderedFeatures(bbox, {
      layers: ['poi-layer', 'transportation-layer']
    });
    if (features.length) {
      console.log('Clicked feature properties:', features[0].properties);
    }
  });
  ```
* **`map.querySourceFeatures(sourceId, options)`**:
  Query features in a source across all loaded tiles (regardless of visibility).

### F. Pixel & Coordinate Conversion
* **`map.project(lngLat)`**: Converts `[lng, lat]` coordinates to screen pixel coordinates `Point { x, y }`.
* **`map.unproject(point)`**: Converts screen pixel coordinates `[x, y]` to geographic `LngLat { lng, lat }`.

### G. Custom Image & Sprite Loading
```javascript
map.loadImage('https://docs.maptiler.com/assets/marker.png', (error, image) => {
  if (error) throw error;
  if (!map.hasImage('custom-marker')) {
    map.addImage('custom-marker', image, { pixelRatio: 2 });
  }
});
```

### H. Lifecycle & Cleanup
* **`map.remove()`**: **Mandatory cleanup method** in Single Page Applications (React `useEffect`, Vue `onUnmounted`, Svelte `onDestroy`). Tears down the WebGL context, detaches event listeners, and prevents browser context loss crashes.

---

## 4. UI Markers & Popups

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

// 2. Custom HTML Element Marker with CSS Pulse
const el = document.createElement('div');
el.className = 'custom-pulsing-marker';
el.style.width = '20px';
el.style.height = '20px';
el.style.background = '#0084FF';
el.style.border = '2px solid #ffffff';
el.style.borderRadius = '50%';
el.style.boxShadow = '0 0 10px rgba(0, 132, 255, 0.8)';

new maplibregl.Marker({ element: el, anchor: 'center' })
  .setLngLat([14.4378, 50.0755])
  .setPopup(new maplibregl.Popup({ offset: 20 }).setHTML('<h4>Prague HQ</h4>'))
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
    <div style="font-family: system-ui; padding: 4px;">
      <h4 style="margin: 0 0 4px; color: #0084FF;">Prague</h4>
      <p style="margin: 0; color: #475569; font-size: 13px;">Operations & Engineering Hub</p>
    </div>
  `)
  .addTo(map);
```

---

## 5. Coordinate Mathematics & Geometry Types

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
