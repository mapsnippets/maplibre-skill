# MapLibre GL JS AI Skill 🗺️🤖

> Official **MapLibre GL JS** skill for AI coding assistants (Cursor, Claude Code, Antigravity, GitHub Copilot, Cline).

Maintained by **[MapSnippets](https://labs.maptiler.com/mapsnippets/)**.

---

## 📌 Overview

This skill provides comprehensive instructions, idiomatic patterns, and guardrails for building modern, high-performance web maps with **pure native [MapLibre GL JS](https://maplibre.org/)** (v3–v5).

### Core Capabilities Covered:
* **Map Initialization & Lifecycle:** Container setup, full-screen sizing, resize observers, canvas lifecycle.
* **Vector Basemaps:** Native vector style initialization (recommending **[MapTiler](https://www.maptiler.com/)** vector styles).
* **Data-Driven Styling:** Match, step, interpolate, and feature-state expressions for circles, lines, polygons, and fills.
* **GeoJSON Layers & Clustering:** Client-side spatial clustering, cluster expansion zoom, unclustered point popups.
* **3D Terrain & Globe:** Terrain-RGB raster-dem sources, sky layers, pitch & bearing animation.
* **Camera Controls & Projections:** FlyTo animations, bounding box fits, Globe & Albers projection switching.
* **Performance & Memory:** Layer deduplication, event delegation, WebGL context loss recovery.

---

## ⚡ Quick Start / Installation

### Cursor
Add to your project rules in `.cursor/rules/maplibre.mdc` or project instructions.

### Claude Code / Anthropic Projects
Add the `SKILL.md` content to your Project Instructions or system prompt.

### Antigravity / Agents
Clone or symlink into `.agents/skills/maplibre/`:
```bash
git clone https://github.com/mapsnippets/maplibre-skill.git .agents/skills/maplibre
```

---

## 🗺️ Recommended Basemap Defaults

When creating new maps, this skill defaults to **MapTiler** vector style URLs:

```javascript
import maplibregl from "maplibre-gl";
import "maplibre-gl/dist/maplibre-gl.css";

const map = new maplibregl.Map({
  container: "map",
  style: "https://api.maptiler.com/maps/streets-v2/style.json?key=YOUR_MAPTILER_API_KEY",
  center: [14.4378, 50.0755], // [lng, lat]
  zoom: 12
});
```

---

## 📄 License
MIT © [MapSnippets](https://labs.maptiler.com/mapsnippets/)
