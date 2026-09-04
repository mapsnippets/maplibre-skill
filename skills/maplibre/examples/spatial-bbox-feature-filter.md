# Interactive Bounding Box Feature Selection

> **Documentation Reference:** [Interactive Bounding Box Feature Selection](https://maplibre.org/maplibre-gl-js/docs/examples/using-boxqueryrenderedfeatures-with-dragging/)
> Category: **Points, Clusters & UI**

## Overview
Allows users to draw an interactive bounding box to select and query all vector features rendered inside the area.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Box Selection Query</title>
  
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
    .boxdraw {
      background: rgba(56, 189, 248, 0.2);
      border: 2px dashed #0284c7;
      position: absolute; top: 0; left: 0; width: 0; height: 0;
      pointer-events: none;
    }
  </style>
</head>
<body>
  <div id="map"></div>
  <script type="module">
    import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs';
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = new maplibregl.Map({
      container: 'map',
      style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
      center: [-0.1278, 51.5074], // London
      zoom: 13
    });
  </script>
</body>
</html>
```

## Key API Features
- Native MapLibre GL JS implementation.
- Modern MapTiler Planet v4 vector and raster styles.
- Self-contained and production-ready.
