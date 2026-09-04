# MapLibre GL JS — Official Examples & Architectural Recipes 📚🛠️

> Production-ready code recipes and implementation patterns representing the most essential MapLibre GL JS examples, covering camera physics, dynamic data feeds, data-driven shader styling, 3D terrain, and spatial interactivity.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Camera Animations

### A. Animate a Point Along a Route

Smoothly animates a vehicle or marker along a multi-vertex GeoJSON `LineString`:

```javascript
import maplibregl from 'maplibre-gl';

const routeCoordinates = [
  [14.4207, 50.0880],
  [14.4250, 50.0850],
  [14.4300, 50.0820],
  [14.4378, 50.0755]
];

// Add point source
map.addSource('moving-vehicle', {
  type: 'geojson',
  data: {
    type: 'Feature',
    geometry: { type: 'Point', coordinates: routeCoordinates[0] }
  }
});

map.addLayer({
  id: 'vehicle-point',
  type: 'circle',
  source: 'moving-vehicle',
  paint: {
    'circle-radius': 8,
    'circle-color': '#0084FF',
    'circle-stroke-width': 3,
    'circle-stroke-color': '#ffffff'
  }
});

let step = 0;
function animatePoint() {
  step = (step + 1) % routeCoordinates.length;
  map.getSource('moving-vehicle').setData({
    type: 'Feature',
    geometry: { type: 'Point', coordinates: routeCoordinates[step] }
  });
  requestAnimationFrame(animatePoint);
}
// Start loop
// animatePoint();
```

### B. 360° Cinematic Camera Orbit Around a Coordinate

Rotates the camera smoothly around a point of interest (e.g. a mountain peak or 3D monument):

```javascript
function rotateCamera(timestamp) {
  // Clamp rotation speed
  map.rotateTo((timestamp / 100) % 360, { duration: 0 });
  requestAnimationFrame(rotateCamera);
}

// Center camera and tilt for 3D perspective
map.jumpTo({ center: [14.4378, 50.0755], zoom: 15, pitch: 60 });
// requestAnimationFrame(rotateCamera);
```

---

## 2. Dynamic Data Feeds & Real-Time Updates

### A. Live GeoJSON Streaming

Update coordinates or feature attributes without triggering WebGL re-initialization:

```javascript
// Add empty source with auto-generated IDs for feature state
map.addSource('realtime-fleet', {
  type: 'geojson',
  data: { type: 'FeatureCollection', features: [] },
  generateId: true
});

map.addLayer({
  id: 'fleet-symbols',
  type: 'symbol',
  source: 'realtime-fleet',
  layout: {
    'icon-image': 'car-15',
    'icon-size': 1.2,
    'text-field': '{name}',
    'text-variable-anchor': ['top', 'bottom', 'left', 'right'],
    'text-offset': [0, 1],
    'text-size': 12
  }
});

// Periodic fetch and update
async function pollFleetPositions() {
  try {
    const res = await fetch('/api/fleet/positions');
    const geojson = await res.json();
    map.getSource('realtime-fleet').setData(geojson);
  } catch (err) {
    console.error('Fleet polling failed:', err);
  }
}
setInterval(pollFleetPositions, 3000);
```

### B. Animated Gradient Line (`line-gradient`)

Renders a continuous color ramp along a route (requires `lineMetrics: true` on the GeoJSON source):

```javascript
map.addSource('route-source', {
  type: 'geojson',
  lineMetrics: true, // MANDATORY for line-gradient expressions
  data: {
    type: 'Feature',
    geometry: {
      type: 'LineString',
      coordinates: [
        [14.4207, 50.0880],
        [14.4250, 50.0850],
        [14.4300, 50.0820],
        [14.4378, 50.0755]
      ]
    }
  }
});

map.addLayer({
  id: 'gradient-route',
  type: 'line',
  source: 'route-source',
  layout: {
    'line-join': 'round',
    'line-cap': 'round'
  },
  paint: {
    'line-width': 6,
    'line-gradient': [
      'interpolate',
      ['linear'],
      ['line-progress'],
      0.0, '#00D2FF',  // Start: Cyan
      0.5, '#0084FF',  // Mid: MapTiler Electric Blue
      1.0, '#ef4444'   // End: Red
    ]
  }
});
```

---

## 3. High-Performance Feature State (Hover & Selection)

Avoid calling `setData()` on every mousemove. Feature state toggles attributes directly inside the GPU shader at 60 FPS:

