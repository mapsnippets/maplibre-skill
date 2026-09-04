# Recipe: Custom HTML Markers & CSS Radar Beacons 📍📡

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/add-custom-icons-with-markers/

This tutorial shows how to construct interactive DOM HTML markers using `maplibregl.Marker`, attach animated CSS pulsating radar rings, and bind customized popups.

---

## 1. HTML Setup with CSS Pulse Animation

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Custom HTML Markers</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }

    /* Pulsing Radar Ring Keyframe */
    .radar-marker {
      position: relative;
      width: 24px;
      height: 24px;
      cursor: pointer;
    }
    .radar-marker .ring {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: rgba(0, 132, 255, 0.4);
      animation: radar-pulse 2s cubic-bezier(0.2, 0.6, 0.35, 1) infinite;
    }
    .radar-marker .core {
      position: absolute;
      top: 6px;
      left: 6px;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: #0084FF;
      border: 2px solid white;
      box-shadow: 0 0 8px rgba(0, 132, 255, 0.8);
    }
    @keyframes radar-pulse {
      0% { transform: scale(0.6); opacity: 1; }
      100% { transform: scale(2.4); opacity: 0; }
    }
  </style>
</head>
<body>
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
  zoom: 13
});

// Helper function to create animated radar DOM element
function createRadarElement() {
  const el = document.createElement('div');
  el.className = 'radar-marker';
  el.innerHTML = '<div class="ring"></div><div class="core"></div>';
  return el;
}

// 1. Add Radar Beacon Marker
const markerA = new maplibregl.Marker({
  element: createRadarElement(),
  anchor: 'center'
})
  .setLngLat([14.42076, 50.08804])
  .setPopup(
    new maplibregl.Popup({ offset: 15 }).setHTML(`
      <div style="font-family: system-ui; padding: 4px;">
        <h4 style="margin: 0 0 4px; color: #0084FF;">Prague Telemetry Node</h4>
        <p style="margin: 0; font-size: 13px; color: #475569;">Real-time radar sensor online.</p>
      </div>
    `)
  )
  .addTo(map);

// 2. Add Draggable Standard SVG Marker
const draggableMarker = new maplibregl.Marker({
  color: '#FF6B00',
  draggable: true
})
  .setLngLat([14.435, 50.082])
  .addTo(map);

draggableMarker.on('dragend', () => {
  const lngLat = draggableMarker.getLngLat();
  console.log('Marker moved to:', lngLat.lng, lngLat.lat);
});
```
