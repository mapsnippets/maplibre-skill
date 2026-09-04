# Recipe: Display Buildings in 3D (`fill-extrusion`) 🏢🌆

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/display-buildings-in-3d/

This tutorial shows how to render 3D building volumes in MapLibre GL JS using `fill-extrusion` layers, data-driven height expressions, and directional viewport sunlight lighting.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre 3D Buildings</title>
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

// Dataviz Dark basemap creates an exceptional aesthetic for 3D extrusions
const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=${apiKey}`,
  center: [14.42076, 50.08804],
  zoom: 15.5,
  pitch: 60,
  bearing: -17.6
});

map.on('load', () => {
  // 1. Locate the first symbol/label layer to insert 3D buildings beneath it
  const layers = map.getStyle().layers;
  let labelLayerId;
  for (let i = 0; i < layers.length; i++) {
    if (layers[i].type === 'symbol' && layers[i].layout && layers[i].layout['text-field']) {
      labelLayerId = layers[i].id;
      break;
    }
  }

  // 2. Add 3D Extruded Buildings Layer
  map.addLayer({
    id: '3d-buildings',
    source: 'maptiler_planet', // Built-in vector source ID in modern styles
    'source-layer': 'building',
    filter: ['!=', ['get', 'hide_3d'], true],
    type: 'fill-extrusion',
    minzoom: 14,
    paint: {
      'fill-extrusion-color': '#475569',
      // Dynamic height interpolation based on zoom and building height property
      'fill-extrusion-height': [
        'interpolate', ['linear'], ['zoom'],
        14, 0,
        14.05, ['get', 'render_height']
      ],
      'fill-extrusion-base': [
        'interpolate', ['linear'], ['zoom'],
        14, 0,
        14.05, ['get', 'render_min_height']
      ],
      'fill-extrusion-opacity': 0.85
    }
  }, labelLayerId); // Insert beneath labels so street names remain readable!

  // 3. Configure Directional Sunlight
  map.setLight({
    anchor: 'viewport',
    color: '#ffffff',
    intensity: 0.4,
    position: [1.15, 210, 30] // [radial distance, azimuthal angle, polar angle]
  });
});
```
