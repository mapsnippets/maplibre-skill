# OGC WMS Layer Integration

> **Documentation Reference:** [OGC WMS Layer Integration](https://maplibre.org/maplibre-gl-js/docs/examples/wms/)
> Category: **Raster, Canvas & Video Overlays**

## Overview
Integrates an external OGC Web Map Service (WMS) as a raster tile source overlaid onto MapTiler vector basemaps.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - OGC WMS Layer</title>
  
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
      center: [-98, 39],
      zoom: 4
    });

    map.on('load', () => {
      map.addSource('wms-radar', {
        type: 'raster',
        tiles: [
          'https://mesonet.agron.iastate.edu/cgi-bin/wms/nexrad/n0r.cgi?service=WMS&request=GetMap&layers=nexrad-n0r-900913&styles=&format=image/png&transparent=true&version=1.1.1&height=256&width=256&srs=EPSG:3857&bbox={bbox-epsg-3857}'
        ],
        tileSize: 256
      });

      map.addLayer({
        id: 'wms-radar-layer',
        type: 'raster',
        source: 'wms-radar',
        paint: { 'raster-opacity': 0.7 }
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
