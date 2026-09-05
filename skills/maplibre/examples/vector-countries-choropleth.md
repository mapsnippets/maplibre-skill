# Thematic Choropleth with MapTiler Countries Vector Tiles 📊🗺️

> **Target Category:** Production Thematic Cartography & Boundary Analysis  
> **Source Schema:** [MapTiler Countries Vector Tileset](https://docs.maptiler.com/schema/countries/) (`schema/countries/`)

Demonstrates how to build an interactive, hardware-accelerated demographic choropleth map without downloading megabytes of GeoJSON polygons. Consumes the official **MapTiler Countries vector tileset**, classifies population density into structured intervals, dynamically binds tabular data using a data-driven `match` expression, and adds an interactive hover HUD and visual color legend.

---

## 1. HTML Container & HUD Styles

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MapTiler Countries Vector Choropleth</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.css" />
  <style>
    body { margin: 0; padding: 0; font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
    #map { width: 100vw; height: 100vh; }

    /* Visual Legend Card */
    .legend-card {
      position: absolute;
      bottom: 24px;
      right: 24px;
      background: rgba(15, 23, 42, 0.9);
      backdrop-filter: blur(8px);
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: 8px;
      padding: 14px 18px;
      color: #f8fafc;
      font-size: 12px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
      z-index: 10;
      min-width: 170px;
    }
    .legend-title { font-weight: 700; font-size: 13px; margin-bottom: 4px; }
    .legend-sub { font-size: 10px; color: #38bdf8; margin-bottom: 10px; font-weight: 600; }
    .legend-item { display: flex; align-items: center; margin-bottom: 6px; }
    .legend-swatch { width: 18px; height: 14px; border-radius: 3px; margin-right: 10px; border: 1px solid rgba(255, 255, 255, 0.3); }

    /* MapLibre popup refinement */
    .maplibregl-popup-content {
      border-radius: 8px;
      padding: 12px 16px;
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
    }
  </style>
</head>
<body>
  <div id="map"></div>

  <!-- Formatted Legend -->
  <div class="legend-card">
    <div class="legend-title">Population Density</div>
    <div class="legend-sub">People per km² (Level 0)</div>
    <div class="legend-item"><div class="legend-swatch" style="background:#b30000;"></div> &gt; 350 / km²</div>
    <div class="legend-item"><div class="legend-swatch" style="background:#e34a33;"></div> 250 – 350</div>
    <div class="legend-item"><div class="legend-swatch" style="background:#fc8d59;"></div> 150 – 250</div>
    <div class="legend-item"><div class="legend-swatch" style="background:#fdcc8a;"></div> 100 – 150</div>
    <div class="legend-item"><div class="legend-swatch" style="background:#fef0d9;"></div> &lt; 100</div>
  </div>

  <script type="module" src="main.js"></script>
</body>
</html>
```

---

## 2. JavaScript Implementation

```javascript
import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.7.0/dist/maplibre-gl.mjs';

const KEY = 'YOUR_MAPTILER_API_KEY';

// Tabular demographic metrics (e.g. from API or census database)
const countryDemographics = {
  'NL': { density: 518, population: 17530000, color: '#b30000' },
  'BE': { density: 381, population: 11590000, color: '#b30000' },
  'GB': { density: 277, population: 67330000, color: '#e34a33' },
  'DE': { density: 236, population: 83200000, color: '#fc8d59' },
  'CH': { density: 218, population: 8700000,  color: '#fc8d59' },
  'IT': { density: 200, population: 59110000, color: '#fc8d59' },
  'DK': { density: 137, population: 5857000,  color: '#fdcc8a' },
  'CZ': { density: 136, population: 10510000, color: '#fdcc8a' },
  'PL': { density: 122, population: 37750000, color: '#fdcc8a' },
  'PT': { density: 112, population: 10330000, color: '#fdcc8a' },
  'AT': { density: 108, population: 8956000,  color: '#fdcc8a' },
  'FR': { density: 106, population: 67750000, color: '#fdcc8a' },
  'ES': { density: 94,  population: 47420000, color: '#fef0d9' }
};

const map = new maplibregl.Map({
  container: 'map',
  // Use Dataviz Light or Streets v4 for optimal choropleth contrast
  style: `https://api.maptiler.com/maps/dataviz-v4-light/style.json?key=${KEY}`,
  center: [8.5, 49.0],
  zoom: 4
});

// Robust lifecycle initialization helper (handles both pre-loaded and streaming styles)
function initChoropleth() {
  if (map.getSource('maptiler-countries')) return;

  // 1. Add official MapTiler Countries vector tileset
  map.addSource('maptiler-countries', {
    type: 'vector',
    url: `https://api.maptiler.com/tiles/countries/tiles.json?key=${KEY}`
  });

  // 2. Add Choropleth Fill Layer joined on iso_a2
  map.addLayer({
    id: 'maptiler-countries-choropleth',
    type: 'fill',
    source: 'maptiler-countries',
    'source-layer': 'administrative',
    filter: ['==', ['get', 'level'], 0], // Level 0 = Sovereign Nations
    paint: {
      'fill-color': [
        'match',
        ['get', 'iso_a2'],
        'NL', '#b30000',
        'BE', '#b30000',
        'GB', '#e34a33',
        'DE', '#fc8d59',
        'CH', '#fc8d59',
        'IT', '#fc8d59',
        'DK', '#fdcc8a',
        'CZ', '#fdcc8a',
        'PL', '#fdcc8a',
        'PT', '#fdcc8a',
        'AT', '#fdcc8a',
        'FR', '#fdcc8a',
        'ES', '#fef0d9',
        'rgba(255, 255, 255, 0.04)' // Fallback for countries without data
      ],
      'fill-opacity': 0.78,
      'fill-outline-color': 'rgba(255, 255, 255, 0.4)'
    }
  });

  // 3. Interactive Detail Popup reading schema attributes
  map.on('click', 'maptiler-countries-choropleth', (e) => {
    const p = e.features[0].properties;
    const meta = countryDemographics[p.iso_a2];
    if (!meta) return;

    new maplibregl.Popup()
      .setLngLat(e.lngLat)
      .setHTML(`
        <div style="font-family:sans-serif; color:#0f172a;">
          <h4 style="margin:0 0 4px; color:#0284c7;">${p['name:en'] || p.name} (${p.iso_a2})</h4>
          <div style="font-size:11px; color:#64748b; margin-bottom:6px;">ISO 3166-1: ${p.code || p.iso_a2} • Level ${p.level}</div>
          <div>Population: <b>${Number(meta.population).toLocaleString()}</b></div>
          <div>Density: <b>${meta.density} people / km²</b></div>
          <div>Surface Area: <b>${p.area ? Number(p.area).toLocaleString() + ' km²' : 'N/A'}</b></div>
          <div>Continent: <b>${p.continent || 'Europe'}</b></div>
        </div>
      `)
      .addTo(map);
  });

  // 4. Pointer Hover Feedback
  map.on('mouseenter', 'maptiler-countries-choropleth', () => {
    map.getCanvas().style.cursor = 'pointer';
  });
  map.on('mouseleave', 'maptiler-countries-choropleth', () => {
    map.getCanvas().style.cursor = '';
  });
}

// Ensure layer mounts reliably regardless of event order
if (map.isStyleLoaded()) {
  initChoropleth();
} else {
  map.on('style.load', initChoropleth);
  map.on('load', initChoropleth);
  map.on('styledata', initChoropleth);
}
```
