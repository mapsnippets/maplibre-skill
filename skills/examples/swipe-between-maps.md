# Split-Screen Swipe Comparison Slider 🪟

> **Official MapLibre GL JS Example:** [Swipe between maps](https://maplibre.org/maplibre-gl-js/docs/examples/mapbox-gl-compare/)  
> **Target Category:** Production Task Implementation

Synchronize two side-by-side MapLibre map instances with an interactive vertical wiper to compare satellite imagery with topographic street layers.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Split-Screen Swipe Comparison Slider 🪟</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="comparison-container">
  <div id="before" class="map"></div>
  <div id="after" class="map"></div>
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

#comparison-container { position: relative; width: 100%; height: 100%; }
.map { position: absolute; top: 0; bottom: 0; width: 100%; }
```

---

## 3. Complete JavaScript Implementation

```javascript
import * as maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const beforeMap = new maplibregl.Map({
  container: 'before',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: [14.4378, 50.0755],
  zoom: 14
});

const afterMap = new maplibregl.Map({
  container: 'after',
  style: `https://api.maptiler.com/maps/satellite-v4/style.json?key=${MAPTILER_KEY}`,
  center: [14.4378, 50.0755],
  zoom: 14
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/mapbox-gl-compare/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
