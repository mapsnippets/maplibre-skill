# Recipe: Camera Navigation & Animations (`flyTo`) ✈️🎥

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/fly-to-a-location/

This guide demonstrates cinematic camera flight transitions, smooth easing, camera pitch/bearing tilt, and configuring vanishing point padding for sidebar layouts.

---

## 1. HTML Setup with Flight HUD

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Camera FlyTo</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; font-family: system-ui; }
    #map { width: 100vw; height: 100vh; }
    .flight-hud {
      position: absolute;
      top: 16px;
      left: 16px;
      z-index: 10;
      background: rgba(255, 255, 255, 0.95);
      padding: 12px;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      display: flex;
      gap: 8px;
    }
    .flight-hud button {
      background: #0084FF;
      color: white;
      border: none;
      padding: 8px 14px;
      border-radius: 4px;
      font-weight: 600;
      cursor: pointer;
    }
    .flight-hud button:hover { background: #0066cc; }
  </style>
</head>
<body>
  <div class="flight-hud">
    <button id="btn-london">London (3D Tilt)</button>
    <button id="btn-prague">Prague (Overhead)</button>
    <button id="btn-tokyo">Tokyo (Cinematic Arc)</button>
  </div>
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
  zoom: 11
});

// 1. London with 3D Pitch and Bearing
document.getElementById('btn-london').addEventListener('click', () => {
  map.flyTo({
    center: [-0.1276, 51.5074],
    zoom: 14.5,
    pitch: 55,        // Tilt camera for 3D perspective
    bearing: 45,      // Rotate azimuth clockwise
    speed: 1.2,       // Flight speed multiplier
    curve: 1.42,      // Zoom-out flight curve factor
    essential: true   // Runs even if prefers-reduced-motion is active
  });
});

// 2. Prague Top-Down
document.getElementById('btn-prague').addEventListener('click', () => {
  map.flyTo({
    center: [14.42076, 50.08804],
    zoom: 13,
    pitch: 0,
    bearing: 0,
    speed: 1.0
  });
});

// 3. Tokyo Long-Distance Flight with Padding Offset
document.getElementById('btn-tokyo').addEventListener('click', () => {
  map.flyTo({
    center: [139.6917, 35.6895],
    zoom: 12,
    pitch: 40,
    bearing: -20,
    speed: 1.5,
    // Reserve margin for an on-screen side drawer
    padding: { top: 40, bottom: 40, left: 320, right: 40 }
  });
});
```
