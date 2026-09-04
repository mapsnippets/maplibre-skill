# Add Layer Below Labels (Sandwich Architecture)

> **Documentation Reference:** [Add Layer Below Labels (Sandwich Architecture)](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-new-layer-below-labels/)
> Category: **Data & Vector Styling**

## Overview
Inspects the style layer hierarchy and inserts custom vector polygon or heat layers directly below road/place labels.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Add Layer Below Labels</title>
  
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script type="module">
    import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs';
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = new maplibregl.Map({
      container: 'map',
      style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
      center: [-74.006, 40.7128], // New York
      zoom: 12
    });

    map.on('load', () => {
      // Find the first symbol layer (text label) in the style
      let firstLabelId;
      const layers = map.getStyle().layers;
      for (const layer of layers) {
        if (layer.type === 'symbol') {
          firstLabelId = layer.id;
          break;
        }
      }

      map.addSource('custom-zone', {
        type: 'geojson',
        data: {
          type: 'Feature',
          geometry: {
            type: 'Polygon',
            coordinates: [[
              [-74.02, 40.70], [-73.98, 40.70],
              [-73.98, 40.74], [-74.02, 40.74],
              [-74.02, 40.70]
            ]]
          }
        }
      });

      // Insert layer BEFORE the first symbol layer so street labels stay on top!
      map.addLayer({
        id: 'zone-fill',
        type: 'fill',
        source: 'custom-zone',
        paint: {
          'fill-color': '#0084FF',
          'fill-opacity': 0.35
        }
      }, firstLabelId);
    });
  </script>
</body>
</html>
```

## Key API Features
- Native MapLibre GL JS implementation.
- Modern MapTiler Planet v4 vector and raster styles.
- Self-contained and production-ready.
