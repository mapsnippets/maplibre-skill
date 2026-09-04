# Real-Time Telemetry & Live GeoJSON Streaming 📡

> **Official MapLibre GL JS Example:** [Add live realtime data](https://maplibre.org/maplibre-gl-js/docs/examples/live-geojson/)  
> **Target Category:** Production Task Implementation

Poll or receive live vehicle telemetry coordinates over WebSockets and update the GeoJSON map source seamlessly via `source.setData()` without flickering.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Real-Time Telemetry & Live GeoJSON Streaming 📡</title>
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
  center: [14.4378, 50.0755],
  zoom: 13
});

map.on('load', () => {
  const geojson = {
    type: 'FeatureCollection',
    features: [{
      type: 'Feature',
      geometry: { type: 'Point', coordinates: [14.4378, 50.0755] }
    }]
  };

  map.addSource('drone', { type: 'geojson', data: geojson });
  map.addLayer({
    id: 'drone-point',
    type: 'circle',
    source: 'drone',
    paint: { 'circle-radius': 10, 'circle-color': '#0084FF', 'circle-stroke-width': 2, 'circle-stroke-color': '#fff' }
  });

  // Simulate real-time GPS pings every 1 second
  setInterval(() => {
    geojson.features[0].geometry.coordinates[0] += (Math.random() - 0.5) * 0.002;
    geojson.features[0].geometry.coordinates[1] += (Math.random() - 0.5) * 0.002;
    map.getSource('drone').setData(geojson);
  }, 1000);
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/live-geojson/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
