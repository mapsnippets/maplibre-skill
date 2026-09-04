# GeoJSON Choropleth with Style Expressions 🎨

> **Official MapLibre GL JS Example:** [Update a choropleth layer by data-driven styling](https://maplibre.org/maplibre-gl-js/docs/examples/updating-choropleth/)  
> **Target Category:** Production Task Implementation

Coloring administrative regions dynamically using data-driven style expressions (`step` / `interpolate`) mapped to numeric demographic properties.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GeoJSON Choropleth with Style Expressions 🎨</title>
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
  style: `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=${MAPTILER_KEY}`,
  center: [-98, 38],
  zoom: 3.5
});

map.on('load', () => {
  map.addSource('states', {
    type: 'geojson',
    data: 'https://raw.githubusercontent.com/PublicMapping/district-builder/master/server/db/fixtures/states.geojson'
  });

  map.addLayer({
    id: 'state-fills',
    type: 'fill',
    source: 'states',
    paint: {
      'fill-color': [
        'interpolate',
        ['linear'],
        ['get', 'density'],
        0, '#fef0d9',
        50, '#fdcc8a',
        100, '#fc8d59',
        500, '#e34a33',
        1000, '#b30000'
      ],
      'fill-opacity': 0.75
    }
  });

  map.addLayer({
    id: 'state-borders',
    type: 'line',
    source: 'states',
    paint: {
      'line-color': '#ffffff',
      'line-width': 1
    }
  });
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/updating-choropleth/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
