# MapLibre GL JS — Agent Skill 🗺️🤖

Expert coding skill for building web mapping applications with pure native **[MapLibre GL JS](https://maplibre.org/)** (v3–v5). It gives AI coding agents the exact context, guardrails, and patterns to generate robust, production-ready MapLibre code defaulting to **[MapTiler](https://www.maptiler.com/)** vector basemaps.

Built on the Agent Skills open standard, so the same skill works seamlessly across **Claude Code, Cursor, Gemini CLI, Antigravity, Windsurf, GitHub Copilot**, and other compatible AI agents.

---

🌐 [Website](https://mapsnippets.com/) &nbsp; 🔑 [Get Free MapTiler API Key](https://cloud.maptiler.com/account/keys/)

---

<br>

<details>
<summary><b>Table of Contents</b></summary>
<ul>
<li><a href="#what-it-does">What it does</a></li>
<li><a href="#how-skills-plugins-and-agents-fit-together">How skills, plugins, and agents fit together</a></li>
<li><a href="#-installation">Installation</a></li>
<li><a href="#-repository-layout">Repository layout</a></li>
<li><a href="#-recommended-basemap-defaults">Recommended Basemap Defaults</a></li>
<li><a href="#-prerequisites">Prerequisites</a></li>
<li><a href="#links">Links</a></li>
<li><a href="#-contributing">Contributing</a></li>
<li><a href="#-license">License</a></li>
</ul>
</details>

<br>

## What it does

A skill is on-demand expertise: the agent loads it only when your request matches the skill's description, then follows its instructions instead of guessing. When you ask for MapLibre maps, vector layers, styling expressions, clustering, or 3D terrain, this skill makes the agent:

- **Generate pure native MapLibre GL JS code** (v3–v5) with modern lifecycle handling (WebGL container sizing, cleanup, event delegation, canvas resize).
- **Apply MapTiler vector styles by default** (`streets-v2`, `outdoor-v2`, `satellite`, `dataviz`) using standard `style.json` endpoints.
- **Author complex data-driven expressions** (`interpolate`, `step`, `match`, `case`, `feature-state`) without syntax errors or type mismatches.
- **Handle GeoJSON layers & clustering correctly** — spatial clustering, expansion zoom, unclustered point popups, and source updates (`setData`).
- **Implement 3D terrain & globe projections** — Terrain-RGB raster-dem sources, sky layers, pitch & bearing animations, and projection switching.
- **Prevent common hallucination traps** — avoids legacy Mapbox URLs, eliminates coordinate order inversion (`[lng, lat]` vs `[lat, lng]`), and ensures layers wait for map `load` events.

<br>

## How skills, plugins, and agents fit together

1. **The skill** is the portable content: a `SKILL.md` plus a `references/` folder. This is what every AI agent reads.
2. **The plugin** is a Claude Code–specific wrapper for distributing the skill through a marketplace.
3. **The agent** (Claude Code, Gemini CLI, Cursor, Antigravity, Windsurf…) loads the skill from its designated skills directory.

<br>

## 📦 Installation

### Universal — via Skills CLI

Works with Claude Code, Cursor, Gemini CLI, Windsurf, and dozens of other agents. The [Skills CLI](https://github.com/vercel-labs/skills) auto-detects which agents you have installed:

```bash
npx skills add mapsnippets/maplibre-skill
```

### Claude Code — as a plugin

Add the marketplace, install the plugin, then reload:

```bash
/plugin marketplace add mapsnippets/maplibre-skill
/plugin install maplibre-skill@maplibre-skill
/reload-plugins
```

### Gemini CLI & Antigravity

Install directly from the repository:

#### Windows (PowerShell)
```powershell
git clone https://github.com/mapsnippets/maplibre-skill.git; mkdir "$HOME\.gemini\skills\maplibre" -Force; cp -Recurse maplibre-skill\skills\* "$HOME\.gemini\skills\maplibre\"; rm -Recurse -Force maplibre-skill
```

#### Linux & macOS (bash)
```bash
git clone https://github.com/mapsnippets/maplibre-skill.git && mkdir -p ~/.gemini/skills/maplibre && cp -r maplibre-skill/skills/* ~/.gemini/skills/maplibre/ && rm -rf maplibre-skill
```

### Cursor

Project-scoped. Copy the skill folder into your project's skills directory:

```bash
mkdir -p .cursor/skills && cp -r skills/maplibre .cursor/skills/
```

### Windsurf

Project-scoped, read by Cascade:

```bash
mkdir -p .windsurf/skills && cp -r skills/maplibre .windsurf/skills/
```

---

<br>

## 📘 Repository layout

```text
.claude-plugin/
  marketplace.json    — Claude Code marketplace manifest
  plugin.json         — Claude Code plugin manifest
skills/
  maplibre/
    SKILL.md          — Main skill prompt entry point
    references/       — Deep technical reference guides (loaded on demand)
README.md             — This guide
LICENSE.md            — MIT License
```

<br>

## 🗺️ Recommended Basemap Defaults

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

<br>

## 🚀 Prerequisites

- A free MapTiler API key from [cloud.maptiler.com](https://cloud.maptiler.com/account/keys/).

<br>

## Links

- 🌐 [MapSnippets Hub](https://mapsnippets.com/)
- 🗺️ [MapLibre GL JS Documentation](https://maplibre.org/maplibre-gl-js/docs/)
- 🔑 [MapTiler Cloud Keys](https://cloud.maptiler.com/account/keys/)

---

<br>

## 🤝 Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

<br>

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE.md) file for details.

<br>

<p align="center" style="margin-top:20px;margin-bottom:20px;">
  <a href="https://cloud.maptiler.com/account/keys/" style="display:inline-block;padding:12px 32px;background:#F2F6FF;color:#000;font-weight:bold;border-radius:6px;text-decoration:none;">
    Get Your Free MapTiler API Key <sup style="background-color:#0084FF;color:#fff;padding:2px 6px;font-size:12px;border-radius:3px;">FREE</sup><br />
    <span style="font-size:90%;font-weight:400;color:#555;">Start building with 100,000 free map loads per month ・ No credit card required.</span>
  </a>
</p>

<p align="center">
  Crafted by <a href="https://mapsnippets.com/">MapSnippets</a>
</p>
