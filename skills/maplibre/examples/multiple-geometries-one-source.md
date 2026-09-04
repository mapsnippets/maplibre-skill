# Multiple Geometries from One GeoJSON Source

> Official Reference: [Multiple Geometries from One GeoJSON Source](https://maplibre.org/maplibre-gl-js/docs/examples/add-multiple-geometries-from-one-geojson-source/)
> Category: **Data & Vector Styling**

## Overview
Renders mixed geometries (Points, Lines, Polygons) from a single GeoJSON source using type-specific filter expressions.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Multiple Geometries One Source</title>
  
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
      center: [-122.4194, 37.7749], // San Francisco
      zoom: 12
    });

    map.on('load', () => {
      map.addSource('mixed-data', {
        type: 'geojson',
        data: {
          type: 'FeatureCollection',
          features: [
            {
              type: 'Feature',
              properties: { kind: 'area' },
              geometry: {
                type: 'Polygon',
                coordinates: [[[-122.45, 37.75], [-122.41, 37.75], [-122.41, 37.78], [-122.45, 37.78], [-122.45, 37.75]]]
              }
            },
            {
              type: 'Feature',
              properties: { kind: 'track' },
              geometry: {
                type: 'LineString',
                coordinates: [[-122.48, 37.76], [-122.40, 37.77]]
              }
            },
            {
              type: 'Feature',
              properties: { kind: 'poi' },
              geometry: { type: 'Point', coordinates: [-122.4194, 37.7749] }
            }
          ]
        }
      });

      // Polygon fill layer
      map.addLayer({
        id: 'polygon-layer',
        type: 'fill',
        source: 'mixed-data',
        filter: ['==', '$type', 'Polygon'],
        paint: { 'fill-color': '#0084FF', 'fill-opacity': 0.3 }
      });

      // Line layer
      map.addLayer({
        id: 'line-layer',
        type: 'line',
        source: 'mixed-data',
        filter: ['==', '$type', 'LineString'],
        paint: { 'line-color': '#00D2FF', 'line-width': 4 }
      });

      // Point circle layer
      map.addLayer({
        id: 'point-layer',
        type: 'circle',
        source: 'mixed-data',
        filter: ['==', '$type', 'Point'],
        paint: { 'circle-color': '#ff3366', 'circle-radius': 8, 'circle-stroke-width': 2, 'circle-stroke-color': '#fff' }
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
