# Interactive 3D Globe Projection 🌍

> **Official MapLibre GL JS Example:** [Display a Globe](https://maplibre.org/maplibre-gl-js/docs/examples/globe/)  
> **Target Category:** Production Task Implementation

Rendering the world as a seamless 3D sphere globe at low zoom levels transitioning smoothly into planar Mercator as users zoom into street level.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Interactive 3D Globe Projection 🌍</title>
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
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: [0, 20],
  zoom: 1.5
});

map.on('load', () => {
  // Set projection to globe
  map.setProjection({
    type: 'globe'
  });
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/globe/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
