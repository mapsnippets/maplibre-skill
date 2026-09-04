# Interactive Distance Measurement with Turf.js

> Official Reference: [Interactive Distance Measurement with Turf.js](https://maplibre.org/maplibre-gl-js/docs/examples/measure/)
> Category: **Camera & Navigation**

## Overview
Interactive click-to-measure tool calculating accurate geodesic path lengths with Turf.js and drawing lines on MapLibre.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Turf.js Distance Measurement</title>
  
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <script src="https://cdn.jsdelivr.net/npm/@turf/turf@6.5.0/turf.min.js"></script>
  <style>
    body { margin: 0; padding: 0; font-family: sans-serif; }
    #map { width: 100vw; height: 100vh; }
    .distance-box {
      position: absolute; top: 16px; left: 16px; z-index: 1000;
      background: rgba(15, 23, 42, 0.9); color: #fff; padding: 12px 18px;
      border-radius: 8px; font-size: 14px;
    }
  </style>
</head>
<body>
  <div id="map"></div>
  <div class="distance-box">
    <strong>Distance:</strong> <span id="distance">0 km</span><br>
    <small>Click map to add measurement points</small>
  </div>
  <script type="module">
    import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs';
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = new maplibregl.Map({
      container: 'map',
      style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
      center: [14.4378, 50.0755], // Prague
      zoom: 12
    });

    const geojson = {
      type: 'FeatureCollection',
      features: []
    };

    map.on('load', () => {
      map.addSource('measure-geojson', { type: 'geojson', data: geojson });

      map.addLayer({
        id: 'measure-lines',
        type: 'line',
        source: 'measure-geojson',
        paint: { 'line-color': '#ff0055', 'line-width': 3, 'line-dasharray': [2, 2] }
      });

      map.addLayer({
        id: 'measure-points',
        type: 'circle',
        source: 'measure-geojson',
        paint: { 'circle-radius': 5, 'circle-color': '#ff0055' }
      });
    });

    map.on('click', (e) => {
      const point = turf.point([e.lngLat.lng, e.lngLat.lat]);
      geojson.features.push(point);

      if (geojson.features.length > 1) {
        const coords = geojson.features.map(f => f.geometry.coordinates);
        const line = turf.lineString(coords);
        const length = turf.length(line, { units: 'kilometers' });
        document.getElementById('distance').textContent = `${length.toFixed(2)} km`;
        geojson.features = [line, ...coords.map(c => turf.point(c))];
      }
      map.getSource('measure-geojson').setData(geojson);
    });
  </script>
</body>
</html>
```

## Key API Features
- Native MapLibre GL JS implementation.
- Modern MapTiler Planet v4 vector and raster styles.
- Self-contained and production-ready.
