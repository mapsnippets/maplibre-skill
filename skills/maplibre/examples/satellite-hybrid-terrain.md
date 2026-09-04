# Recipe: MapTiler Satellite Hybrid with 3D Terrain 🛰️🏔️

> **Documentation Reference:** https://docs.maptiler.com/gl-style-specification/terrain/

This recipe demonstrates how to render high-resolution MapTiler Satellite Hybrid imagery draped across 3D digital elevation models (DEM) with MapLibre GL JS v6 (v6.7.0 ESM), combining photorealistic satellite textures, razor-sharp vector road labels, and real terrain elevation.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre GL JS — MapTiler Satellite Hybrid 3D</title>
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

// 1. Initialize MapLibre with MapTiler Satellite Hybrid v4
const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/hybrid-v4/style.json?key=${apiKey}`,
  center: [7.7491, 45.9765], // Matterhorn / Zermatt, Swiss Alps
  zoom: 13,
  pitch: 65,                 // Low-angle perspective to emphasize 3D peaks
  bearing: -25,
  maxPitch: 85
});

// 2. Add Navigation Controls with pitch needle
map.addControl(new maplibregl.NavigationControl({
  visualizePitch: true,
  showCompass: true,
  showZoom: true
}), 'top-right');

map.on('load', () => {
  // 3. Add MapTiler Terrain-RGB v2 Elevation Source
  map.addSource('maptiler-dem', {
    type: 'raster-dem',
    url: `https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=${apiKey}`,
    tileSize: 256,
    encoding: 'mapbox'
  });

  // 4. Activate 3D Terrain Elevation
  map.setTerrain({
    source: 'maptiler-dem',
    exaggeration: 1.5 // 1.5x vertical relief for mountain landscapes
  });

  // 5. Add Atmospheric Sky Horizon
  map.setSky({
    'sky-color': '#87CEEB',
    'sky-horizon-blend': 0.5,
    'horizon-color': '#FFFFFF',
    'horizon-fog-blend': 0.8,
    'fog-color': '#dce8f5',
    'fog-ground-blend': 0.6
  });
});
```

---

## 3. Production Best Practices

1. **MapTiler Hybrid v4**: The `hybrid-v4` style provides high-resolution, cloudless satellite imagery combined with vector road networks, administrative boundaries, and multilingual place names.
2. **Terrain-RGB v2 Integration**: `terrain-rgb-v2` encodes elevation in Red, Green, and Blue color channels (`height = -10000 + ((R * 256 * 256 + G * 256 + B) * 0.1)`), decoded instantly by MapLibre's WebGL shaders.
3. **Camera Pitch**: Keeping `maxPitch: 85` allows users to tilt the camera dynamically using right-click-drag to inspect topography from valley floor to summit.
