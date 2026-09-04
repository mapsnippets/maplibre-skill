# Multidirectional Hillshade with Terrain-RGB

> Official Reference: [Multidirectional Hillshade with Terrain-RGB](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-multidirectional-hillshade-layer/)
> Category: **3D Terrain, Buildings & Elevation**

## Overview
Configures multidirectional hillshading illuminating terrain slopes from multiple sun angles simultaneously for crisp cartographic relief.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Multidirectional Hillshade</title>
  
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
      style: `https://api.maptiler.com/maps/dataviz-v4-dark/style.json?key=${MAPTILER_KEY}`,
      center: [7.7491, 46.0207], // Matterhorn
      zoom: 12,
      pitch: 45
    });

    map.on('load', () => {
      map.addSource('terrain-source', {
        type: 'raster-dem',
        url: `https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=${MAPTILER_KEY}`,
        tileSize: 512
      });

      map.addLayer({
        id: 'multi-hillshade',
        type: 'hillshade',
        source: 'terrain-source',
        paint: {
          'hillshade-method': 'multidirectional',
          'hillshade-exaggeration': 0.8,
          'hillshade-shadow-color': '#0f172a',
          'hillshade-highlight-color': '#38bdf8',
          'hillshade-accent-color': '#0284c7'
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
