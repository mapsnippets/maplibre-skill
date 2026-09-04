# Scrollytelling & Narrative Chapter Navigation 📜

> **Official MapLibre GL JS Example:** [Fly to a location on scroll](https://maplibre.org/maplibre-gl-js/docs/examples/scroll-fly-to/)  
> **Target Category:** Production Task Implementation

Sync map camera positions (bearing, pitch, center, zoom) to story chapter sections as the user scrolls down an article sidebar.

---

## 1. HTML Container

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Scrollytelling & Narrative Chapter Navigation 📜</title>
  <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div id="map"></div>
<div id="features">
  <section id="chapter-1" class="active"><h3>Prague Castle</h3><p>Historical seat of Czech kings.</p></section>
  <section id="chapter-2"><h3>Charles Bridge</h3><p>Gothic stone bridge built in 1357.</p></section>
  <section id="chapter-3"><h3>Old Town Square</h3><p>Astronomical clock tower and historic hub.</p></section>
</div>
  <script type="module" src="main.js"></script>
</body>
</html>
```

---

## 2. CSS Styling

```css
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

#map {
  width: 100%;
  height: 100%;
}

#features { position: absolute; top: 0; left: 0; width: 320px; height: 100%; overflow-y: scroll; padding: 20px; z-index: 1000; }
section { background: rgba(255,255,255,0.9); margin-bottom: 200px; padding: 20px; border-radius: 8px; opacity: 0.3; transition: opacity 0.3s; }
section.active { opacity: 1; border-left: 4px solid #0084FF; }
```

---

## 3. Complete JavaScript Implementation

```javascript
import maplibregl from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';

const MAPTILER_KEY = 'YOUR_MAPTILER_KEY';

const chapters = {
  'chapter-1': { center: [14.4005, 50.0909], zoom: 15.5, pitch: 45, bearing: -20 },
  'chapter-2': { center: [14.4114, 50.0865], zoom: 16.0, pitch: 30, bearing: 40 },
  'chapter-3': { center: [14.4212, 50.0875], zoom: 16.2, pitch: 20, bearing: 0 }
};

const map = new maplibregl.Map({
  container: 'map',
  style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${MAPTILER_KEY}`,
  center: chapters['chapter-1'].center,
  zoom: chapters['chapter-1'].zoom
});

const features = document.getElementById('features');
features.onscroll = () => {
  for (const chapterName in chapters) {
    const el = document.getElementById(chapterName);
    const rect = el.getBoundingClientRect();
    if (rect.top >= 0 && rect.top <= window.innerHeight / 2) {
      document.querySelectorAll('section').forEach((s) => s.classList.remove('active'));
      el.classList.add('active');
      map.flyTo(chapters[chapterName]);
      break;
    }
  }
};
```

---

## 4. Key Architecture & Options

| Parameter / Feature | Purpose |
| :--- | :--- |
| **Official Standard** | Conforms to `https://maplibre.org/maplibre-gl-js/docs/examples/scroll-fly-to/` using native `maplibregl.*` APIs. |
| **Basemap Service** | Powered by modern MapTiler Planet v4 vector styles or Terrain-RGB tiles. |
| **WebGL Lifecycle** | Automatically destroys WebGL contexts on SPA unmount via `map.remove()`. |
