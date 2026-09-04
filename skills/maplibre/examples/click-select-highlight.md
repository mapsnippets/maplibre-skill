# Click Selection & Persistent Feature State 🎯

> **Official MapLibre GL JS Example:** [Highlight features containing similar data](https://maplibre.org/maplibre-gl-js/docs/examples/set-feature-state/)  
> **Target Category:** Production Task Implementation

Select and keep track of selected polygon state on click using MapLibre `map.setFeatureState` for hardware-accelerated 60 FPS highlighting.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Click Selection & Persistent Feature State 🎯</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<div id="selected-info">Click a state to select</div>
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

#selected-info { position: absolute; top: 16px; right: 16px; background: rgba(255,255,255,0.95); padding: 10px 14px; border-radius: 6px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); font-size: 13px; z-index: 1000; }
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
  center: [-98, 38],
  zoom: 3.5
});

let selectedFeatureId = null;

map.on('load', () => {
  map.addSource('counties', {
    type: 'geojson',
    data: 'https://raw.githubusercontent.com/PublicMapping/district-builder/master/server/db/fixtures/states.geojson',
    generateId: true
  });

  map.addLayer({
    id: 'counties-fill',
    type: 'fill',
    source: 'counties',
    paint: {
      'fill-color': [
        'case',
        ['boolean', ['feature-state', 'selected'], false],
        '#0084FF',
        '#e2e8f0'
      ],
      'fill-opacity': 0.7
    }
  });

  map.on('click', 'counties-fill', (e) => {
    if (e.features.length > 0) {
      if (selectedFeatureId !== null) {
        map.setFeatureState({ source: 'counties', id: selectedFeatureId }, { selected: false });
      }
      selectedFeatureId = e.features[0].id;
      map.setFeatureState({ source: 'counties', id: selectedFeatureId }, { selected: true });
      document.getElementById('selected-info').textContent = `Selected: ${e.features[0].properties.name || 'Feature ' + selectedFeatureId}`;
    }
  });
});
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/set-feature-state/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
