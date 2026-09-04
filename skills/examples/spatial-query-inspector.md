# Official Example: Feature Querying & Inspection (`queryRenderedFeatures`) 🔍📋

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/get-features-under-the-mouse-pointer/

This tutorial shows how to use `map.queryRenderedFeatures` to inspect visible vector and GeoJSON features beneath the mouse cursor, filter by layer names, and display dynamic property inspection cards.

---

## 1. HTML Setup with Inspector HUD

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Feature Inspector</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; font-family: system-ui, sans-serif; }
    #map { width: 100vw; height: 100vh; }
    .inspector-panel {
      position: absolute;
      bottom: 24px;
      left: 24px;
      z-index: 10;
      background: white;
      padding: 12px 16px;
      border-radius: 8px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.15);
      max-width: 320px;
      max-height: 240px;
      overflow-y: auto;
    }
    .inspector-panel h4 { margin: 0 0 6px; color: #0084FF; font-size: 15px; }
    .inspector-panel pre { margin: 0; font-size: 11px; background: #f8fafc; padding: 6px; border-radius: 4px; }
  </style>
</head>
<body>
  <div class="inspector-panel" id="inspector">
    <h4>Feature Inspector</h4>
    <div id="inspector-output">Hover or click any feature...</div>
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
  zoom: 14
});

const output = document.getElementById('inspector-output');

// 1. Query Rendered Features on Click
map.on('click', (e) => {
  // Query a small 6x6 pixel bounding box around the click coordinate
  const bbox = [
    [e.point.x - 3, e.point.y - 3],
    [e.point.x + 3, e.point.y + 3]
  ];

  const features = map.queryRenderedFeatures(bbox);

  if (!features.length) {
    output.innerHTML = 'No vector feature detected at click point.';
    return;
  }

  const topFeature = features[0];

  // Update HUD
  output.innerHTML = `
    <b>Layer:</b> <code>${topFeature.layer.id}</code><br/>
    <b>Source Layer:</b> <code>${topFeature.sourceLayer || 'N/A'}</code><br/>
    <b>Geometry:</b> ${topFeature.geometry.type}<br/>
    <b>Properties:</b>
    <pre>${JSON.stringify(topFeature.properties, null, 2)}</pre>
  `;

  // Display map popup at coordinate
  new maplibregl.Popup()
    .setLngLat(e.lngLat)
    .setHTML(`
      <div style="font-family: system-ui; font-size: 12px;">
        <strong style="color: #0084FF;">${topFeature.properties.name || topFeature.layer.id}</strong><br/>
        <span>Type: ${topFeature.geometry.type}</span>
      </div>
    `)
    .addTo(map);
});

// 2. Change cursor to pointer when hovering over interactive vector layers
map.on('mousemove', (e) => {
  const features = map.queryRenderedFeatures(e.point);
  map.getCanvas().style.cursor = features.length ? 'pointer' : '';
});
```
