# Vector Contour Lines with Elevation Labels

> Official Reference: [Vector Contour Lines with Elevation Labels](https://maplibre.org/maplibre-gl-js/docs/examples/add-contour-lines/)
> Category: **Data & Vector Styling**

## Overview
Adds vector contour lines from MapTiler Contours tileset with dynamic line elevation labels and index contour emphasis.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre GL JS - Vector Contour Lines</title>
  <script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = new maplibregl.Map({
      container: 'map',
      style: `https://api.maptiler.com/maps/outdoor-v4/style.json?key=${MAPTILER_KEY}`,
      center: [7.9838, 46.5475], // Jungfrau, Swiss Alps
      zoom: 13,
      pitch: 45
    });

    map.on('load', () => {
      map.addSource('contours', {
        type: 'vector',
        url: `https://api.maptiler.com/tiles/contours/tiles.json?key=${MAPTILER_KEY}`
      });

      // Regular contour lines
      map.addLayer({
        id: 'contour-lines',
        type: 'line',
        source: 'contours',
        'source-layer': 'contour',
        paint: {
          'line-color': '#b45309',
          'line-width': ['case', ['==', ['%', ['get', 'height'], 100], 0], 1.8, 0.8],
          'line-opacity': 0.7
        }
      });

      // Elevation labels on index contours
      map.addLayer({
        id: 'contour-labels',
        type: 'symbol',
        source: 'contours',
        'source-layer': 'contour',
        filter: ['==', ['%', ['get', 'height'], 100], 0],
        layout: {
          'symbol-placement': 'line',
          'text-field': ['concat', ['get', 'height'], 'm'],
          'text-size': 11,
          'text-font': ['Noto Sans Regular']
        },
        paint: {
          'text-color': '#78350f',
          'text-halo-color': '#ffffff',
          'text-halo-width': 1.5
        }
      });
    });
  </script>
</body>
</html>
```

## Key API Features
- Native MapLibre GL JS implementation.
- Modern MapTiler Planet v4 vector and raster styles.
- Self-contained and production-ready.
