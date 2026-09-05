# Polygon Drawing & GeoJSON Export (Mapbox GL Draw) ✏️📐

> **Documentation Link:** [Draw polygon with mapbox-gl-draw](https://maplibre.org/maplibre-gl-js/docs/examples/draw-polygon-with-mapbox-gl-draw/)  
> **Target Category:** Production Task Implementation

Equip MapLibre GL JS with an interactive vector digitization suite allowing users to draw custom polygon boundaries, delete features, and export the resulting GeoJSON coordinates.

---

## 1. HTML Container & Dependencies

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Polygon Drawing & GeoJSON Export</title>
  
  <!-- MapLibre GL JS -->
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <script src="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.js"></script>

  <!-- Mapbox GL Draw Plugin -->
  <link rel="stylesheet" href="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.4.3/mapbox-gl-draw.css" />
  <script src="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.4.3/mapbox-gl-draw.js"></script>

  <style>
    body { margin: 0; padding: 0; font-family: sans-serif; }
    #map { width: 100vw; height: 100vh; }
    .geojson-panel {
      position: absolute; bottom: 20px; left: 20px; z-index: 1000;
      background: rgba(15, 23, 42, 0.92); color: #f8fafc;
      padding: 14px 18px; border-radius: 8px; max-width: 340px; max-height: 220px;
      overflow: auto; box-shadow: 0 4px 16px rgba(0, 0, 0, 0.4);
      font-size: 12px;
    }
  </style>
</head>
<body>
  <div id="map"></div>
  <div class="geojson-panel">
    <b>📐 Drawn Geometry (GeoJSON)</b>
    <pre id="geojsonOutput" style="font-size:10px; margin-top:6px;">Use the toolbar above to draw a polygon</pre>
  </div>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
const KEY = 'YOUR_MAPTILER_API_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${KEY}`,
  center: [4.8952, 52.3702], // Amsterdam
  zoom: 13
});

// 1. Initialize MapboxDraw with Polygon and Trash controls
const draw = new MapboxDraw({
  displayControlsDefault: false,
  controls: {
    polygon: true,
    trash: true
  }
});

// 2. Add the Draw control to the map
map.addControl(draw, 'top-left');

// 3. Listen for creation, update, and deletion events
function updateGeometryOutput() {
  const data = draw.getAll();
  const output = document.getElementById('geojsonOutput');
  if (data.features.length > 0) {
    output.innerText = JSON.stringify(data, null, 2);
  } else {
    output.innerText = 'Use the toolbar above to draw a polygon';
  }
}

map.on('draw.create', updateGeometryOutput);
map.on('draw.update', updateGeometryOutput);
map.on('draw.delete', updateGeometryOutput);
```

---

## 3. Key Architecture & Invariants

| Parameter / Feature | Invariant Requirement |
| :--- | :--- |
| **Plugin Compatibility** | `@mapbox/mapbox-gl-draw@1.4.3` operates natively on MapLibre GL JS instances via standard `IControl` interface. |
| **Event Listeners** | Always listen to `draw.create`, `draw.update`, and `draw.delete` to capture geometry updates in real-time. |
| **Coordinate Export** | Call `draw.getAll()` to retrieve the complete GeoJSON `FeatureCollection`. |
| **Lifecycle Cleanup** | Call `map.removeControl(draw)` on SPA unmount to detach event listeners. |
