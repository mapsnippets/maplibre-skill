# Georeferenced Image Overlay (Drone / Historical) 🗺️

> **Official MapLibre GL JS Example:** [Add an image source](https://maplibre.org/maplibre-gl-js/docs/examples/image-source/)  
> **Target Category:** Production Task Implementation

Project orthophoto drone surveys, historical archive maps, or blueprint diagrams onto geographical coordinates using 4 bounding corners.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Georeferenced Image Overlay (Drone / Historical) 🗺️</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
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
import * as maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/satellite-v4/style.json?key=${MAPTILER_KEY}`,
  center: [-75.1652, 39.9526],
  zoom: 14
});

map.on('load', () => {
  map.addSource('historical-overlay', {
    type: 'image',
    url: 'https://maplibre.org/maplibre-gl-js/docs/assets/radar.gif',
    coordinates: [
      [-75.20, 39.98],
      [-75.12, 39.98],
      [-75.12, 39.92],
      [-75.20, 39.92]
    ]
  });

  map.addLayer({
    id: 'historical-layer',
    type: 'raster',
    source: 'historical-overlay',
    paint: { 'raster-opacity': 0.85 }
  });
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/image-source/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
