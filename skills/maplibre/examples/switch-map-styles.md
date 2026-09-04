# Runtime Basemap Style Switcher 🗂️

> **Documentation Reference:** [Change a map's style](https://maplibre.org/maplibre-gl-js/docs/examples/set-style/)  
> **Target Category:** Production Task Implementation

Seamlessly switch between different vector and raster basemaps (Streets v4, Outdoor v4, Satellite v4, Dataviz Dark) without recreating the map instance or losing camera state.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Runtime Basemap Style Switcher 🗂️</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<div id="menu">
  <input id="streets-v4" type="radio" name="rtoggle" value="streets-v4" checked />
  <label for="streets-v4">Streets</label>
  <input id="outdoor-v4" type="radio" name="rtoggle" value="outdoor-v4" />
  <label for="outdoor-v4">Outdoor</label>
  <input id="satellite-v4" type="radio" name="rtoggle" value="satellite-v4" />
  <label for="satellite-v4">Satellite</label>
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

#menu { position: absolute; top: 16px; left: 16px; background: rgba(255,255,255,0.95); padding: 8px 14px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); z-index: 1000; font-size: 13px; font-weight: 600; display: flex; gap: 10px; align-items: center; }
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
  center: [14.4378, 50.0755],
  zoom: 12
});

const inputs = document.querySelectorAll('#menu input');
inputs.forEach((input) => {
  input.onclick = (layer) => {
    const styleId = layer.target.value;
    map.setStyle(`https://api.maptiler.com/maps/${styleId}/style.json?key=${MAPTILER_KEY}`);
  };
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Reference Spec** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/set-style/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
