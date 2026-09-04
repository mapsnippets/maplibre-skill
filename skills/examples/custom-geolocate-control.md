# High-Accuracy GPS Geolocation with Heading

> Official Reference: [High-Accuracy GPS Geolocation with Heading](https://maplibre.org/maplibre-gl-js/docs/examples/locate-user/)
> Category: **Camera & Navigation**

## Overview
Integrates `maplibregl.GeolocateControl` with continuous GPS position tracking and device orientation heading.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Geolocate Control</title>
  
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
      center: [0, 0],
      zoom: 2
    });

    const geolocate = new maplibregl.GeolocateControl({
      positionOptions: { enableHighAccuracy: true },
      trackUserLocation: true,
      showUserHeading: true
    });

    map.addControl(geolocate, 'top-right');
  </script>
</body>
</html>
```

## Key API Features
- Native MapLibre GL JS implementation.
- Modern MapTiler Planet v4 vector and raster styles.
- Self-contained and production-ready.
