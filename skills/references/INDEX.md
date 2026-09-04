# MapLibre GL JS Reference Index 🗂️

> Master index and topic routing directory for all MapLibre GL JS agent references, API standards, official documentation guides, and MapTiler basemap integrations.

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

## 1. Master Reference Directory

| Document | Scope & Contents | Key Topics |
| :--- | :--- | :--- |
| [**api-classes-and-controls.md**](api-classes-and-controls.md) | Exhaustive reference for MapLibre core classes, UI controls, and methods. | `Map` methods (`flyTo`, `fitBounds`, `queryRenderedFeatures`, `project`), controls (`NavigationControl`, `GeolocateControl`, `ScaleControl`, `FullscreenControl`, `AttributionControl`), `Marker`, `Popup`, `LngLat`, `MercatorCoordinate`. |
| [**plugins-catalog.md**](plugins-catalog.md) | Comprehensive directory of third-party plugins, controls, and layer extensions. | UI controls, `@maptiler/geocoding-control`, PMTiles protocol, Three.js 3D layers, `terra-draw`, `react-map-gl/maplibre`. |
| [**style-spec-reference.md**](style-spec-reference.md) | Complete reference for MapLibre Style Specification v8. | Root properties, sources (`vector`, `raster`, `raster-dem`, `geojson`), 9 layer types (`fill`, `line`, `symbol`, `circle`, `heatmap`, `fill-extrusion`, `raster`, `hillshade`, `background`), expression DSL, light, terrain. |
| [**architecture-and-guides.md**](architecture-and-guides.md) | Deep technical guide to architecture, WebGL lifecycle, and optimization. | WebGL context limits (16 context trap), React/Next.js lifecycle cleanup (`map.remove()`), camera math (`fitBounds`, padding), `CustomLayerInterface`, PMTiles protocol, feature-state hover patterns. |
| [**official-examples-catalog.md**](official-examples-catalog.md) | Complete directory of 70+ official MapLibre examples with links and patterns. | Map basics, camera animations, source feeds, layer styling, 3D terrain/globe, annotations/labels, spatial querying. |
| [**vector-tile-schemas.md**](vector-tile-schemas.md) | MapTiler Planet v4 vector tile schema and attribute specifications. | `transportation`, `building`, `water`, `place`, `poi`, `boundary`, `contour` layers and attribute fields. |
| [**basemaps-and-terrain.md**](basemaps-and-terrain.md) | Guide to MapTiler basemap styles, high-DPI raster tiles, and 3D DEM. | `streets-v4`, `dataviz-v4-dark`, `outdoor-v4`, `satellite-v4`, `hybrid-v4`, `terrain-rgb-v2`, hillshade exaggeration. |
| [**sources-layers.md**](sources-layers.md) | Detailed usage guide for managing sources and layers dynamically via JS API. | `map.addSource()`, `map.addLayer()`, `map.setFilter()`, `map.setLayoutProperty()`, `map.setPaintProperty()`. |
| [**expressions.md**](expressions.md) | Deep dive into MapLibre expression syntax and shader logic. | Data expressions, camera expressions, step functions, interpolate, decision logic (`match`, `case`), color scales. |
| [**events.md**](events.md) | Event handling, pointer tracking, and spatial queries. | `click`, `mousemove`, `mouseenter`, `mouseleave`, `queryRenderedFeatures`, hover states. |
| [**frameworks.md**](frameworks.md) | Production integration guides for frontend frameworks. | React (`react-map-gl/maplibre`), Next.js (SSR bypass), Vue 3, Svelte (`svelte-maplibre`), Angular. |
| [**geocoding-and-services.md**](geocoding-and-services.md) | MapTiler Cloud REST API integration. | Forward search, reverse geocoding, autocomplete, static maps, elevation endpoints. |
| [**patterns-gotchas.md**](patterns-gotchas.md) | Common pitfalls and definitive solutions. | `[lng, lat]` coordinate order, container zero-height CSS, missing CSS link, context loss prevention. |
| [**prompt-benchmarks.md**](prompt-benchmarks.md) | AI assistant prompt benchmarks and test scenarios. | Standardized evaluation prompts and expected code generation patterns. |

---

## 2. Topic Routing by User Request

| If the user asks for... | Consult these references |
| :--- | :--- |
| **Adding a vector or raster basemap** | [basemaps-and-terrain.md](basemaps-and-terrain.md) & [style-spec-reference.md](style-spec-reference.md) |
| **Custom 3D terrain elevation or hillshade** | [basemaps-and-terrain.md](basemaps-and-terrain.md) & [architecture-and-guides.md](architecture-and-guides.md) |
| **Styling vector tile layers (buildings, roads, water)** | [vector-tile-schemas.md](vector-tile-schemas.md) & [sources-layers.md](sources-layers.md) |
| **Complex data-driven expressions (colors, scales, filters)** | [expressions.md](expressions.md) & [style-spec-reference.md](style-spec-reference.md) |
| **Search bars, address autocomplete, or geocoding** | [plugins-catalog.md](plugins-catalog.md) & [geocoding-and-services.md](geocoding-and-services.md) |
| **Large-scale point data, clustering, or heatmaps** | [sources-layers.md](sources-layers.md) & [architecture-and-guides.md](architecture-and-guides.md) |
| **Drawing or editing polygons, lines, points** | [plugins-catalog.md](plugins-catalog.md) (`terra-draw`) |
| **Custom 3D models or Three.js scenes** | [architecture-and-guides.md](architecture-and-guides.md) & [plugins-catalog.md](plugins-catalog.md) |
| **PMTiles or serverless cloud-native tiles** | [architecture-and-guides.md](architecture-and-guides.md) & [plugins-catalog.md](plugins-catalog.md) |
| **Integrating into React / Next.js / Vue / Svelte** | [frameworks.md](frameworks.md) & [architecture-and-guides.md](architecture-and-guides.md) |
| **Finding official examples or code templates** | [official-examples-catalog.md](official-examples-catalog.md) |
