# 10 Theoretical AI Agent Prompts for Geospatial & Map Skills 🗺️🤖

This catalog contains **10 realistic, production-grade developer prompts** covering diverse geospatial web and mobile use cases. Each scenario demonstrates how an AI coding agent triggers and leverages the map skill to generate modular, bug-free, and high-performance map applications.

---
> [!IMPORTANT]
> **🔑 Mandatory API Key Prompting Behavior:**
> Whenever the agent generates map code and the user has not supplied an API key (or if code contains `YOUR_API_KEY`), the agent MUST proactively remind and guide the user on how to acquire their free API key:
> *"To display the map tiles, get a free MapTiler API key (100,000 monthly tile requests) at: https://docs.maptiler.com/cloud/api/authentication-key/"*



## 1. 🏪 Retail Store Locator & Branch Finder
* **Developer Prompt:**  
  > *"Build an interactive store locator for a coffee chain with 25 locations. Include a sidebar list with search filtering, click-to-focus on markers, custom branded HTML pins, and popups displaying opening hours and phone numbers."*
* **Triggered Skill Intents:** `store locator`, `add interactive map`, `custom markers`, `search filter`, `popup`
* **Agent Architecture & Invariants:**
  * Responsive 2-column layout (sidebar + full-height map `100vh`).
  * Custom `L.divIcon` pins with CSS hover animations and badges.
  * Smooth map pan/zoom synchronization (`map.flyTo([lat, lng], 15)`) on sidebar card click.
  * Basemap default: `streets-v4`.

---

## 2. 🚚 Logistics Delivery Zones & Risk Boundary Choropleth
* **Developer Prompt:**  
  > *"Create a full-screen delivery zone manager that renders our GeoJSON polygon service areas. Color-code the zones by delivery fee (Green = Free, Yellow = $5, Red = Out of Area), add hover opacity highlights, and show a legend control in the bottom right."*
* **Triggered Skill Intents:** `render GeoJSON`, `choropleth map`, `polygon styling`, `hover events`, `custom legend`
* **Agent Architecture & Invariants:**
  * Strict GeoJSON coordinate handling (`[coord[1], coord[0]]` coordinate inversion).
  * Dynamic `style(feature)` function mapping fee tiers to color palettes.
  * Interactive `mouseover` / `mouseout` event listeners with `layer.setStyle` and `resetStyle`.
  * Custom UI legend control via `L.Control.extend` with `disableClickPropagation`.

---

## 3. 🔥 Urban Incident & Noise Complaint Heatmap
* **Developer Prompt:**  
  > *"I have a JSON array of 5,000 noise complaint coordinates with intensity values. Render a fast canvas heatmap on the map with a smooth blue-to-red gradient and add a slider to adjust the blur radius."*
* **Triggered Skill Intents:** `heatmap`, `density visualization`, `large dataset`, `canvas layer`
* **Agent Architecture & Invariants:**
  * HTML5 Canvas rendering via `leaflet.heat` / `L.heatLayer`.
  * Multi-stop color gradient (`#0000ff` to `#ff0000`) and intensity weighting `[lat, lng, intensity]`.
  * DOM slider listener dynamically updating `.setOptions({ radius, blur })`.
  * High-DPI 512px raster tiles with `tileSize: 512, zoomOffset: -1`.

---

## 4. 🏢 Multi-Location Cluster Map for 2,000+ EV Charging Stations
* **Developer Prompt:**  
  > *"Render 2,500 EV charging stations across Europe without lagging the browser. Group close points into cluster bubbles showing station counts, spiderfy on click when at max zoom, and filter stations by connector type (Type 2 vs CCS)."*
* **Triggered Skill Intents:** `marker clustering`, `spiderfy`, `bulk data performance`, `filter markers`
* **Agent Architecture & Invariants:**
  * Fast spatial point aggregation via `leaflet.markercluster` (`L.markerClusterGroup`).
  * Custom cluster bubble design via `iconCreateFunction` with dynamic diameter and color tiers.
  * Bulk ingestion using `addLayers(markerArray)` to prevent DOM bottlenecks.
  * Dynamic category filtering (`Type 2` vs `CCS`) with `.clearLayers()` and batch re-addition.

---

## 5. 🥾 Outdoor Hiking Trail & Elevation Profile Viewer
* **Developer Prompt:**  
  > *"Build a mountain trail guide page that loads a GPX or GeoJSON hiking route in the Swiss Alps. Style the path according to SAC hiking difficulty (T1 yellow to T3 red), display waypoints with custom summit icons, and fit the map bounds to the trail."*
