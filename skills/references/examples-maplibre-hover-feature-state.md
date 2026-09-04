# Official Example: High-Performance Hover Effect (`feature-state`) 🖱️✨

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/create-a-hover-effect/

**Never call `getSource().setData()` on `mousemove`!** This tutorial demonstrates the 60 FPS standard pattern using `map.setFeatureState` and shader paint expressions (`["feature-state", "hover"]`) to highlight polygons seamlessly without re-uploading geometries to GPU memory.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Hover Feature State</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
  center: [-100.48, 37.78],
  zoom: 3
});

let hoveredStateId = null;

map.on('load', () => {
  // 1. Add GeoJSON Source with generateId: true
  // Feature-state REQUIRES every feature to have a unique numeric 'id'
  map.addSource('states', {
    type: 'geojson',
    data: 'https://raw.githubusercontent.com/datasets/geo-boundaries-world-110m/master/countries.geojson',
    generateId: true // Automatically generates IDs for features lacking one
  });

  // 2. Polygon Fill Layer reading feature-state in paint expression
  map.addLayer({
    id: 'state-fills',
    type: 'fill',
    source: 'states',
    paint: {
      'fill-color': '#0084FF',
      'fill-opacity': [
        'case',
        ['boolean', ['feature-state', 'hover'], false],
        0.8,  // Hovered opacity
        0.2   // Default opacity
      ]
    }
  });

  // 3. Border Stroke Layer
  map.addLayer({
    id: 'state-borders',
    type: 'line',
    source: 'states',
    paint: {
      'line-color': '#0084FF',
      'line-width': [
        'case',
        ['boolean', ['feature-state', 'hover'], false],
        3.0,
        1.0
      ]
    }
  });

  // 4. Mousemove Event Handler
  map.on('mousemove', 'state-fills', (e) => {
    if (e.features.length > 0) {
      if (hoveredStateId !== null) {
        // Reset previously hovered feature
        map.setFeatureState(
          { source: 'states', id: hoveredStateId },
          { hover: false }
        );
      }
      // Set new hovered feature
      hoveredStateId = e.features[0].id;
      map.setFeatureState(
        { source: 'states', id: hoveredStateId },
        { hover: true }
      );
      map.getCanvas().style.cursor = 'pointer';
    }
  });

  // 5. Mouseleave Event Handler
  map.on('mouseleave', 'state-fills', () => {
    if (hoveredStateId !== null) {
      map.setFeatureState(
        { source: 'states', id: hoveredStateId },
        { hover: false }
      );
    }
    hoveredStateId = null;
    map.getCanvas().style.cursor = '';
  });
});
```
