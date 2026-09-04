# Fit Viewport Bounds with UI Padding 📐

> **Official MapLibre GL JS Example:** [Fit a map to a bounding box](https://maplibre.org/maplibre-gl-js/docs/examples/fitbounds/)  
> **Target Category:** Production Task Implementation

Calculate the bounding box of multiple locations or routes and fit the camera snugly with asymmetric padding so pins are not obscured by overlay sidebars.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Fit Viewport Bounds with UI Padding 📐</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<button id="fit-btn">Fit All Locations</button>
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

#fit-btn { position: absolute; top: 16px; left: 16px; background: #0084FF; color: #fff; border: none; padding: 10px 16px; border-radius: 6px; font-weight: 600; cursor: pointer; z-index: 1000; }
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
  zoom: 10
});

const coordinates = [
  [14.35, 50.05],
  [14.52, 50.04],
  [14.48, 50.12],
  [14.38, 50.11]
];

map.on('load', () => {
  coordinates.forEach((coord) => {
    new maplibregl.Marker({ color: '#0084FF' }).setLngLat(coord).addTo(map);
  });
});

document.getElementById('fit-btn').onclick = () => {
  const bounds = coordinates.reduce((b, coord) => b.extend(coord), new maplibregl.LngLatBounds(coordinates[0], coordinates[0]));
  map.fitBounds(bounds, {
    padding: { top: 60, bottom: 60, left: 100, right: 60 },
    duration: 1200,
    maxZoom: 14
  });
};
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/fitbounds/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
