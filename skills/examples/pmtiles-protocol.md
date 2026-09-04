# Official Example: PMTiles Source & Protocol (`addProtocol`) 📦⚡

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/pmtiles-source-and-protocol/

This tutorial shows how to extend MapLibre GL JS with custom streaming protocols using `maplibregl.addProtocol` to load cloud-native PMTiles vector archives directly from HTTP range requests without requiring tile server backends.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre PMTiles Protocol</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
  <script src="https://unpkg.com/pmtiles@3.0.6/dist/pmtiles.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import { Protocol } from 'pmtiles';
import 'maplibre-gl/dist/maplibre-gl.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

// 1. Register the PMTiles Protocol Handler Globally
const protocol = new Protocol();
maplibregl.addProtocol('pmtiles', protocol.tile);

// 2. Initialize Map with MapTiler Streets v4 as backdrop
const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
  center: [0, 20],
  zoom: 2
});

map.on('load', () => {
  // 3. Add Vector Tile Source using the custom 'pmtiles://' URI scheme
  map.addSource('protomaps-vector', {
    type: 'vector',
    url: 'pmtiles://https://r2-public.protomaps.com/protomaps-sample-datasets/nz-monuments.pmtiles'
  });

  // 4. Add Visual Layer styled from the PMTiles vector sublayer
  map.addLayer({
    id: 'monuments-circle',
    type: 'circle',
    source: 'protomaps-vector',
    'source-layer': 'monuments',
    paint: {
      'circle-color': '#0084FF',
      'circle-radius': 5,
      'circle-stroke-width': 1.5,
      'circle-stroke-color': '#ffffff'
    }
  });

  // 5. Interactive Click Inspection
  map.on('click', 'monuments-circle', (e) => {
    const props = e.features[0].properties;
    new maplibregl.Popup()
      .setLngLat(e.lngLat)
      .setHTML(`<b>${props.name || 'Historic Monument'}</b>`)
      .addTo(map);
  });
});
```
