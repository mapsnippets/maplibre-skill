# Recipe: Create a Gradient Line (`line-gradient`) 🌈〰️

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/create-a-gradient-line-using-an-expression/

This tutorial shows how to render a dynamic color gradient along the length of a polyline route in MapLibre GL JS using `line-gradient`, `line-progress`, and `lineMetrics: true`.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Gradient Line</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script type="module" src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import * as maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=${apiKey}`,
  center: [-77.035, 38.889],
  zoom: 12
});

map.on('load', () => {
  // 1. Add GeoJSON Source with lineMetrics: true (MANDATORY for line-gradient)
  map.addSource('route', {
    type: 'geojson',
    lineMetrics: true, // Computes normalized [0.0 - 1.0] progression along polyline
    data: {
      type: 'Feature',
      properties: {},
      geometry: {
        type: 'LineString',
        coordinates: [
          [-77.044, 38.899],
          [-77.036, 38.895],
          [-77.028, 38.889],
          [-77.018, 38.885],
          [-77.009, 38.880]
        ]
      }
    }
  });

  // 2. Add Line Layer with line-gradient paint expression
  map.addLayer({
    id: 'gradient-route',
    type: 'line',
    source: 'route',
    layout: {
      'line-join': 'round',
      'line-cap': 'round'
    },
    paint: {
      'line-width': 8,
      'line-gradient': [
        'interpolate',
        ['linear'],
        ['line-progress'],
        0.0, '#00D2FF',  // Cyan (Start)
        0.3, '#0084FF',  // MapTiler Electric Blue
        0.7, '#f59e0b',  // Amber
        1.0, '#ef4444'   // Red (Finish)
      ]
    }
  });
});
```
