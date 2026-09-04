# Recipe: Create & Style Clusters in MapLibre 📍✨

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/create-and-style-clusters/

This tutorial shows how to configure native GeoJSON point clustering in MapLibre GL JS, style cluster bubbles using step functions, display count labels, render unclustered features, and zoom to cluster bounds on click.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Point Clustering</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script type="module" src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import * as maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
  center: [-103.59, 40.66],
  zoom: 3
});

map.on('load', () => {
  // 1. Add Clustered GeoJSON Source
  map.addSource('earthquakes', {
    type: 'geojson',
    data: 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson',
    cluster: true,
    clusterMaxZoom: 14, // Max zoom to cluster points on
    clusterRadius: 50    // Radius of each cluster in pixels
  });

  // 2. Clustered Circles Layer with Step Expressions
  map.addLayer({
    id: 'clusters',
    type: 'circle',
    source: 'earthquakes',
    filter: ['has', 'point_count'],
    paint: {
      // Color ramp: Cyan (<100), Blue (100-750), Pink (>750)
      'circle-color': [
        'step',
        ['get', 'point_count'],
        '#00D2FF',
        100, '#0084FF',
        750, '#f43f5e'
      ],
      // Radius ramp: 20px (<100), 30px (100-750), 40px (>750)
      'circle-radius': [
        'step',
        ['get', 'point_count'],
        20,
        100, 30,
        750, 40
      ],
      'circle-stroke-width': 2,
      'circle-stroke-color': '#ffffff'
    }
  });

  // 3. Cluster Count Number Labels
  map.addLayer({
    id: 'cluster-count',
    type: 'symbol',
    source: 'earthquakes',
    filter: ['has', 'point_count'],
    layout: {
      'text-field': '{point_count_abbreviated}',
      'text-font': ['Open Sans Bold', 'Arial Unicode MS Bold'],
      'text-size': 12
    },
    paint: {
      'text-color': '#ffffff'
    }
  });

  // 4. Unclustered Individual Points
  map.addLayer({
    id: 'unclustered-point',
    type: 'circle',
    source: 'earthquakes',
    filter: ['!', ['has', 'point_count']],
    paint: {
      'circle-color': '#FF6B00',
      'circle-radius': 6,
      'circle-stroke-width': 1.5,
      'circle-stroke-color': '#ffffff'
    }
  });

  // 5. Click on Cluster to Zoom In
  map.on('click', 'clusters', async (e) => {
    const features = map.queryRenderedFeatures(e.point, { layers: ['clusters'] });
    const clusterId = features[0].properties.cluster_id;

    const zoom = await map.getSource('earthquakes').getClusterExpansionZoom(clusterId);
    map.easeTo({
      center: features[0].geometry.coordinates,
      zoom: zoom
    });
  });

  // 6. Click on Individual Feature to Show Popup
  map.on('click', 'unclustered-point', (e) => {
    const coords = e.features[0].geometry.coordinates.slice();
    const mag = e.features[0].properties.mag;

    new maplibregl.Popup()
      .setLngLat(coords)
      .setHTML(`<b>Earthquake</b><br/>Magnitude: ${mag}`)
      .addTo(map);
  });

  // Pointer Cursor Interactions
  map.on('mouseenter', 'clusters', () => { map.getCanvas().style.cursor = 'pointer'; });
  map.on('mouseleave', 'clusters', () => { map.getCanvas().style.cursor = ''; });
});
```
