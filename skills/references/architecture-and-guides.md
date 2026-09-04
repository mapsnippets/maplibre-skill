# MapLibre GL JS Architecture & Guides 🏗️

> Deep technical guide to MapLibre GL JS architecture, WebGL rendering pipeline, custom layers, protocol extensions, camera math, and production performance optimizations.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Core Map Lifecycle & WebGL Context Management

### A. Lifecycle Phases
1. **Instantiation (`new maplibregl.Map({...})`)**: Creates DOM container, WebGL canvas context, worker threads, and initiates style JSON network fetch.
2. **`style.load` Event**: Fired when the style JSON is downloaded and parsed. Sources and layers can safely be added now.
3. **`load` Event**: Fired when all initial style assets (glyphs, sprites, tiles) have finished loading.
4. **`render` / `idle` Events**: `render` fires after every frame. `idle` fires when all currently pending tiles and assets have finished processing.
5. **Teardown (`map.remove()`)**: Explicitly releases WebGL context, worker pools, event listeners, and DOM elements.

### B. The 16 WebGL Context Trap
Browsers (Chrome, Firefox, Safari) enforce a hard limit of **16 active WebGL contexts per tab**. If maps are created in single-page applications (React, Next.js, Vue, Svelte) without proper destruction on unmount:
* New map canvases will silently fail to render or trigger `webglcontextlost`.
* Memory will rapidly leak across route transitions.

**Production React Cleanup Pattern:**
```typescript
import React, { useEffect, useRef } from 'react';
import * as maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

export const MapComponent: React.FC<{ apiKey: string }> = ({ apiKey }) => {
  const mapContainer = useRef<HTMLDivElement>(null);
  const mapInstance = useRef<maplibregl.Map | null>(null);

  useEffect(() => {
    if (!mapContainer.current) return;

    const map = new maplibregl.Map({
      container: mapContainer.current,
      style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
      center: [14.42076, 50.08804],
      zoom: 12
    });
    mapInstance.current = map;

    return () => {
      map.remove();
      mapInstance.current = null;
    };
  }, [apiKey]);

  return <div ref={mapContainer} style={{ width: '100%', height: '100vh' }} />;
};
```

---

## 2. Camera Transformations & Math

### A. Smooth Navigation (`flyTo` vs `easeTo` vs `jumpTo`)
* **`map.flyTo({ center, zoom, pitch, bearing, speed, curve })`**: Non-linear, cinematic flight trajectory. Automatically zooms out and in.
* **`map.easeTo({ center, zoom, duration, easing })`**: Smooth linear transition.
* **`map.jumpTo({ center, zoom })`**: Instantaneous repositioning with zero animation.

### B. Bounding Box Framing (`fitBounds`)
```javascript
const bounds = new maplibregl.LngLatBounds();
features.forEach(f => bounds.extend(f.geometry.coordinates));

map.fitBounds(bounds, {
  padding: { top: 60, bottom: 60, left: 60, right: 60 },
  maxZoom: 16,
  duration: 1200
});
```

### C. Offsetting the Center (Padding & Vanishing Points)
When building sidebar-heavy applications, offset the viewport vanishing point using `padding`:
```javascript
map.setPadding({ left: 360, top: 0, right: 0, bottom: 0 });
```

---

## 3. Custom WebGL Layers (`CustomLayerInterface`)

MapLibre allows custom WebGL rendering routines (such as Three.js or raw shaders) to be drawn directly within the MapLibre render loop, sharing depth buffers and projection matrices:

```javascript
import * as THREE from 'three';

const modelOrigin = [14.42076, 50.08804];
const modelAltitude = 0;
const modelAsMercatorCoordinate = maplibregl.MercatorCoordinate.fromLngLat(modelOrigin, modelAltitude);

const customLayer = {
  id: '3d-model',
  type: 'custom',
  renderingMode: '3d',
  onAdd: function (map, gl) {
    this.camera = new THREE.Camera();
    this.scene = new THREE.Scene();

    const light = new THREE.DirectionalLight(0xffffff);
    light.position.set(0, -70, 100).normalize();
    this.scene.add(light);

    const geometry = new THREE.BoxGeometry(30, 30, 30);
    const material = new THREE.MeshLambertMaterial({ color: 0x0084FF });
    this.cube = new THREE.Mesh(geometry, material);
    this.scene.add(this.cube);

    this.map = map;
    this.renderer = new THREE.WebGLRenderer({
      canvas: map.getCanvas(),
      context: gl,
      antialias: true
    });
    this.renderer.autoClear = false;
  },
  render: function (gl, matrix) {
    const rotationX = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(1, 0, 0), modelTransform.rx);
    const scale = new THREE.Matrix4().makeScale(modelTransform.scale, -modelTransform.scale, modelTransform.scale);

    const m = new THREE.Matrix4().fromArray(matrix);
    this.camera.projectionMatrix = m.multiply(rotationX).multiply(scale);
    this.renderer.resetState();
    this.renderer.render(this.scene, this.camera);
    this.map.triggerRepaint();
  }
};

map.addLayer(customLayer);
```

---

## 4. Custom Protocol Extensions (`maplibregl.addProtocol`)

Extend MapLibre tile loading with custom streaming decoders:

### A. PMTiles (Zero-Server Cloud-Native Tiles)
```javascript
import { Protocol } from 'pmtiles';

const protocol = new Protocol();
maplibregl.addProtocol('pmtiles', protocol.tile);

map.addSource('osm-pmtiles', {
  type: 'vector',
  url: 'pmtiles://https://data.source.coop/protomaps/osm.pmtiles'
});
```

### B. Custom GeoJSON/Feature Transformations
Intercept raw requests and parse in memory:
```javascript
maplibregl.addProtocol('csv-points', (params, abortController) => {
  return fetch(params.url.replace('csv-points://', ''))
    .then(r => r.text())
    .then(csv => {
      const geojson = parseCsvToGeoJson(csv);
      return { data: geojson };
    });
});
```

---

## 5. Production Performance & Memory Checklist

1. **Enable GeoJSON Clustering:**
   * Always set `cluster: true`, `clusterRadius: 50`, `clusterMaxZoom: 14` for point datasets > 500 features.
   * Prevents UI freezing by delegating point aggregation to background Web Workers.
2. **Use Feature State for Hover/Selection:**
   * **Do NOT** call `map.getSource().setData()` on `mousemove`! Re-uploading GeoJSON every frame degrades frame rates.
   * **DO** use `map.setFeatureState({ source, id }, { hover: true })` and read via `["feature-state", "hover"]` inside paint expressions.
3. **Budget Tile Caching:**
   * Use `maxTileCacheSize: 100` to limit RAM consumption on mobile devices.
4. **Constrain Bounds & Zooms:**
   * Always configure `minZoom`, `maxZoom`, and `maxBounds` to avoid querying unnecessary global tiles.
