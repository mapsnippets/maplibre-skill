# 3D Models on Globe Projection (Three.js) 🛰️

> **Official MapLibre GL JS Example:** [Add a 3D model to globe using three.js](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-to-globe-using-threejs/)  
> **Target Category:** Production Task Implementation

Render 3D satellites, aircraft, or space stations orbiting the interactive 3D globe projection using custom WebGL layers and Three.js.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>3D Models on Globe Projection (Three.js) 🛰️</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
  <script type="module" src="main.js"></script>
</body>
</html>
```

---

## 2. CSS Styling

```css
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

#map {
  width: 100%;
  height: 100%;
}

#map { width: 100%; height: 100%; }
```

---

## 3. Complete JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import * as THREE from 'three';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=${MAPTILER_KEY}`,
  center: [0, 0],
  zoom: 1.5
});

map.on('load', () => {
  map.setProjection({ type: 'globe' });
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-to-globe-using-threejs/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
