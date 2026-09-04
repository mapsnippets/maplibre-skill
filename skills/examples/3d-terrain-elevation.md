# Official Example: 3D Terrain & Hillshade Elevation 🏔️🌲

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/3d-terrain/

This guide demonstrates how to configure real 3D digital elevation models (DEM) in MapLibre GL JS using MapTiler Terrain-RGB v2, dynamic terrain exaggeration, hillshading, and atmospheric sky fog.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre 3D Terrain</title>
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

// 1. Initialize Map centered on Mont Blanc / Alps with heavy pitch
const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/outdoor-v4/style.json?key=${apiKey}`,
  center: [6.865, 45.832], // Mont Blanc coordinates
  zoom: 12.5,
  pitch: 70,               // Dramatic low-angle camera tilt
  bearing: 30,
  maxPitch: 85
});

map.addControl(new maplibregl.NavigationControl({ visualizePitch: true }), 'top-right');

map.on('load', () => {
  // 2. Add Raster-DEM Elevation Source
  map.addSource('maptiler-terrain', {
    type: 'raster-dem',
    url: `https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=${apiKey}`,
    tileSize: 256,
    encoding: 'mapbox' // RGB decoding formula
  });

  // 3. Activate 3D Terrain across the entire map viewport
  map.setTerrain({
    source: 'maptiler-terrain',
    exaggeration: 1.4 // Multiplier for vertical relief
  });

  // 4. Add Shaded Relief / Hillshade Layer
  map.addLayer({
    id: 'hills',
    type: 'hillshade',
    source: 'maptiler-terrain',
    paint: {
      'hillshade-exaggeration': 0.6,
      'hillshade-shadow-color': '#1e293b',
      'hillshade-highlight-color': '#ffffff',
      'hillshade-illumination-direction': 315 // Sun angle from northwest
    }
  });

  // 5. Configure Atmospheric Horizon Sky Fog (MapLibre v4+)
  map.setSky({
    'sky-color': '#0084FF',
    'horizon-color': '#ffffff',
    'fog-color': '#cbd5e1',
    'fog-ground-blend': 0.5
  });
});
```
