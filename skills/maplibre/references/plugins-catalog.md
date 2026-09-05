# MapLibre GL JS Plugins Catalog & Integration Guide 🔌🗺️

> Authoritative catalog and technical implementation reference for top third-party plugins, controls, layer extensions, drawing suites, and utility libraries for **MapLibre GL JS v4–v6**.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 🔎 Quick Plugin Directory

| Category | Top Recommendation | Alternatives | Typical Use Case |
| :--- | :--- | :--- | :--- |
| **Search & Geocoding** | `@maptiler/geocoding-control` | `@maplibre/maplibre-gl-geocoder` | Autocomplete search, POI lookup, reverse geocoding |
| **Drawing & Digitizing**| `@mapbox/mapbox-gl-draw` | `terra-draw` | Polygon, line, and point digitizing; GeoJSON export |
| **Map Comparison** | `@maplibre/maplibre-gl-compare`| `maplibre-gl-opacity` | Split-screen before/after swipe wiper |
| **Inspection & Debug** | `maplibre-gl-inspect` | Built-in tile/collision bounds | Vector tile feature & attribute inspection |
| **Export & Printing** | `@watergis/maplibre-gl-export` | HTML5 Canvas `.toDataURL()` | High-DPI PDF, PNG, SVG map image exports |
| **3D & Elevation** | Native MapTiler Terrain DEM | `three` (CustomLayer), `maplibre-contour` | 3D terrain mesh, glTF models, contour isolines |
| **Framework Wrappers** | `react-map-gl/maplibre` | `@vis.gl/react-maplibre`, `svelte-maplibre` | Declarative UI components for React, Svelte, Vue |

---

## 1. Search & Geocoding Plugins

### 1.1 MapTiler Geocoding Control *(Recommended)*
Official MapTiler search and reverse-geocoding control featuring fuzzy matching, POI category filtering, and bbox constraints.

* **NPM:** `npm install @maptiler/geocoding-control maplibre-gl`
* **CDN (UMD):**
  ```html
  <link rel="stylesheet" href="https://cdn.maptiler.com/maptiler-geocoding-control/v1.3.0/style.css" />
  <script src="https://cdn.maptiler.com/maptiler-geocoding-control/v1.3.0/maplibregl.umd.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const gc = new maptilergeocoding.GeocodingControl({
    apiKey: 'YOUR_MAPTILER_API_KEY',
    maplibregl: maplibregl,
    placeholder: 'Search cities, addresses, POIs...',
    collapsed: false,
    showResultsWhileTyping: true,
    marker: { color: '#0084FF' }
  });
  map.addControl(gc, 'top-left');

  gc.on('select', (feature) => {
    console.log('Selected location:', feature);
  });
  ```
* **Gotcha:** When using the UMD CDN bundle, the global namespace is `maptilergeocoding.GeocodingControl`.

### 1.2 MapLibre GL Geocoder
Generic geocoder control for self-hosted Pelias, Nominatim, or custom geocoding backends.

* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/@maplibre/maplibre-gl-geocoder@1.5.0/dist/maplibre-gl-geocoder.css" />
  <script src="https://unpkg.com/@maplibre/maplibre-gl-geocoder@1.5.0/dist/maplibre-gl-geocoder.min.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const geocoderApi = {
    forwardGeocode: async (config) => {
      const features = [];
      const res = await fetch(`https://api.maptiler.com/geocoding/${encodeURIComponent(config.query)}.json?key=YOUR_API_KEY`);
      const geojson = await res.json();
      return { features: geojson.features };
    }
  };
  const geocoder = new MaplibreGeocoder(geocoderApi, { maplibregl: maplibregl });
  map.addControl(geocoder, 'top-left');
  ```

---

## 2. Drawing & Vector Digitizing

### 2.1 Mapbox GL Draw (`@mapbox/mapbox-gl-draw`) *(Standard)*
The most battle-tested drawing suite for MapLibre GL JS. Enables drawing points, polylines, and polygons with vertex snapping, deletion, and full GeoJSON extraction.

* **Recipe Reference:** [`examples/draw-polygon-geojson.md`](../examples/draw-polygon-geojson.md)
* **NPM:** `npm install @mapbox/mapbox-gl-draw`
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.4.3/mapbox-gl-draw.css" />
  <script src="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.4.3/mapbox-gl-draw.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const draw = new MapboxDraw({
    displayControlsDefault: false,
    controls: {
      polygon: true,
      line_string: true,
      point: true,
      trash: true
    }
  });
  map.addControl(draw, 'top-left');

  function syncGeoJSON() {
    const data = draw.getAll();
    console.log('Drawn GeoJSON FeatureCollection:', data);
  }

  map.on('draw.create', syncGeoJSON);
  map.on('draw.update', syncGeoJSON);
  map.on('draw.delete', syncGeoJSON);
  ```
* **Key Invariant:** Mapbox GL Draw v1.4.3 works natively on `maplibregl.Map`. Do NOT attempt to hand-roll polygon digitizing using raw `map.on('click')` coordinate arrays.

