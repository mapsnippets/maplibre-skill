# MapLibre GL JS — Events Reference

Complete reference for all map events with signatures and usage examples.

> [MapLibre Events Docs](https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapEventType/)

---

## Lifecycle Events

### load

Fired when the map has finished loading all resources (style, tiles, etc.). **This is the safest place to add sources and layers.**

```js
map.on('load', () => {
  map.addSource('my-source', { ... });
  map.addLayer({ ... });
});
```

### style.load

Fired when the style has finished loading. Fires before `load`.

```js
map.on('style.load', () => {
  console.log('Style loaded');
});
```

### styledata

Fired when the style is changed via `setStyle()`. **Critical for re-adding custom layers after style change.**

```js
map.once('styledata', () => {
  reAddCustomLayers();
});
```

### idle

Fired when the map enters an idle state (nothing loading, no animations).

```js
map.on('idle', () => {
  console.log('Map is idle');
});
```

### remove

Fired when the map is destroyed via `map.remove()`.

```js
map.on('remove', () => {
  // Cleanup custom resources
});
```

### render

Fired on each frame render. Use sparingly — fires very often.

```js
map.on('render', () => {
  // Called every frame
});
```

---

## Camera Events

### move / movestart / moveend

Fired during camera movement (pan, zoom, rotate, pitch).

```js
map.on('movestart', () => {
  console.log('Camera movement started');
});

map.on('move', () => {
  console.log('Moving...', map.getCenter());
});

map.on('moveend', () => {
  console.log('Camera stopped at:', map.getCenter());
});
```

### zoom / zoomstart / zoomend

Fired specifically during zoom changes.

```js
map.on('zoomend', () => {
  console.log('Zoom level:', map.getZoom());
});
```

### rotate / rotatestart / rotateend

Fired during bearing changes.

```js
map.on('rotateend', () => {
  console.log('Bearing:', map.getBearing());
});
```

### pitch / pitchstart / pitchend

Fired during pitch (tilt) changes.

```js
map.on('pitchend', () => {
  console.log('Pitch:', map.getPitch());
});
```

---

## Interaction Events

### click

Fired on map click. Use layer-specific version for feature clicks.

```js
// Click anywhere on map
map.on('click', (e) => {
  console.log('Clicked at:', e.lngLat.lng, e.lngLat.lat);
  console.log('Screen point:', e.point.x, e.point.y);
});

// Click on specific layer
map.on('click', 'my-layer', (e) => {
  const feature = e.features[0];
  console.log('Clicked feature:', feature.properties);

  new maplibregl.Popup()
    .setLngLat(e.lngLat)
    .setHTML(`<h3>${feature.properties.name}</h3>`)
    .addTo(map);
});
```

### dblclick

Fired on double-click. Default behavior zooms in.

```js
map.on('dblclick', (e) => {
  e.preventDefault();  // Prevent zoom
  console.log('Double-clicked at:', e.lngLat);
});
```

### contextmenu

Fired on right-click.

```js
map.on('contextmenu', (e) => {
  showCustomMenu(e.lngLat);
});
```

### mouseenter / mouseleave

Fired when mouse enters/leaves a layer's features. **Essential for hover effects.**

```js
map.on('mouseenter', 'my-layer', (e) => {
  map.getCanvas().style.cursor = 'pointer';
});

map.on('mouseleave', 'my-layer', () => {
  map.getCanvas().style.cursor = '';
});
```

### mousemove

Fired on mouse move over map.

```js
// Map-level — track cursor
map.on('mousemove', (e) => {
  coordsDisplay.textContent =
    `${e.lngLat.lng.toFixed(4)}, ${e.lngLat.lat.toFixed(4)}`;
});

// Layer-specific — feature interaction
map.on('mousemove', 'my-layer', (e) => {
  const feature = e.features[0];
  // Use for hover highlighting
});
```

### mousedown / mouseup

```js
map.on('mousedown', (e) => {
  console.log('Mouse down at:', e.lngLat);
});
```

---

## Touch Events

### touchstart / touchend / touchcancel

Touch equivalents for mobile devices.

```js
map.on('touchstart', (e) => {
  console.log('Touch at:', e.lngLat);
});
```

---

## Data Events

### data

Fired when any data (style, source, tile) changes.

```js
map.on('data', (e) => {
  if (e.dataType === 'source') {
    console.log('Source data changed:', e.sourceId);
  }
});
```

### sourcedata

Fired when a source's data changes.

```js
map.on('sourcedata', (e) => {
  if (e.sourceId === 'my-source' && e.isSourceLoaded) {
    console.log('My source finished loading');
  }
});
```

### dataloading / sourcedataloading

Fired when data/source loading begins.

---

## Terrain Events

### terrain

Fired when terrain is added/removed.

```js
map.on('terrain', () => {
  console.log('Terrain state changed');
});
```

---

## Error Events

### error

Fired when an error occurs.

```js
map.on('error', (e) => {
  console.error('Map error:', e.error);
});
```

---

## Event Object Properties

All mouse/touch events include:

| Property | Type | Description |
|----------|------|-------------|
| `type` | string | Event type name |
| `target` | Map | The map instance |
| `originalEvent` | Event | Original DOM event |
| `point` | Point | Screen coordinates `{x, y}` |
| `lngLat` | LngLat | Geographic coordinates `{lng, lat}` |
| `features` | Feature[] | Features at point (layer-specific events only) |
| `preventDefault()` | function | Prevent default behavior |

---

## Removing Event Listeners

```js
// Named function (recommended for removal)
function handleClick(e) {
  console.log('Clicked');
}

map.on('click', handleClick);
map.off('click', handleClick);

// One-time listener
map.once('load', () => {
  // Only fires once, auto-removes
});

// Layer-specific removal
map.on('click', 'my-layer', handleLayerClick);
map.off('click', 'my-layer', handleLayerClick);
```

---

## Event Quick Reference

| Event | When | Common Use |
|-------|------|-----------|
| `load` | Style + tiles ready | Add sources/layers |
| `styledata` | Style changed | Re-add custom layers |
| `idle` | Nothing loading | Screenshots, exports |
| `click` | Map clicked | Place markers, show info |
| `click` (layer) | Feature clicked | Popups, selection |
| `mouseenter` (layer) | Mouse on feature | Cursor change, highlight |
| `mouseleave` (layer) | Mouse off feature | Reset cursor/highlight |
| `mousemove` | Mouse moves | Coordinate display |
| `moveend` | Camera stops | Load data for view |
| `zoomend` | Zoom stops | Zoom-dependent logic |
| `error` | Error occurs | Error handling |
