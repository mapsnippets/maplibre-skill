# Three.js 3D Model Clamped to 3D Terrain

> **Documentation Reference:** [Three.js 3D Model Clamped to 3D Terrain](https://maplibre.org/maplibre-gl-js/docs/examples/adding-3d-models-using-threejs-on-terrain/)
> Category: **3D Terrain, Buildings & Elevation**

## Overview
Places an external 3D mesh model with Three.js accurately positioned and altitude-clamped onto 3D DEM terrain.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MapLibre - Three.js Model Clamped to Terrain</title>
  
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <script src="https://unpkg.com/three@0.147.0/build/three.min.js"></script>
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script type="module">
    import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs';
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const modelOrigin = [8.5417, 47.3769];
    const modelAltitude = 0;

    const map = new maplibregl.Map({
      container: 'map',
      style: `https://api.maptiler.com/maps/outdoor-v4/style.json?key=${MAPTILER_KEY}`,
      center: modelOrigin,
      zoom: 15,
      pitch: 60,
      bearing: -20
    });

    map.on('load', () => {
      map.addSource('terrain-rgb', {
        type: 'raster-dem',
        url: `https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=${MAPTILER_KEY}`,
        tileSize: 512
      });
      map.setTerrain({ source: 'terrain-rgb', exaggeration: 1.5 });

      const customLayer = {
        id: '3d-mesh',
        type: 'custom',
        renderingMode: '3d',
        onAdd: function (map, gl) {
          this.camera = new THREE.Camera();
          this.scene = new THREE.Scene();

          const light = new THREE.DirectionalLight(0xffffff);
          light.position.set(0, -70, 100).normalize();
          this.scene.add(light);

          const geom = new THREE.ConeGeometry(50, 150, 16);
          const mat = new THREE.MeshPhongMaterial({ color: 0x0084ff });
          this.mesh = new THREE.Mesh(geom, mat);
          this.scene.add(this.mesh);
          this.map = map;
          this.renderer = new THREE.WebGLRenderer({ canvas: map.getCanvas(), context: gl, antialias: true });
          this.renderer.autoClear = false;
        },
        render: function (gl, matrix) {
          const terrainElevation = this.map.queryTerrainElevation(modelOrigin) || 0;
          const coord = maplibregl.MercatorCoordinate.fromLngLat(modelOrigin, terrainElevation + 50);
          const scale = coord.meterInMercatorCoordinateUnits();

          const m = new THREE.Matrix4().fromArray(matrix);
          const l = new THREE.Matrix4()
            .makeTranslation(coord.x, coord.y, coord.z)
            .scale(new THREE.Vector3(scale, -scale, scale))
            .multiply(new THREE.Matrix4().makeRotationX(Math.PI / 2));

          this.camera.projectionMatrix = m.multiply(l);
          this.renderer.resetState();
          this.renderer.render(this.scene, this.camera);
          this.map.triggerRepaint();
        }
      };

      map.addLayer(customLayer);
    });
  </script>
</body>
</html>
```

## Key API Features
- Native MapLibre GL JS implementation.
- Modern MapTiler Planet v4 vector and raster styles.
- Self-contained and production-ready.