### 2.2 Terra Draw (`terra-draw`) *(Modern Alternative)*
Modern, lightweight, framework-agnostic drawing library with modular modes (circles, freehand, rectangles).

* **NPM:** `npm install terra-draw`
* **Integration Snippet:**
  ```javascript
  import { TerraDraw, TerraDrawMapLibreGLAdapter, TerraDrawPolygonMode, TerraDrawSelectMode } from 'terra-draw';

  const draw = new TerraDraw({
    adapter: new TerraDrawMapLibreGLAdapter({ map }),
    modes: [
      new TerraDrawPolygonMode({
        snapping: { toCoordinate: true }
      }),
      new TerraDrawSelectMode({
        flags: { polygon: { feature: { draggable: true, coordinates: { midpoints: true } } } }
      })
    ]
  });
  draw.start();
  draw.setMode('polygon');
  ```

---

## 3. UI Controls & Comparison Tools

### 3.1 MapLibre GL Compare (`@maplibre/maplibre-gl-compare`)
Interactive split-screen swipe slider synchronizing two maps for before-after visual comparison.

* **Recipe Reference:** [`examples/swipe-between-maps.md`](../examples/swipe-between-maps.md)
* **NPM:** `npm install @maplibre/maplibre-gl-compare`
* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/@maplibre/maplibre-gl-compare@0.5.0/dist/maplibre-gl-compare.css" />
  <script src="https://unpkg.com/@maplibre/maplibre-gl-compare@0.5.0/dist/maplibre-gl-compare.js"></script>
  ```
* **HTML Container Structure:**
  ```html
  <div id="comparison-container" style="position:relative; width:100%; height:100vh; overflow:hidden;">
    <div id="before" style="position:absolute; top:0; bottom:0; width:100%;"></div>
    <div id="after" style="position:absolute; top:0; bottom:0; width:100%;"></div>
  </div>
  ```
* **Integration Snippet:**
  ```javascript
  const beforeMap = new maplibregl.Map({
    container: 'before',
    style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${KEY}`,
    center: [6.8656, 45.8326],
    zoom: 12
  });

  const afterMap = new maplibregl.Map({
    container: 'after',
    style: `https://api.maptiler.com/maps/satellite-v4/style.json?key=${KEY}`,
    center: [6.8656, 45.8326],
    zoom: 12
  });

  const compare = new maplibregl.Compare(beforeMap, afterMap, '#comparison-container', {
    mousemove: false,
    orientation: 'vertical'
  });
  ```
* **Critical Gotchas:**
  1. Both maps MUST share identical `center` and `zoom` at creation.
  2. The parent container `#comparison-container` must have `position: relative` and `overflow: hidden`.
  3. Swiping clips DOM viewport containers, so both maps render their own independent WebGL canvases.

### 3.2 MapLibre GL Export (`@watergis/maplibre-gl-export`)
Client-side high-resolution map export control supporting PDF, PNG, and SVG outputs with DPI scaling.

* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/@watergis/maplibre-gl-export@3.0.1/dist/maplibre-gl-export.css" />
  <script src="https://unpkg.com/@watergis/maplibre-gl-export@3.0.1/dist/maplibre-gl-export.umd.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const exportControl = new MaplibreExportControl({
    PageSize: MaplibreExportControl.Size.A4,
    PageOrientation: MaplibreExportControl.PageOrientation.Landscape,
    Format: MaplibreExportControl.Format.PNG,
    DPI: MaplibreExportControl.DPI[300]
  });
  map.addControl(exportControl, 'top-right');
  ```
* **Gotcha:** `preserveDrawingBuffer: true` MUST be passed in `new maplibregl.Map({ preserveDrawingBuffer: true, ... })` if you intend to export the canvas directly using `canvas.toDataURL()`.

### 3.3 MapLibre GL Inspect (`maplibre-gl-inspect`)
Visual vector tile debugger allowing developers to view feature boundaries, tile grid lines, and click any vector feature to inspect its raw property table.

* **CDN:**
  ```html
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl-inspect@1.3.1/dist/maplibre-gl-inspect.css" />
  <script src="https://unpkg.com/maplibre-gl-inspect@1.3.1/dist/maplibre-gl-inspect.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  map.addControl(new MaplibreInspect({
    showInspectorButton: true,
    showColorizerButton: true,
    popup: new maplibregl.Popup({ closeButton: false })
  }), 'top-right');
  ```

---

## 4. 3D Models, Elevation & Visualizations

### 4.1 Three.js Custom WebGL Layer (`three` + `CustomLayerInterface`)
Renders true 3D glTF/GLB models, directional lighting, and animations directly inside MapLibre's WebGL coordinate system.

* **Recipe Reference:** [`examples/custom-layer-threejs.md`](../examples/custom-layer-threejs.md)
* **Integration Snippet:**
  ```javascript
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

  const modelCoord = [8.5417, 47.3769]; // Zurich
  const modelAlt = 0;

  const custom3DLayer = {
    id: '3d-model-layer',
    type: 'custom',
    renderingMode: '3d',
    onAdd: function (map, gl) {
      this.camera = new THREE.Camera();
      this.scene = new THREE.Scene();
      this.renderer = new THREE.WebGLRenderer({
        canvas: map.getCanvas(),
        context: gl,
        antialias: true
      });
      this.renderer.autoClear = false;

      // Add lights
      const light = new THREE.DirectionalLight(0xffffff, 1.5);
      light.position.set(0, -70, 100).normalize();
      this.scene.add(light);
    },
    render: function (gl, matrix) {
      const coord = maplibregl.MercatorCoordinate.fromLngLat(modelCoord, modelAlt);
      const scale = coord.meterInMercatorCoordinateUnits();

      const m = new THREE.Matrix4().fromArray(matrix);
      const l = new THREE.Matrix4()
        .makeTranslation(coord.x, coord.y, coord.z)
        .scale(new THREE.Vector3(scale, -scale, scale));

      this.camera.projectionMatrix = m.multiply(l);
      this.renderer.resetState();
      this.renderer.render(this.scene, this.camera);
      map.triggerRepaint();
    }
  };

  map.on('style.load', () => map.addLayer(custom3DLayer));
  ```

### 4.2 MapLibre Contour (`maplibre-contour`)
Generates dynamic contour lines and elevation isolines client-side from Terrain-RGB DEM tiles.

* **Recipe Reference:** [`examples/vector-contour-lines.md`](../examples/vector-contour-lines.md)
* **CDN:**
  ```html
  <script src="https://unpkg.com/maplibre-contour@0.2.0/dist/index.min.js"></script>
  ```
* **Integration Snippet:**
  ```javascript
  const demSource = new mlcontour.DemSource({
    url: `https://api.maptiler.com/tiles/terrain-rgb-v2/{z}/{x}/{y}.webp?key=${KEY}`,
    encoding: 'mapbox',
    maxzoom: 14
  });
  demSource.setupMaplibre(maplibregl);

  map.addSource('contour-source', {
    type: 'vector',
    tiles: [demSource.contourProtocolUrl({
      thresholds: {
        11: [50, 200],
        12: [20, 100],
        14: [10, 50]
      },
      elevationKey: 'ele',
      levelKey: 'level',
      contourLayer: 'contours'
    })],
    maxzoom: 15
  });

  map.addLayer({
    id: 'contour-lines',
    type: 'line',
    source: 'contour-source',
    'source-layer': 'contours',
    paint: {
      'line-color': '#0084FF',
      'line-width': ['match', ['get', 'level'], 1, 1.5, 0.7]
    }
  });
  ```

---

## 5. Framework Wrappers & Declarative UI

### 5.1 React Map GL (`react-map-gl/maplibre`)
Standard React wrapper providing declarative components with full TypeScript support.

* **NPM:** `npm install react-map-gl maplibre-gl`
* **Declarative React Example:**
  ```tsx
  import React, { useState } from 'react';
  import Map, { NavigationControl, Marker, Popup } from 'react-map-gl/maplibre';
  import 'maplibre-gl/dist/maplibre-gl.css';

  export function InteractiveMap() {
    const [popupOpen, setPopupOpen] = useState(false);

    return (
      <Map
        initialViewState={{ longitude: 14.4378, latitude: 50.0755, zoom: 12 }}
        style={{ width: '100vw', height: '100vh' }}
        mapStyle="https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_API_KEY"
      >
        <NavigationControl position="top-right" />
        <Marker longitude={14.4378} latitude={50.0755} color="#0084FF" onClick={() => setPopupOpen(true)} />
        {popupOpen && (
          <Popup longitude={14.4378} latitude={50.0755} onClose={() => setPopupOpen(false)}>
            <div>Prague Office</div>
          </Popup>
        )}
      </Map>
    );
  }
  ```
* **Next.js SSR Rule:** Always dynamically import `react-map-gl/maplibre` with `{ ssr: false }` because MapLibre relies on browser `window` and `WebGLRenderingContext`.

---

## 6. Plugin Compatibility & Troubleshooting Invariants

| Failure / Error | Root Cause | Solution |
| :--- | :--- | :--- |
| `Cannot read properties of undefined (reading 'addControl')` | Control instantiated before `map` was created or style was loaded | Call `map.addControl()` after `new maplibregl.Map(...)`. |
| `Error: layers.drawn-line.paint.fill-color: unknown property` | Layer type mismatch (using `fill-color` on `line` layer) | Use `line-color` on `type: 'line'`, or use `@mapbox/mapbox-gl-draw`. |
| Black or inverted canvas in Three.js | Depth buffer or context mismatch | Set `renderer.autoClear = false;` and pass `map.getCanvas()` + `context: gl`. |
| Swipe wiper not sliding | Parent container lacks `position: relative` or `overflow: hidden` | Set explicit dimensions and `position: relative` on `#comparison-container`. |
| Canvas export returns blank image | WebGL canvas buffer cleared after render | Pass `preserveDrawingBuffer: true` in map constructor options. |
