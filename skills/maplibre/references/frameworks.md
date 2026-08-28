# MapLibre GL JS Framework Integration

Same core pattern everywhere: create a map on mount, call `map.remove()` on unmount. MapLibre uses WebGL — **client-side only**.

---

## React (react-map-gl)

The standard React wrapper for MapLibre GL JS, maintained by Vis.gl.

```bash
npm install react-map-gl maplibre-gl
```

### Basic Usage

```jsx
import Map, { Marker, Popup, NavigationControl, Source, Layer } from 'react-map-gl/maplibre';
import 'maplibre-gl/dist/maplibre-gl.css';

function MapView() {
  return (
    <Map
      initialViewState={{
        longitude: 14.4178,
        latitude: 50.1167,
        zoom: 12
      }}
      style={{ width: '100%', height: '400px' }}
      mapStyle="https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY"
    >
      <NavigationControl position="top-right" />
      <Marker longitude={14.4178} latitude={50.1167} color="#FF0000" />
    </Map>
  );
}
```

### GeoJSON Layer

```jsx
import Map, { Source, Layer } from 'react-map-gl/maplibre';

const geojsonData = {
  type: 'FeatureCollection',
  features: [/* ... */]
};

const layerStyle = {
  id: 'points',
  type: 'circle',
  paint: {
    'circle-radius': 8,
    'circle-color': '#0891b2'
  }
};

function GeoJSONMap() {
  return (
    <Map
      initialViewState={{ longitude: 14.4178, latitude: 50.1167, zoom: 10 }}
      style={{ width: '100%', height: '400px' }}
      mapStyle="https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY"
    >
      <Source id="my-data" type="geojson" data={geojsonData}>
        <Layer {...layerStyle} />
      </Source>
    </Map>
  );
}
```

### Interactive Features (click, hover)

```jsx
import { useState, useCallback } from 'react';
import Map, { Source, Layer, Popup } from 'react-map-gl/maplibre';

function InteractiveMap() {
  const [popupInfo, setPopupInfo] = useState(null);

  const onClick = useCallback((event) => {
    const feature = event.features?.[0];
    if (feature) {
      setPopupInfo({
        longitude: event.lngLat.lng,
        latitude: event.lngLat.lat,
        name: feature.properties.name
      });
    }
  }, []);

  return (
    <Map
      initialViewState={{ longitude: 14.4178, latitude: 50.1167, zoom: 10 }}
      style={{ width: '100%', height: '400px' }}
      mapStyle="https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY"
      interactiveLayerIds={['points']}
      onClick={onClick}
      cursor="pointer"
    >
      <Source type="geojson" data={geojsonData}>
        <Layer id="points" type="circle" paint={{ 'circle-radius': 8, 'circle-color': '#0891b2' }} />
      </Source>

      {popupInfo && (
        <Popup
          longitude={popupInfo.longitude}
          latitude={popupInfo.latitude}
          onClose={() => setPopupInfo(null)}
        >
          <div>{popupInfo.name}</div>
        </Popup>
      )}
    </Map>
  );
}
```

### Accessing the Map Instance

```jsx
import { useRef, useCallback } from 'react';
import Map from 'react-map-gl/maplibre';

function MapWithRef() {
  const mapRef = useRef(null);

  const onLoad = useCallback(() => {
    const map = mapRef.current.getMap();
    // Access the raw maplibregl.Map instance
    map.addSource(...);
    map.addLayer(...);
  }, []);

  return (
    <Map
      ref={mapRef}
      onLoad={onLoad}
      initialViewState={{ longitude: 14.4178, latitude: 50.1167, zoom: 12 }}
      style={{ width: '100%', height: '400px' }}
      mapStyle="https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY"
    />
  );
}
```

### Vanilla MapLibre in React (without react-map-gl)

If you prefer imperative control:

```jsx
import { useEffect, useRef } from 'react';
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

function MapView() {
  const containerRef = useRef(null);
  const mapRef = useRef(null);

  useEffect(() => {
    if (mapRef.current) return;  // Strict Mode guard

    mapRef.current = new maplibregl.Map({
      container: containerRef.current,
      style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
      center: [14.4178, 50.1167],
      zoom: 12
    });

    return () => {
      mapRef.current?.remove();
      mapRef.current = null;
    };
  }, []);

  return <div ref={containerRef} style={{ width: '100%', height: '400px' }} />;
}
```

**Strict Mode**: React 18 fires `useEffect` twice in dev. The `if (mapRef.current) return` guard + `null` reset in cleanup is essential.

### Next.js

MapLibre requires `window`/`document` — use dynamic import with SSR disabled:

