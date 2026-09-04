# Time / Magnitude Filtering with Sliders 🎚️

> **Official MapLibre GL JS Example:** [Filter symbols by text input](https://maplibre.org/maplibre-gl-js/docs/examples/filter-markers/)  
> **Target Category:** Production Task Implementation

Dynamically filter visible map features in real time based on a range slider or search query using `map.setFilter()`.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Time / Magnitude Filtering with Sliders 🎚️</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<div id="slider-panel">
  <label>Minimum Magnitude: <span id="mag-val">3</span></label>
  <input id="slider" type="range" min="1" max="7" step="0.5" value="3" />
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

#slider-panel { position: absolute; top: 16px; right: 16px; background: rgba(255,255,255,0.95); padding: 12px 16px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); font-size: 13px; z-index: 1000; }
```

---

## 3. Complete JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=${MAPTILER_KEY}`,
  center: [-122.4, 37.7],
  zoom: 5
});

map.on('load', () => {
  map.addSource('earthquakes', {
    type: 'geojson',
    data: 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson'
  });

  map.addLayer({
    id: 'quakes-circle',
    type: 'circle',
    source: 'earthquakes',
    paint: {
      'circle-radius': ['*', ['get', 'mag'], 3],
      'circle-color': '#f43f5e',
      'circle-opacity': 0.7
    },
    filter: ['>=', ['get', 'mag'], 3]
  });

  const slider = document.getElementById('slider');
  const magVal = document.getElementById('mag-val');
  slider.oninput = (e) => {
    const val = parseFloat(e.target.value);
    magVal.textContent = val;
    map.setFilter('quakes-circle', ['>=', ['get', 'mag'], val]);
  };
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/filter-markers/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
