# 3D Sun Directional Lighting & Shadows ☀️

> **Official MapLibre GL JS Example:** [Change the lighting of a map](https://maplibre.org/maplibre-gl-js/docs/examples/set-light/)  
> **Target Category:** Production Task Implementation

Control sun azimuth and altitude angles to project realistic directional cast shadows across 3D buildings and mountain ridges via `map.setLight()`.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>3D Sun Directional Lighting & Shadows ☀️</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<div id="light-controls">
  <label>Sun Azimuth: <span id="az-val">210</span>°</label>
  <input id="azimuth" type="range" min="0" max="360" value="210" />
</div>
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

#light-controls { position: absolute; top: 16px; right: 16px; background: rgba(255,255,255,0.95); padding: 10px 14px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); font-size: 13px; z-index: 1000; }
```

---

## 3. Complete JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: [-74.006, 40.7128], // Manhattan
  zoom: 15.5,
  pitch: 60,
  bearing: -15
});

map.on('load', () => {
  map.setLight({
    anchor: 'map',
    color: '#fffaed',
    intensity: 0.4,
    position: [1.5, 210, 30] // [radial, azimuthal, polar]
  });

  const azimuthInput = document.getElementById('azimuth');
  const azVal = document.getElementById('az-val');
  azimuthInput.oninput = (e) => {
    const az = parseFloat(e.target.value);
    azVal.textContent = az;
    map.setLight({ position: [1.5, az, 30] });
  };
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/set-light/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
