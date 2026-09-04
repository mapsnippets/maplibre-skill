# Procedural HTML5 Canvas Layer Source 🎨

> **Official MapLibre GL JS Example:** [Add a canvas source](https://maplibre.org/maplibre-gl-js/docs/examples/canvas-source/)  
> **Target Category:** Production Task Implementation

Draw real-time animated charts, radar simulations, or particles on an HTML5 canvas and project it into MapLibre as a georeferenced raster layer.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Procedural HTML5 Canvas Layer Source 🎨</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<canvas id="canvas" width="400" height="400" style="display: none;"></canvas>
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
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: [-122.4194, 37.7749],
  zoom: 11
});

map.on('load', () => {
  map.addSource('canvas-source', {
    type: 'canvas',
    canvas: 'canvas',
    coordinates: [
      [-122.514, 37.805],
      [-122.355, 37.805],
      [-122.355, 37.744],
      [-122.514, 37.744]
    ],
    animate: true
  });

  map.addLayer({
    id: 'canvas-layer',
    type: 'raster',
    source: 'canvas-source'
  });

  let angle = 0;
  function draw() {
    ctx.clearRect(0, 0, 400, 400);
    ctx.beginPath();
    ctx.arc(200, 200, 80, 0, Math.PI * 2);
    ctx.fillStyle = 'rgba(0, 132, 255, 0.4)';
    ctx.fill();
    ctx.strokeStyle = '#0084FF';
    ctx.lineWidth = 4;
    ctx.stroke();
    requestAnimationFrame(draw);
  }
  draw();
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/canvas-source/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
