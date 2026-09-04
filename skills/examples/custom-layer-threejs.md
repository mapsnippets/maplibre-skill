# Official Example: 3D Model with Three.js Custom Layer 🎮📦

> Source: https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-using-threejs/

This tutorial shows how to render custom 3D WebGL meshes (using Three.js) directly inside the MapLibre GL JS render pipeline via the `CustomLayerInterface`, synchronizing depth buffers and camera projection matrices using `MercatorCoordinate`.

---

## 1. HTML Setup

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapLibre Three.js Custom Layer</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; }
    #map { width: 100vw; height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>
  
  <script src="https://unpkg.com/three@0.147.0/build/three.min.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import * as maplibregl from 'maplibre-gl';
import * as THREE from 'three';
import 'maplibre-gl/dist/maplibre-gl.css';

const apiKey = 'YOUR_MAPTILER_API_KEY';

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
  center: [14.42076, 50.08804],
  zoom: 16.5,
  pitch: 60,
  bearing: 25
});

// 1. Geographic origin of the 3D model
const modelOrigin = [14.42076, 50.08804];
const modelAltitude = 0;
const modelAsMercatorCoordinate = maplibregl.MercatorCoordinate.fromLngLat(modelOrigin, modelAltitude);

// Transformation parameters to scale and position model
const modelTransform = {
  translateX: modelAsMercatorCoordinate.x,
  translateY: modelAsMercatorCoordinate.y,
  translateZ: modelAsMercatorCoordinate.z,
  rotateX: Math.PI / 2,
  rotateY: 0,
  rotateZ: 0,
  scale: modelAsMercatorCoordinate.meterInMercatorCoordinateUnits()
};

// 2. Custom WebGL Layer Definition
const customThreeLayer = {
  id: '3d-threejs-model',
  type: 'custom',
  renderingMode: '3d',

  onAdd: function (map, gl) {
    this.camera = new THREE.Camera();
    this.scene = new THREE.Scene();

    // Ambient & Directional Lighting
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.8);
    this.scene.add(ambientLight);

    const directionalLight = new THREE.DirectionalLight(0xffffff, 0.6);
    directionalLight.position.set(0, -70, 100).normalize();
    this.scene.add(directionalLight);

    // Create 3D Geometry (e.g. Branded 3D Monolith)
    const geometry = new THREE.BoxGeometry(40, 80, 40);
    const material = new THREE.MeshLambertMaterial({ color: 0x0084FF });
    this.cube = new THREE.Mesh(geometry, material);
    this.cube.position.set(0, 40, 0);
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
    const rotationX = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(1, 0, 0), modelTransform.rotateX);
    const rotationY = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(0, 1, 0), modelTransform.rotateY);
    const rotationZ = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(0, 0, 1), modelTransform.rotateZ);

    const m = new THREE.Matrix4().fromArray(matrix);
    const l = new THREE.Matrix4()
      .makeTranslation(modelTransform.translateX, modelTransform.translateY, modelTransform.translateZ)
      .scale(new THREE.Vector3(modelTransform.scale, -modelTransform.scale, modelTransform.scale))
      .multiply(rotationX)
      .multiply(rotationY)
      .multiply(rotationZ);

    this.camera.projectionMatrix = m.multiply(l);
    this.renderer.resetState();
    this.renderer.render(this.scene, this.camera);
    this.map.triggerRepaint();
  }
};

map.on('load', () => {
  map.addLayer(customThreeLayer);
});
```