```javascript
map.addSource('states', {
  type: 'geojson',
  data: 'https://raw.githubusercontent.com/datasets/geo-boundaries-world-110m/master/countries.geojson',
  generateId: true // Generates numeric feature IDs if missing
});

map.addLayer({
  id: 'states-fill',
  type: 'fill',
  source: 'states',
  paint: {
    'fill-color': '#0084FF',
    'fill-opacity': [
      'case',
      ['boolean', ['feature-state', 'hover'], false],
      0.8,  // Opacity when hovered
      0.25  // Default opacity
    ]
  }
});

map.addLayer({
  id: 'states-borders',
  type: 'line',
  source: 'states',
  paint: {
    'line-color': '#0084FF',
    'line-width': [
      'case',
      ['boolean', ['feature-state', 'hover'], false],
      2.5,
      0.8
    ]
  }
});

let hoveredId = null;

map.on('mousemove', 'states-fill', (e) => {
  if (e.features.length > 0) {
    if (hoveredId !== null) {
      map.setFeatureState({ source: 'states', id: hoveredId }, { hover: false });
    }
    hoveredId = e.features[0].id;
    map.setFeatureState({ source: 'states', id: hoveredId }, { hover: true });
    map.getCanvas().style.cursor = 'pointer';
  }
});

map.on('mouseleave', 'states-fill', () => {
  if (hoveredId !== null) {
    map.setFeatureState({ source: 'states', id: hoveredId }, { hover: false });
  }
  hoveredId = null;
  map.getCanvas().style.cursor = '';
});
```

---

## 4. 3D Terrain, Buildings & Atmosphere

### 3D Building Extrusions with Dynamic Sun Direction

```javascript
map.on('load', () => {
  // Add 3D Extruded Buildings beneath label layers
  map.addLayer({
    id: '3d-buildings',
    source: 'openmaptiles', // Or the primary vector source from style
    'source-layer': 'building',
    filter: ['!=', ['get', 'hide_3d'], true],
    type: 'fill-extrusion',
    minzoom: 14,
    paint: {
      'fill-extrusion-color': '#cbd5e1',
      'fill-extrusion-height': [
        'interpolate', ['linear'], ['zoom'],
        14, 0,
        14.05, ['get', 'render_height']
      ],
      'fill-extrusion-base': [
        'interpolate', ['linear'], ['zoom'],
        14, 0,
        14.05, ['get', 'render_min_height']
      ],
      'fill-extrusion-opacity': 0.85
    }
  });

  // Set directional sunlight
  map.setLight({
    anchor: 'viewport',
    color: '#ffffff',
    intensity: 0.35,
    position: [1.15, 210, 30] // [radial coordinate, azimuthal angle, polar angle]
  });
});
```

---

## 5. Clustering with Spiderfy & Zoom-to-Bounds

```javascript
map.addSource('earthquakes', {
  type: 'geojson',
  data: 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson',
  cluster: true,
  clusterMaxZoom: 14,
  clusterRadius: 50
});

// Cluster circles with step-function size and color
map.addLayer({
  id: 'clusters',
  type: 'circle',
  source: 'earthquakes',
  filter: ['has', 'point_count'],
  paint: {
    'circle-color': [
      'step',
      ['get', 'point_count'],
      '#00D2FF', // < 10 items
      10, '#0084FF', // 10 - 49
      50, '#ef4444'  // >= 50
    ],
    'circle-radius': [
      'step',
      ['get', 'point_count'],
      18,
      10, 24,
      50, 32
    ],
    'circle-stroke-width': 2,
    'circle-stroke-color': '#ffffff'
  }
});

// Cluster count labels
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

// Unclustered individual points
map.addLayer({
  id: 'unclustered-point',
  type: 'circle',
  source: 'earthquakes',
  filter: ['!', ['has', 'point_count']],
  paint: {
    'circle-color': '#FF6B00',
    'circle-radius': 6,
    'circle-stroke-width': 1.5,
    'circle-stroke-color': '#fff'
  }
});

// Zoom to cluster bounds on click
map.on('click', 'clusters', async (e) => {
  const features = map.queryRenderedFeatures(e.point, { layers: ['clusters'] });
  const clusterId = features[0].properties.cluster_id;
  
  const zoom = await map.getSource('earthquakes').getClusterExpansionZoom(clusterId);
  map.easeTo({
    center: features[0].geometry.coordinates,
    zoom: zoom
  });
});
```

---

## 6. Spatial Querying (`queryRenderedFeatures`)

Query features interactively across visible vector or GeoJSON layers:

```javascript
map.on('click', (e) => {
  // Query a 10px bounding box around mouse click
  const bbox = [
    [e.point.x - 5, e.point.y - 5],
    [e.point.x + 5, e.point.y + 5]
  ];

  const features = map.queryRenderedFeatures(bbox, {
    layers: ['unclustered-point', 'states-fill']
  });

  if (!features.length) return;

  const feature = features[0];
  new maplibregl.Popup()
    .setLngLat(e.lngLat)
    .setHTML(`
      <div style="font-family: system-ui; font-size: 13px;">
        <strong style="color: #0084FF;">Feature Inspector</strong><br/>
        <b>Layer:</b> ${feature.layer.id}<br/>
        <b>Properties:</b> <pre style="margin: 4px 0 0; font-size: 11px;">${JSON.stringify(feature.properties, null, 2)}</pre>
      </div>
    `)
    .addTo(map);
});
```