```jsx
// components/Map.jsx
"use client";
import dynamic from 'next/dynamic';

const MapView = dynamic(() => import('./MapView'), { ssr: false });
export default MapView;
```

Env: `NEXT_PUBLIC_MAPTILER_KEY`

---

## Vue 3

No official vue-maplibre wrapper — use maplibre-gl directly (it works great).

```bash
npm install maplibre-gl
```

### Basic Usage

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const container = ref(null);
let map = null;  // plain let, NOT ref() — Vue reactivity on map causes issues

onMounted(() => {
  map = new maplibregl.Map({
    container: container.value,
    style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
    center: [14.4178, 50.1167],
    zoom: 12
  });

  map.on('load', () => {
    // Add sources and layers here
  });
});

onUnmounted(() => {
  map?.remove();
  map = null;
});
</script>

<template>
  <div ref="container" style="width: 100%; height: 400px" />
</template>
```

### Vue Composable

```js
// composables/useMap.js
import { ref, onMounted, onUnmounted } from 'vue';
import maplibregl from 'maplibre-gl';

export function useMap(containerRef, options = {}) {
  const map = ref(null);
  const loaded = ref(false);

  onMounted(() => {
    const instance = new maplibregl.Map({
      container: containerRef.value,
      style: `https://api.maptiler.com/maps/${options.style || 'streets-v4'}/style.json?key=YOUR_MAPTILER_KEY`,
      center: options.center || [14.4178, 50.1167],
      zoom: options.zoom || 12,
      ...options
    });

    instance.on('load', () => { loaded.value = true; });
    map.value = instance;
  });

  onUnmounted(() => {
    map.value?.remove();
    map.value = null;
  });

  return { map, loaded };
}
```

### Nuxt SSR

Wrap with `<ClientOnly>`:

```vue
<template>
  <ClientOnly>
    <MapView />
  </ClientOnly>
</template>
```

---

## Svelte

```svelte
<script>
  import { onMount, onDestroy } from 'svelte';
  import maplibregl from 'maplibre-gl';
  import 'maplibre-gl/dist/maplibre-gl.css';

  let mapContainer;
  let map;

  onMount(() => {
    map = new maplibregl.Map({
      container: mapContainer,
      style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
      center: [14.4178, 50.1167],
      zoom: 12
    });

    map.on('load', () => {
      // Add sources and layers
    });
  });

  onDestroy(() => { map?.remove(); });
</script>

<div bind:this={mapContainer} style="width: 100%; height: 400px;" />
```

**SvelteKit SSR:** `onMount` only runs client-side, so the import is safe. For top-level dynamic imports, guard with `browser` from `$app/environment`.

---

## Angular

```bash
npm install maplibre-gl
```

```typescript
import { Component, ElementRef, AfterViewInit, OnDestroy, ViewChild } from '@angular/core';
import maplibregl from 'maplibre-gl';

@Component({
  selector: 'app-map',
  template: `<div #mapEl style="width: 100%; height: 400px"></div>`,
})
export class MapComponent implements AfterViewInit, OnDestroy {
  @ViewChild('mapEl', { static: true }) mapEl!: ElementRef;
  private map!: maplibregl.Map;

  ngAfterViewInit() {
    this.map = new maplibregl.Map({
      container: this.mapEl.nativeElement,
      style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
      center: [14.4178, 50.1167],
      zoom: 12
    });
  }

  ngOnDestroy() {
    this.map?.remove();
  }
}
```

Add MapLibre CSS in `angular.json`:
```json
"styles": [
  "node_modules/maplibre-gl/dist/maplibre-gl.css",
  "src/styles.css"
]
```

**SSR (Angular Universal):** Guard with `isPlatformBrowser()` or use `afterNextRender()`.

---

## Cleanup Checklist

1. **Always call `map.remove()` on unmount** — prevents WebGL context leaks and memory growth
2. **Guard against double initialization** — React Strict Mode, HMR, and SPA navigation
3. **`height` is required** — container must have explicit CSS height
4. **SSR guard** — MapLibre needs `window`/`document`; use dynamic import or client-only wrappers
5. **Do NOT wrap map instance in Vue `ref()`/`reactive()`** — Vue proxy breaks WebGL internals

## Env Var Quick Reference

| Tool | Prefix | Access |
|------|--------|--------|
| Vite | `VITE_` | `import.meta.env.VITE_MAPTILER_KEY` |
| Next.js | `NEXT_PUBLIC_` | `process.env.NEXT_PUBLIC_MAPTILER_KEY` |
| CRA | `REACT_APP_` | `process.env.REACT_APP_MAPTILER_KEY` |
| Angular | — | `environment.ts` |
| SvelteKit | `PUBLIC_` | `$env/static/public` |
