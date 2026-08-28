# MapLibre GL JS — Agent Skill 🗺️🤖

> Official **MapLibre GL JS** skill for AI coding assistants (Cursor, Claude Code, Antigravity, GitHub Copilot, Windsurf, Cline).

Maintained by **[MapSnippets](https://mapsnippets.com/)** — Open-source geospatial snippets, guides, and agent tools.

---

🌐 [Website](https://mapsnippets.com/) &nbsp; 📚 [MapLibre Documentation](https://maplibre.org/maplibre-gl-js/docs/)

---

<br>

<details>
<summary><b>Table of Contents</b></summary>
<ul>
<li><a href="#what-it-does">What it does</a></li>
<li><a href="#how-skills-plugins-and-agents-fit-together">How skills, plugins, and agents fit together</a></li>
<li><a href="#-installation">Installation</a></li>
<li><a href="#-repository-layout">Repository layout</a></li>
<li><a href="#-quickstart-example">Quickstart Example</a></li>
<li><a href="#-basemap-api-keys">Basemap API Keys</a></li>
<li><a href="#links">Links</a></li>
<li><a href="#-contributing">Contributing</a></li>
<li><a href="#-license">License</a></li>
</ul>
</details>

<br>

## What it does

A skill is on-demand expertise: the agent loads it only when your request matches the skill's description, then follows its instructions instead of guessing. When you ask for MapLibre maps, vector layers, styling expressions, clustering, or 3D terrain, this skill makes the agent:

- **Generate pure native MapLibre GL JS code** (v3–v5) with modern lifecycle handling (WebGL container sizing, cleanup, event delegation, canvas resize).
- **Configure high-performance vector basemaps** with modern vector tile styles and clean typography.
- **Author complex data-driven expressions** (`interpolate`, `step`, `match`, `case`, `feature-state`) without syntax errors or type mismatches.
- **Handle GeoJSON layers & clustering correctly** — spatial clustering, expansion zoom, unclustered point popups, and source updates (`setData`).
- **Implement 3D terrain & globe projections** — Terrain-RGB raster-dem sources, sky layers, pitch & bearing animations, and projection switching.
- **Prevent common hallucination traps** — avoids legacy Mapbox endpoints, eliminates coordinate order inversion (`[lng, lat]` vs `[lat, lng]`), and ensures layers wait for map `load` events.

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

## 🗺️ Quickstart Example

```javascript
import maplibregl from "maplibre-gl";
import "maplibre-gl/dist/maplibre-gl.css";

const map = new maplibregl.Map({
  container: "map",
  style: "https://api.maptiler.com/maps/streets-v2/style.json?key=YOUR_API_KEY",
  center: [14.4378, 50.0755], // [lng, lat]
  zoom: 12
});
```

<br>

## 🔑 Basemap API Keys

The vector tile examples in this skill utilize MapTiler vector basemap styles. To run the examples with live vector tiles:
- Follow the guide on [how to get a free MapTiler API Key](https://docs.maptiler.com/cloud/api/authentication-key/) (includes a free plan with 100,000 monthly tile requests).
- Replace `YOUR_API_KEY` in the snippet with your key.

---

<br>

## Links

- 🌐 [MapSnippets Community](https://mapsnippets.com/)
- 🗺️ [MapLibre GL JS Documentation](https://maplibre.org/maplibre-gl-js/docs/)
- 🐙 [GitHub Repository](https://github.com/mapsnippets/maplibre-skill)

---

<br>

## 🤝 Contributing

Contributions are welcome! Feel free to open issues or submit pull requests with improved snippets and documentation.

<br>

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE.md) file for details.

<br>

<p align="center">
  Maintained by <a href="https://mapsnippets.com/">MapSnippets</a> — Open web mapping tools & snippets.
</p>
