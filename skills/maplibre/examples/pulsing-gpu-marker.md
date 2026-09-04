# Pulsing Radar Marker (Canvas / GPU Animation)

> **Documentation Reference:** [Pulsing Radar Marker (Canvas / GPU Animation)](https://maplibre.org/maplibre-gl-js/docs/examples/add-an-animated-icon-to-the-map/)
> Category: **Points, Clusters & UI**

## Overview
Renders a high-performance animated radar pulse beacon using a dynamic Canvas `map.addImage` implementation rendered at 60 FPS.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Animated Pulsing Radar Icon</title>
  
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
      center: [2.3522, 48.8566],
      zoom: 12
    });

    const size = 150;
    const pulsingDot = {
      width: size,
      height: size,
      data: new Uint8Array(size * size * 4),
      onAdd: function () {
        const canvas = document.createElement('canvas');
        canvas.width = this.width;
        canvas.height = this.height;
        this.context = canvas.getContext('2d');
      },
      render: function () {
        const duration = 1500;
        const t = (performance.now() % duration) / duration;
        const radius = (size / 2) * 0.3;
        const outerRadius = (size / 2) * 0.7 * t + radius;
        const ctx = this.context;

        ctx.clearRect(0, 0, this.width, this.height);
        // Outer pulsing ring
        ctx.beginPath();
        ctx.arc(this.width / 2, this.height / 2, outerRadius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(0, 210, 255, ${1 - t})`;
        ctx.fill();

        // Inner solid circle
        ctx.beginPath();
        ctx.arc(this.width / 2, this.height / 2, radius, 0, Math.PI * 2);
        ctx.fillStyle = '#0084FF';
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 2;
        ctx.fill();
        ctx.stroke();

        this.data = ctx.getImageData(0, 0, this.width, this.height).data;
        map.triggerRepaint();
        return true;
      }
    };

    map.on('load', () => {
      map.addImage('pulsing-dot', pulsingDot, { pixelRatio: 2 });
      map.addSource('dot-point', {
        type: 'geojson',
        data: {
          type: 'FeatureCollection',
          features: [{ type: 'Feature', geometry: { type: 'Point', coordinates: [2.3522, 48.8566] } }]
        }
      });
      map.addLayer({
        id: 'layer-dot',
        type: 'symbol',
        source: 'dot-point',
        layout: { 'icon-image': 'pulsing-dot' }
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
