# MapTiler Style URLs for MapLibre GL JS

Complete reference for all MapTiler vector style.json URLs usable with `maplibregl.Map`.

> [MapTiler Maps API](https://docs.maptiler.com/cloud/api/maps/) · [Style gallery](https://www.maptiler.com/maps/)

---

## URL Pattern

```
https://api.maptiler.com/maps/{style}/style.json?key=YOUR_MAPTILER_KEY
```

MapLibre loads the style.json which includes all vector tile sources, layers, fonts (glyphs), and sprites. No additional tile URL configuration needed.

---

## Usage

```js
const map = new maplibregl.Map({
  container: 'map',
  style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_MAPTILER_KEY',
  center: [14.4178, 50.1167],
  zoom: 12
});
```

### Switching Styles at Runtime

```js
map.setStyle('https://api.maptiler.com/maps/satellite/style.json?key=YOUR_MAPTILER_KEY');

// Re-add custom layers after style change
map.once('styledata', () => {
  addMyCustomLayers();
});
```

---

## All Available Styles

| Style Name | URL path | Description |
|------------|----------|-------------|
| Streets v4 | `maps/streets-v4/style.json` | Default street map with roads, labels, POIs |
| Streets v4 Dark | `maps/streets-v4-dark/style.json` | Dark theme streets |
| Streets v4 Light | `maps/streets-v4-light/style.json` | Light theme streets |
| Satellite | `maps/satellite/style.json` | Satellite/aerial imagery |
| Hybrid | `maps/hybrid/style.json` | Satellite imagery with labels overlay |
| Outdoor v4 | `maps/outdoor-v4/style.json` | Hiking, cycling, trails, elevation |
| Outdoor v4 Dark | `maps/outdoor-v4-dark/style.json` | Dark theme outdoor |
| Topo v4 | `maps/topo-v4/style.json` | Topographic with contour lines |
| Dataviz | `maps/dataviz/style.json` | Clean background for data overlays |
| Dataviz Dark | `maps/dataviz-dark/style.json` | Dark theme dataviz |
| Dataviz Light | `maps/dataviz-light/style.json` | Light theme dataviz |
| Base v4 | `maps/base-v4/style.json` | Simplified, minimal |
| Base v4 Dark | `maps/base-v4-dark/style.json` | Dark simplified |
| Base v4 Light | `maps/base-v4-light/style.json` | Light simplified |
| Bright v4 | `maps/bright-v4/style.json` | Vibrant, colorful |
| Bright v4 Dark | `maps/bright-v4-dark/style.json` | Dark vibrant |
| Winter v4 | `maps/winter-v4/style.json` | Ski slopes, winter terrain |
| Ocean | `maps/ocean/style.json` | Maritime/nautical |
| Landscape | `maps/landscape/style.json` | Natural landscape emphasis |
| OpenStreetMap | `maps/openstreetmap/style.json` | Classic OSM look |
| Backdrop | `maps/backdrop/style.json` | High contrast with hillshading |
| Backdrop Dark | `maps/backdrop-dark/style.json` | Dark backdrop |

---

## Raster Tile Fallback

If you need raster tiles (e.g., for `raster` source type), MapTiler also provides them:

```
https://api.maptiler.com/maps/{style}/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY
```

But for MapLibre, **always prefer style.json** — it gives you vector tiles with full interactivity, expressions, and dynamic styling.

---

## Custom Styles

You can create custom styles at [MapTiler Cloud](https://cloud.maptiler.com/maps/). The custom style URL follows the same pattern:

```
https://api.maptiler.com/maps/{your-custom-style-id}/style.json?key=YOUR_MAPTILER_KEY
```

---

## Style Contents

A MapTiler style.json includes:

| Property | Description |
|----------|-------------|
| `sources` | Vector tile endpoints, terrain, satellite |
| `layers` | Layer definitions with paint/layout properties |
| `glyphs` | Font glyph URL template |
| `sprite` | Sprite sheet URL (icons, patterns) |
| `metadata` | Style metadata |

The style is self-contained — MapLibre automatically fetches tiles, fonts, and sprites based on the URLs in the style.

---

## Static Map Images

For non-interactive map images (emails, thumbnails, social cards):

```
https://api.maptiler.com/maps/{style}/static/{lng},{lat},{zoom}/{width}x{height}.png?key=YOUR_MAPTILER_KEY
```

Example:
```
https://api.maptiler.com/maps/streets-v4/static/14.4178,50.1167,12/800x600.png?key=YOUR_MAPTILER_KEY
```

Optional: `@2x` for HiDPI, `markers` for pin overlays, `path` for route lines.
