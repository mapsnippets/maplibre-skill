# Official Example: Animate a Point Along a Route 🚗📍

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/animate-a-point-along-a-route/

This tutorial shows how to animate a vehicle icon or dot smoothly along a multi-coordinate GeoJSON `LineString` route using `requestAnimationFrame` and source data streaming.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Animate Point Along Route</title>
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
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
  center: [14.42076, 50.08804],
  zoom: 13
});

// Route coordinates: [lng, lat]
const route = [
  [14.410, 50.080],
  [14.415, 50.082],
  [14.420, 50.085],
  [14.425, 50.088],
  [14.430, 50.090],
  [14.435, 50.092]
];

map.on('load', () => {
  // 1. Static Polyline Route
  map.addSource('route', {
    type: 'geojson',
    data: {
      type: 'Feature',
      geometry: { type: 'LineString', coordinates: route }
    }
  });

  map.addLayer({
    id: 'route-line',
    type: 'line',
    source: 'route',
    paint: {
      'line-color': '#0084FF',
      'line-width': 4,
      'line-opacity': 0.8
    }
  });

  // 2. Animated Vehicle Point Source
  map.addSource('point', {
    type: 'geojson',
    data: {
      type: 'Feature',
      geometry: { type: 'Point', coordinates: route[0] }
    }
  });

  map.addLayer({
    id: 'point-marker',
    type: 'circle',
    source: 'point',
    paint: {
      'circle-radius': 9,
      'circle-color': '#FF6B00',
      'circle-stroke-width': 3,
      'circle-stroke-color': '#ffffff'
    }
  });

  // 3. Animation Loop using requestAnimationFrame
  let step = 0;
  function animate() {
    step = (step + 1) % route.length;
    map.getSource('point').setData({
      type: 'Feature',
      geometry: { type: 'Point', coordinates: route[step] }
    });
    setTimeout(() => {
      requestAnimationFrame(animate);
    }, 400); // Step interval
  }

  animate();
});
```