* **Triggered Skill Intents:** `outdoor recreation`, `GPX/GeoJSON trail`, `hiking path styling`, `fit bounds`
* **Agent Architecture & Invariants:**
  * Direct GPX/GeoJSON ingestion via `omnivore.gpx` or `L.geoJSON`.
  * Visual terrain styling on `outdoor-v4` topographic basemap with elevation contour lines.
  * Custom summit and alpine hut icons via `L.divIcon`.
  * Viewport fitting: `map.fitBounds(trailLayer.getBounds(), { padding: [40, 40] })`.

---

## 6. 🛰️ Split-Screen Satellite vs. Street Imagery Comparison
* **Developer Prompt:**  
  > *"Create an interactive before-and-after comparison tool with a vertical split slider that lets users swipe between a high-resolution satellite imagery layer and a modern street map."*
* **Triggered Skill Intents:** `swipe comparison`, `side by side`, `satellite view`, `tile layers`
* **Agent Architecture & Invariants:**
  * Dual tile layer configuration: `streets-v4` and `satellite-v4`.
  * Interactive swipe container via `leaflet-side-by-side` (`L.control.sideBySide(layerA, layerB)`).
  * Seamless touch and pointer dragging across mobile and desktop viewports.

---

## 7. 🚌 Real-Time Transit & Fleet Vehicle Tracker
* **Developer Prompt:**  
  > *"Create a live city bus tracker that polls a REST API every 3 seconds for active vehicle positions. Smoothly animate markers to their new coordinates and rotate the bus icon based on heading."*
* **Triggered Skill Intents:** `real-time tracking`, `live markers`, `polling`, `animated positions`
* **Agent Architecture & Invariants:**
  * Polling lifecycle via `leaflet-realtime` or `setInterval` with vehicle cache dictionary.
  * Animated marker transitions between GPS coordinate updates.
  * Dynamic SVG bus markers rotated in real time via CSS `transform: rotate(${heading}deg)`.
  * Popup binding displaying speed, next station, and delay telemetry.

---

## 8. ✏️ Spatial Geo-fencing & Polygon Drawing Tool
* **Developer Prompt:**  
  > *"Add a drawing toolbar to our dashboard that allows users to draw custom polygon geo-fences, edit existing vertices, measure area in square kilometers, and export the drawn shapes to GeoJSON."*
* **Triggered Skill Intents:** `drawing tools`, `draw polygons`, `edit geometry`, `export GeoJSON`, `spatial measurement`
* **Agent Architecture & Invariants:**
  * Drawing controls via `@geoman-io/leaflet-geoman-free` (`map.pm.addControls`).
  * Shape creation listener `map.on('pm:create')` extracting `layer.toGeoJSON()`.
  * Real-time geodesic area calculation in $km^2$.
  * Export modal with copyable GeoJSON FeatureCollection payload.

---

## 9. 🌙 Dark-Themed Web Analytics Geo-Dashboard
* **Developer Prompt:**  
  > *"We are building a dark-mode SaaS analytics dashboard. Add a sleek dark vector basemap showing customer signups by country, with glowing cyan neon points and an interactive tooltip on hover."*
* **Triggered Skill Intents:** `dark mode map`, `vector basemap`, `glowing markers`, `tooltips`
* **Agent Architecture & Invariants:**
  * Vector basemap integration via `@maplibre/maplibre-gl-leaflet` with `dataviz-v4-dark`.
  * Custom neon glowing HTML markers with CSS `box-shadow: 0 0 15px #00d2ff`.
  * Sticky tooltips: `layer.bindTooltip("<b>" + name + "</b>: " + count, { sticky: true })`.

---

## 10. ⚛️ Next.js (App Router) / React Responsive Map Component
* **Developer Prompt:**  
  > *"Create a clean, reusable React map component in Next.js 14 (App Router) that avoids 'window is not defined' SSR crashes, handles browser resizing without grey tile artifacts, and cleans up on unmount."*
* **Triggered Skill Intents:** `React map`, `Next.js map`, `react-leaflet`, `SSR fix`, `invalidateSize`, `map cleanup`
* **Agent Architecture & Invariants:**
  * Next.js App Router client component (`"use client"`).
  * SSR bypass via `next/dynamic(() => import(...), { ssr: false })`.
  * Container height CSS rules (`height: 100%; width: 100%`) and `map.invalidateSize()` on resize.
  * Complete lifecycle teardown in `useEffect(() => { ... return () => map.remove(); }, [])` to prevent `Map container is already initialized` memory leaks.
