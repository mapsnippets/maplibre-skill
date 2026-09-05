# Split-Screen Swipe Comparison Slider 🪟

> **Documentation Reference:** [Swipe between maps](https://maplibre.org/maplibre-gl-js/docs/examples/mapbox-gl-compare/)  
> **Target Category:** Production Task Implementation

Synchronize two side-by-side MapLibre map instances with an interactive vertical wiper to compare satellite imagery with topographic street layers using `@maplibre/maplibre-gl-compare`.

---

## 1. HTML Container & Dependencies

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Split-Screen Swipe Comparison Slider 🪟</title>
  
  <!-- MapLibre GL JS -->
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
  
  <!-- MapLibre GL Compare Plugin -->
  <link rel="stylesheet" href="https://unpkg.com/@maplibre/maplibre-gl-compare@0.5.0/dist/maplibre-gl-compare.css" />
  <script src="https://unpkg.com/@maplibre/maplibre-gl-compare@0.5.0/dist/maplibre-gl-compare.js"></script>

  <style>
    body { margin: 0; padding: 0; overflow: hidden; font-family: sans-serif; }
    #comparison-container {
      position: relative;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
    }
    .map {
      position: absolute;
      top: 0;
      bottom: 0;
      width: 100%;
    }
  </style>
</head>
<body>
  <div id="comparison-container">
    <div id="before" class="map"></div>
    <div id="after" class="map"></div>
  </div>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

### Standalone CDN Usage:
```javascript
const KEY = 'YOUR_MAPTILER_API_KEY';

// 1. Initialize 'before' map (Street Basemap)
const beforeMap = new maplibregl.Map({
  container: 'before',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${KEY}`,
  center: [6.8656, 45.8326], // Mont Blanc
  zoom: 11
});

// 2. Initialize 'after' map (Satellite Imagery)
const afterMap = new maplibregl.Map({
  container: 'after',
  style: `https://api.maptiler.com/maps/satellite-v4/style.json?key=${KEY}`,
  center: [6.8656, 45.8326],
  zoom: 11
});

// 3. Instantiate the Compare slider on the container
const compare = new maplibregl.Compare(beforeMap, afterMap, '#comparison-container', {
  mousemove: false,        // Set true to swipe on hover
  orientation: 'vertical'  // 'vertical' divider or 'horizontal' divider
});
```

### Modern ESM / Bundler Usage:
```javascript
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';
import Compare from '@maplibre/maplibre-gl-compare';
import '@maplibre/maplibre-gl-compare/dist/maplibre-gl-compare.css';

const KEY = 'YOUR_MAPTILER_API_KEY';

const beforeMap = new maplibregl.Map({
  container: 'before',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${KEY}`,
  center: [6.8656, 45.8326],
  zoom: 11
});

const afterMap = new maplibregl.Map({
  container: 'after',
  style: `https://api.maptiler.com/maps/satellite-v4/style.json?key=${KEY}`,
  center: [6.8656, 45.8326],
  zoom: 11
});

const compare = new Compare(beforeMap, afterMap, '#comparison-container', {
  mousemove: false,
  orientation: 'vertical'
});
```

---

## 3. Key Architecture & Critical Invariants

| Parameter / Feature | Invariant Requirement |
| :--- | :--- |
| **Container CSS** | `#comparison-container` **must** have `position: relative; overflow: hidden;` and explicit width/height (e.g. `100vw; 100vh;`). |
| **Map Layer CSS** | Both child `.map` containers **must** have `position: absolute; top: 0; bottom: 0; width: 100%;` so MapLibre can clip the top canvas properly. |
| **Compare Instantiation** | `new maplibregl.Compare(before, after, container, options)` manages dual camera synchronization via `@mapbox/mapbox-gl-sync-move` under the hood. |
| **Cleanup on Unmount** | Call `compare.remove()` followed by `beforeMap.remove()` and `afterMap.remove()` to free WebGL contexts in SPAs. |

