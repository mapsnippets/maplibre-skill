# Official Example: Display a Vector Map 🗺️⚡

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/display-a-map/

This tutorial shows the canonical setup for initializing MapLibre GL JS with hardware-accelerated WebGL vector tiles, embedding navigation zoom/compass controls, and fullscreen toggle buttons.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre GL JS — Display Vector Map</title>
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

// 1. Initialize MapLibre Map with Streets v4 Vector Style
const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
  center: [14.42076, 50.08804], // [longitude, latitude]
  zoom: 12,
  hash: true // Syncs viewport to URL hash (#12/50.088/14.420)
});

// 2. Add Navigation Controls (Zoom +/- and Interactive Bearing Compass)
map.addControl(new maplibregl.NavigationControl({
  showCompass: true,
  showZoom: true,
  visualizePitch: true // Tilts the compass needle when map pitch changes
}), 'top-right');

// 3. Add Fullscreen Button
map.addControl(new maplibregl.FullscreenControl(), 'top-right');

// 4. Add Metric Distance Scale Bar
map.addControl(new maplibregl.ScaleControl({
  maxWidth: 120,
  unit: 'metric'
}), 'bottom-left');

// 5. Lifecycle Ready Listener
map.on('load', () => {
  console.log('MapLibre GL JS vector map loaded successfully!');
});
```
