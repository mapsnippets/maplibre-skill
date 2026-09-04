# Georeferenced Video Layer Overlay 🎥

> **Official MapLibre GL JS Example:** [Add a video source](https://maplibre.org/maplibre-gl-js/docs/examples/video-on-a-map/)  
> **Target Category:** Production Task Implementation

Drape live video feeds, meteorological loops, or simulated drone flight footage over 3D terrain using `video` layer sources.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Georeferenced Video Layer Overlay 🎥</title>
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
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: [-122.4194, 37.7749],
  zoom: 12
});

map.on('load', () => {
  map.addSource('video-source', {
    type: 'video',
    urls: [
      'https://maplibre.org/maplibre-gl-js/docs/assets/drone.mp4'
    ],
    coordinates: [
      [-122.515, 37.810],
      [-122.355, 37.810],
      [-122.355, 37.740],
      [-122.515, 37.740]
    ]
  });

  map.addLayer({
    id: 'video-layer',
    type: 'raster',
    source: 'video-source'
  });
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/video-on-a-map/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
