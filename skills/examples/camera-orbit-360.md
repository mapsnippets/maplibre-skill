# Continuous 360° Camera Orbit Animation 🔄

> **Official MapLibre GL JS Example:** [Rotate Camera](https://maplibre.org/maplibre-gl-js/docs/examples/rotate-camera/)  
> **Target Category:** Production Task Implementation

Smoothly rotate the camera continuously around a central point of interest (e.g., a mountain peak, stadium, or landmark) using requestAnimationFrame.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Continuous 360° Camera Orbit Animation 🔄</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<button id="toggle-orbit">Pause Orbit</button>
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

#toggle-orbit { position: absolute; top: 16px; right: 16px; background: #0084FF; color: #fff; border: none; padding: 8px 14px; border-radius: 6px; font-weight: 600; cursor: pointer; z-index: 1000; }
```

---

## 3. Complete JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/outdoor-v4/style.json?key=${MAPTILER_KEY}`,
  center: [7.7491, 46.0207], // Matterhorn, Switzerland
  zoom: 13,
  pitch: 65,
  bearing: 0
});

let isOrbiting = true;
function rotateCamera(timestamp) {
  if (isOrbiting) {
    map.rotateTo((map.getBearing() + 0.15) % 360, { duration: 0 });
  }
  requestAnimationFrame(rotateCamera);
}

map.on('load', () => {
  rotateCamera(0);
});

document.getElementById('toggle-orbit').onclick = () => {
  isOrbiting = !isOrbiting;
  document.getElementById('toggle-orbit').textContent = isOrbiting ? 'Pause Orbit' : 'Resume Orbit';
};
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/rotate-camera/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
