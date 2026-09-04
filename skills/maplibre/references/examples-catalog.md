# MapLibre GL JS — Complete Official Examples Catalog & Recipe Matrix 📚🗺️

> Exhaustive index of all **140 official MapLibre GL JS examples** from [`maplibre.org/maplibre-gl-js/docs/examples/`](https://maplibre.org/maplibre-gl-js/docs/examples/) cross-referenced with MapSnippets production recipes in [`skills/maplibre/examples/`](../examples/INDEX.md).

---

## 🧭 Catalog Architecture & Taxonomy

The 140 official examples are categorized into 5 primary groups and 21 subcategories:
1. **Map Basics** (35 examples): Getting Started, Camera & Animation, Controls & Gestures
2. **Sources** (17 examples): Vector & GeoJSON, Raster & Imagery, Live & Realtime Data
3. **Layers** (52 examples): Icons & Symbols, Layer Styling, Expressions, Filtering, Heatmaps & Clusters, Data Visualization, Terrain & Hillshade, 3D Models & Buildings, Globe
4. **Annotations & Labels** (20 examples): Markers, Popups, Labels & Fonts, Internationalization
5. **Interactivity & Extensions** (16 examples): Events & Queries, Plugins & Integrations

---

## 📁 Map Basics

### 🏷️ Getting Started (6 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Display a map](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-map/)** | Initialize a map in an HTML element with MapLibre GL JS. | [`display-vector-map.md`](../examples/display-vector-map.md) |
| **[Display a satellite map](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-satellite-map/)** | Display a satellite raster baselayer. | [`satellite-hybrid-terrain.md`](../examples/satellite-hybrid-terrain.md) |
| **[Display a non-interactive map](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-non-interactive-map/)** | Disable interactivity to create a static map. | — |
| **[Change the default position for attribution](https://maplibre.org/maplibre-gl-js/docs/examples/change-the-default-position-for-attribution/)** | Place attribution in the top-left position when initializing a map. | — |
| **[Check if WebGL is supported](https://maplibre.org/maplibre-gl-js/docs/examples/check-if-webgl-is-supported/)** | Check for WebGL browser support. | — |
| **[Display a map with MLT](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-map-with-mlt/)** | Initialize a map in an HTML element with MapLibre GL JS. This example is using a style that uses MLT (MapLibre Tiles). The example is very similar to the regular one except the tiles are in a different encoding. | — |

### 🏷️ Camera & Animation (21 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Fly to a location](https://maplibre.org/maplibre-gl-js/docs/examples/fly-to-a-location/)** | Use flyTo to smoothly interpolate between locations. | [`fly-to-camera.md`](../examples/fly-to-camera.md) |
| **[Jump to a series of locations](https://maplibre.org/maplibre-gl-js/docs/examples/jump-to-a-series-of-locations/)** | Use the jumpTo function to showcase multiple locations. | — |
| **[Set pitch and bearing](https://maplibre.org/maplibre-gl-js/docs/examples/set-pitch-and-bearing/)** | Initialize a map with pitch and bearing camera options. | — |
| **[Slowly fly to a location](https://maplibre.org/maplibre-gl-js/docs/examples/slowly-fly-to-a-location/)** | Use flyTo with flyOptions to slowly zoom to a location. | — |
| **[Animate a line](https://maplibre.org/maplibre-gl-js/docs/examples/animate-a-line/)** | Animate a line by updating a GeoJSON source on each frame. | — |
| **[Animate a point](https://maplibre.org/maplibre-gl-js/docs/examples/animate-a-point/)** | Animate the position of a point by updating a GeoJSON source on each frame. | — |
| **[Animate a point along a route](https://maplibre.org/maplibre-gl-js/docs/examples/animate-a-point-along-a-route/)** | Use Turf to smoothly animate a point along the distance of a line. | [`animate-point-along-route.md`](../examples/animate-point-along-route.md) |
| **[Animate map camera around a point](https://maplibre.org/maplibre-gl-js/docs/examples/animate-map-camera-around-a-point/)** | Animate the map camera around a point. | [`camera-orbit-360.md`](../examples/camera-orbit-360.md) |
| **[Customize camera animations](https://maplibre.org/maplibre-gl-js/docs/examples/customize-camera-animations/)** | Customize camera animations using AnimationOptions. | — |
| **[Customize the map transform constrain](https://maplibre.org/maplibre-gl-js/docs/examples/customize-the-map-transform-constrain/)** | Customize the constrain callback of the map transform. For example, to allow users to underzoom and overpan the bounds. | — |
| **[Enter a 360° photosphere](https://maplibre.org/maplibre-gl-js/docs/examples/enter-a-360-photosphere/)** `NEW` | Click a marker to smoothly enter an immersive 360° photosphere at a fixed point, blended with the live map; exit to return to normal navigation. | — |
| **[Fit a map to a bounding box](https://maplibre.org/maplibre-gl-js/docs/examples/fit-a-map-to-a-bounding-box/)** | Fit the map to a specific area, regardless of the pixel size of the map. | [`fit-bounds-padding.md`](../examples/fit-bounds-padding.md) |
| **[Fit to the bounds of a LineString](https://maplibre.org/maplibre-gl-js/docs/examples/fit-to-the-bounds-of-a-linestring/)** | Get the bounds of a LineString. | — |
| **[Fly to a location based on scroll position](https://maplibre.org/maplibre-gl-js/docs/examples/fly-to-a-location-based-on-scroll-position/)** | Scroll down through the story and the map will fly to the chapter's location. | [`scroll-driven-fly-to.md`](../examples/scroll-driven-fly-to.md) |
| **[Hash routing](https://maplibre.org/maplibre-gl-js/docs/examples/hash-routing/)** | Keep the viewport state in the url with hash routing. | — |
| **[Level of Detail Control](https://maplibre.org/maplibre-gl-js/docs/examples/level-of-detail-control/)** | Modify how Level of Detail behaves at high pitch angles. | — |
| **[Offset the vanishing point using padding](https://maplibre.org/maplibre-gl-js/docs/examples/offset-the-vanishing-point-using-padding/)** | Offset the center or vanishing point of the map to reduce distortion when floating elements are displayed over the map. | — |
| **[Render world copies](https://maplibre.org/maplibre-gl-js/docs/examples/render-world-copies/)** | Toggle between rendering a single world and multiple copies of the world using setRenderWorldCopies. | — |
| **[Restrict map panning to an area](https://maplibre.org/maplibre-gl-js/docs/examples/restrict-map-panning-to-an-area/)** | Prevent a map from being panned to a different place by setting maxBounds. | — |
| **[Set center point above ground](https://maplibre.org/maplibre-gl-js/docs/examples/set-center-point-above-ground/)** | Set the center point above ground level. | — |
| **[Walk around a map in first person](https://maplibre.org/maplibre-gl-js/docs/examples/walk-around-a-map-in-first-person/)** `NEW` | Drive an eye-height, level first-person camera over 3D buildings with the keyboard or on-screen buttons | — |

### 🏷️ Controls & Gestures (8 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Display map navigation controls](https://maplibre.org/maplibre-gl-js/docs/examples/display-map-navigation-controls/)** | Add zoom and rotation controls to the map. | [`display-vector-map.md`](../examples/display-vector-map.md) |
| **[Locate the user](https://maplibre.org/maplibre-gl-js/docs/examples/locate-the-user/)** | Geolocate the user and then track their current location on the map using the GeolocateControl. | [`custom-geolocate-control.md`](../examples/custom-geolocate-control.md) |
| **[Cooperative gestures](https://maplibre.org/maplibre-gl-js/docs/examples/cooperative-gestures/)** | Enable cooperative gestures. See how it behaves in fullscreen mode. | — |
| **[Disable map rotation](https://maplibre.org/maplibre-gl-js/docs/examples/disable-map-rotation/)** | Prevent users from rotating a map. | — |
| **[Disable scroll zoom](https://maplibre.org/maplibre-gl-js/docs/examples/disable-scroll-zoom/)** | Prevent scroll from zooming a map. | — |
| **[Navigate the map with game-like controls](https://maplibre.org/maplibre-gl-js/docs/examples/navigate-the-map-with-game-like-controls/)** | Use the keyboard's arrow keys to move around the map with game-like controls. | — |
| **[Toggle interactions](https://maplibre.org/maplibre-gl-js/docs/examples/toggle-interactions/)** | Enable or disable UI handlers on a map. | — |
| **[View a fullscreen map](https://maplibre.org/maplibre-gl-js/docs/examples/view-a-fullscreen-map/)** | Toggle between current view and fullscreen mode. Does not work on iPhones because a pseudo-fullscreen is used, and the code is embedded in an iframe, which prevents the map from scaling. | — |

---

## 📁 Sources

### 🏷️ Vector & GeoJSON (8 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Add a vector tile source](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-vector-tile-source/)** | Add a vector source to a map. | — |
| **[Add a GeoJSON line](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-geojson-line/)** | Add a GeoJSON line to a map using addSource, then style it using addLayer’s paint properties. | — |
| **[Add a GeoJSON polygon](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-geojson-polygon/)** | Style a polygon with the fill layer type. | — |
| **[Draw GeoJSON points](https://maplibre.org/maplibre-gl-js/docs/examples/draw-geojson-points/)** | Draw points from a GeoJSON collection to a map. | — |
| **[Add multiple geometries from one GeoJSON source](https://maplibre.org/maplibre-gl-js/docs/examples/add-multiple-geometries-from-one-geojson-source/)** | Add a polygon and circle layer from the same GeoJSON source. | [`multiple-geometries-one-source.md`](../examples/multiple-geometries-one-source.md) |
| **[Display line that crosses 180th meridian](https://maplibre.org/maplibre-gl-js/docs/examples/display-line-that-crosses-180th-meridian/)** | Draw a line across the 180th meridian using a GeoJSON source. | — |
| **[View local GeoJSON](https://maplibre.org/maplibre-gl-js/docs/examples/view-local-geojson/)** | View local GeoJSON without server upload. | — |
| **[View local GeoJSON (experimental)](https://maplibre.org/maplibre-gl-js/docs/examples/view-local-geojson-experimental/)** | View local GeoJSON with experimental File System Access API. | — |

### 🏷️ Raster & Imagery (6 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Add a raster tile source](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-raster-tile-source/)** | Add a third-party raster source to the map. | — |
| **[Add a WMS source](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-wms-source/)** | Add an external Web Map Service raster layer to the map using addSource's tiles option. | [`wms-raster-source.md`](../examples/wms-raster-source.md) |
| **[Add a canvas source](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-canvas-source/)** | Add a canvas source to the map. | [`add-canvas-source.md`](../examples/add-canvas-source.md) |
| **[Add a COG raster source](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-cog-raster-source/)** | Add an external Cloud Optimized Geotiff (COG) as source. | — |
| **[Add a video](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-video/)** | Display a video on top of a satellite raster baselayer. | [`video-on-a-map.md`](../examples/video-on-a-map.md) |
| **[Animate a series of images](https://maplibre.org/maplibre-gl-js/docs/examples/animate-a-series-of-images/)** | Use a series of image sources to create an animation. | — |

### 🏷️ Live & Realtime Data (3 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Add live realtime data](https://maplibre.org/maplibre-gl-js/docs/examples/add-live-realtime-data/)** | Use realtime GeoJSON data streams to move a symbol on your map. | [`streaming-realtime-geojson.md`](../examples/streaming-realtime-geojson.md) |
| **[Update a feature in realtime](https://maplibre.org/maplibre-gl-js/docs/examples/update-a-feature-in-realtime/)** | Change an existing feature on your map in real-time by updating its data. | — |
| **[Update GeoJSON polygons](https://maplibre.org/maplibre-gl-js/docs/examples/update-geojson-polygons/)** | Update GeoJSON polygons using updateable GeoJSONVT | — |

---

## 📁 Layers

### 🏷️ Icons & Symbols (9 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Add an icon to the map](https://maplibre.org/maplibre-gl-js/docs/examples/add-an-icon-to-the-map/)** | Add an icon to the map from an external URL and use it in a symbol layer. | — |
| **[Add a generated icon to the map](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-generated-icon-to-the-map/)** | Add an icon to the map that was generated at runtime. | — |
| **[Add a stretchable image to the map](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-stretchable-image-to-the-map/)** | Use a stretchable image as a background for text. | — |
| **[](https://maplibre.org/maplibre-gl-js/docs/examples/add-an-animated-icon-to-the-map/)** |  | — |
| **[](https://maplibre.org/maplibre-gl-js/docs/examples/animate-an-icon-on-the-gpu/)** |  | [`pulsing-gpu-marker.md`](../examples/pulsing-gpu-marker.md) |
| **[Display a remote SVG symbol](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-remote-svg-symbol/)** | Uses a missing style image resolver to load a remote image and use it. | — |
| **[Elevate symbols above the terrain](https://maplibre.org/maplibre-gl-js/docs/examples/elevate-symbols-above-the-terrain/)** `NEW` | Use the symbol-height-offset property to float icons and text above the ground in 3D. | — |
| **[Generate and add a missing icon to the map](https://maplibre.org/maplibre-gl-js/docs/examples/generate-and-add-a-missing-icon-to-the-map/)** | Dynamically generate a missing icon at runtime and add it to the map. | — |
| **[Use a fallback image](https://maplibre.org/maplibre-gl-js/docs/examples/use-a-fallback-image/)** | Use a coalesce expression to display another image when a requested image is not available. | — |

### 🏷️ Layer Styling (4 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Change a layer's color with buttons](https://maplibre.org/maplibre-gl-js/docs/examples/change-a-layers-color-with-buttons/)** | Use setPaintProperty to change a layer's fill color. | — |
| **[Add a new layer below labels](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-new-layer-below-labels/)** | Use the second argument of addLayer to add a layer below labels. | [`add-layer-below-labels.md`](../examples/add-layer-below-labels.md) |
| **[Add a custom style layer](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-custom-style-layer/)** | Use a custom style layer to render custom WebGL content. | — |
| **[Add a pattern to a polygon](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-pattern-to-a-polygon/)** | Use fill-pattern to draw a polygon from a repeating image pattern. | — |

### 🏷️ Expressions (4 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Style lines with a data-driven property](https://maplibre.org/maplibre-gl-js/docs/examples/style-lines-with-a-data-driven-property/)** | Create a visualization with a data expression for line-color. | — |
| **[Change building color based on zoom level](https://maplibre.org/maplibre-gl-js/docs/examples/change-building-color-based-on-zoom-level/)** | Use the interpolate expression to ease-in the building layer and smoothly fade from one color to the next. | — |
| **[Create a gradient dashed line using an expression](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-gradient-dashed-line-using-an-expression/)** | Use the line-gradient and line-dasharray paint properties together to create a dashed line with gradient colors. | — |
| **[Create a gradient line using an expression](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-gradient-line-using-an-expression/)** | Use the line-gradient paint property and an expression to visualize distance from the starting point of a line. | — |

### 🏷️ Filtering (4 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Filter within a Layer](https://maplibre.org/maplibre-gl-js/docs/examples/filter-within-a-layer/)** | Filter a layer based on user input using setFilter(). | — |
| **[Filter symbols by text input](https://maplibre.org/maplibre-gl-js/docs/examples/filter-symbols-by-text-input/)** | Filter symbols by icon name by typing in a text input. | [`filter-features-slider.md`](../examples/filter-features-slider.md) |
| **[Filter layer symbols using global state](https://maplibre.org/maplibre-gl-js/docs/examples/filter-layer-symbols-using-global-state/)** | Filter a layer symbols based on user input using setGlobalStateProperty(). | — |
| **[Filter symbols by toggling a list](https://maplibre.org/maplibre-gl-js/docs/examples/filter-symbols-by-toggling-a-list/)** | Filter a set of symbols based on a property value in the data. | — |

### 🏷️ Heatmaps & Clusters (3 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Create a heatmap layer](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-heatmap-layer/)** | Visualize earthquake frequency by location using a heatmap layer. | [`heatmap-layer.md`](../examples/heatmap-layer.md) |
| **[Create and style clusters](https://maplibre.org/maplibre-gl-js/docs/examples/create-and-style-clusters/)** | Use MapLibre GL JS' built-in functions to visualize points as clusters. | [`marker-clustering.md`](../examples/marker-clustering.md) |
| **[Display HTML clusters with custom properties](https://maplibre.org/maplibre-gl-js/docs/examples/display-html-clusters-with-custom-properties/)** | Extend clustering with HTML markers and custom property expressions. | — |

### 🏷️ Data Visualization (4 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Visualize population density](https://maplibre.org/maplibre-gl-js/docs/examples/visualize-population-density/)** | Use a variable binding expression to calculate and display population density. | — |
| **[Measure distances](https://maplibre.org/maplibre-gl-js/docs/examples/measure-distances/)** | Click points on a map to create lines that measure distanced using turf.length. | [`turf-distance-measurement.md`](../examples/turf-distance-measurement.md) |
| **[Create a time slider](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-time-slider/)** | Visualize earthquakes with a range slider. | — |
| **[Draw a Circle](https://maplibre.org/maplibre-gl-js/docs/examples/draw-a-circle/)** | Draw a radius to approximate a location with Turf.js | — |

### 🏷️ Terrain & Hillshade (8 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[3D Terrain](https://maplibre.org/maplibre-gl-js/docs/examples/3d-terrain/)** | Go beyond hillshade and show elevation in actual 3D. | [`3d-terrain-elevation.md`](../examples/3d-terrain-elevation.md) |
| **[Add a hillshade layer](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-hillshade-layer/)** | Add a simple hillshade layer. | — |
| **[Add 3D terrain from quantized-mesh tiles](https://maplibre.org/maplibre-gl-js/docs/examples/add-3d-terrain-from-quantized-mesh-tiles/)** `NEW` | Use 3D Tiles quantized-mesh terrain as a raster-dem source via the maplibre-gl-3dtiles-terrain plugin, then drape an OpenStreetMap basemap and hillshade with map.setTerrain(). | — |
| **[Add a color relief layer](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-color-relief-layer/)** | Add a color relief layer. | [`color-relief-layer.md`](../examples/color-relief-layer.md) |
| **[Add a multidirectional hillshade layer](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-multidirectional-hillshade-layer/)** | Add a hillshade layer with multiple illumination sources. | [`multidirectional-hillshade.md`](../examples/multidirectional-hillshade.md) |
| **[Add Contour Lines](https://maplibre.org/maplibre-gl-js/docs/examples/add-contour-lines/)** | Add contour lines to your map from a raster-dem source. | [`vector-contour-lines.md`](../examples/vector-contour-lines.md) |
| **[Display a hybrid satellite map with terrain elevation](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-hybrid-satellite-map-with-terrain-elevation/)** | Display a hybrid satellite map with terrain elevation. | — |
| **[Sky, Fog, Terrain](https://maplibre.org/maplibre-gl-js/docs/examples/sky-fog-terrain/)** | Allows changing the sky, fog and horizon color and blends. | — |

### 🏷️ 3D Models & Buildings (9 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Display buildings in 3D](https://maplibre.org/maplibre-gl-js/docs/examples/display-buildings-in-3d/)** | Use extrusions to display buildings' height in 3D. | [`3d-buildings-extrusion.md`](../examples/3d-buildings-extrusion.md) |
| **[Add a 3D model using three.js](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-using-threejs/)** | Use a custom style layer with three.js to add a 3D model to the map. | [`custom-layer-threejs.md`](../examples/custom-layer-threejs.md) |
| **[Add 3D tiles using three.js](https://maplibre.org/maplibre-gl-js/docs/examples/add-3d-tiles-using-threejs/)** `NEW` | Use a custom style layer with three.js to add 3D tiles to the map. | — |
| **[Add a 3D model to globe using three.js](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-to-globe-using-threejs/)** | Use a custom style layer with three.js to add a 3D model to a globe. | [`add-3d-model-globe.md`](../examples/add-3d-model-globe.md) |
| **[Add a 3D model with babylon.js](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-with-babylonjs/)** | Use a custom style layer with babylon.js to add a 3D model to the map. | — |
| **[Add a 3D model with shadow using three.js](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-3d-model-with-shadow-using-threejs/)** | Use a custom style layer with three.js to add a 3D model with shadow to the map. | — |
| **[Adding 3D models using three.js on terrain](https://maplibre.org/maplibre-gl-js/docs/examples/adding-3d-models-using-threejs-on-terrain/)** | Use a custom style layer with three.js to add 3D models to a map with 3d terrain. | [`threejs-3d-model-on-terrain.md`](../examples/threejs-3d-model-on-terrain.md) |
| **[Extrude polygons for 3D indoor mapping](https://maplibre.org/maplibre-gl-js/docs/examples/extrude-polygons-for-3d-indoor-mapping/)** | Create a 3D indoor map with the fill-extrude-height paint property. | — |
| **[Fill extrusion rounded corners](https://maplibre.org/maplibre-gl-js/docs/examples/fill-extrusion-rounded-corners/)** `NEW` | Use fill-extrusion-rounded-corner-distance to round the corners of extruded polygons. | — |

### 🏷️ Globe (7 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Display a globe with a vector map](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-globe-with-a-vector-map/)** | Display a globe with a vector map. | [`globe-projection.md`](../examples/globe-projection.md) |
| **[Display a globe with an atmosphere](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-globe-with-an-atmosphere/)** | Display a globe with an atmosphere. | — |
| **[Add a custom layer with tiles to a globe](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-custom-layer-with-tiles-to-a-globe/)** | Use custom layer to display arbitrary tiles drawn with a custom WebGL shader on a globe. | — |
| **[Add a simple custom layer on a globe](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-simple-custom-layer-on-a-globe/)** | Use a custom layer to draw simple WebGL content on a globe. | — |
| **[Create a Heatmap layer on a globe with terrain elevation](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-heatmap-layer-on-a-globe-with-terrain-elevation/)** | Create a Heatmap layer on a globe with terrain elevation. | — |
| **[Display a globe with a fill extrusion layer](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-globe-with-a-fill-extrusion-layer/)** | Display a globe with a fill extrusion layer. | — |
| **[Zoom and planet size relation on globe](https://maplibre.org/maplibre-gl-js/docs/examples/zoom-and-planet-size-relation-on-globe/)** | Explanation of zoom and planet size relation under globe projection and how to account for it when changing the map center and zoom by some delta. | — |

---

## 📁 Annotations & Labels

### 🏷️ Markers (5 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Add a default marker](https://maplibre.org/maplibre-gl-js/docs/examples/add-a-default-marker/)** | Add a default marker to the map. | — |
| **[Add custom icons with Markers](https://maplibre.org/maplibre-gl-js/docs/examples/add-custom-icons-with-markers/)** | Add custom marker icons to your map. | — |
| **[Create a draggable Marker](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-draggable-marker/)** | Drag a marker with the pointer or the keyboard, and make a custom marker element keyboard accessible. | — |
| **[Animate a marker](https://maplibre.org/maplibre-gl-js/docs/examples/animate-a-marker/)** | Animate the position of a marker by updating its location on each frame. | — |
| **[Create a draggable point](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-draggable-point/)** | Drag the point to a new location on a map and populate its coordinates in a display. | — |

### 🏷️ Popups (4 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Display a popup](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-popup/)** | Add a popup to the map. | — |
| **[Display a popup on click](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-popup-on-click/)** | When a user clicks a symbol, show a popup containing more information. | — |
| **[Display a popup on hover](https://maplibre.org/maplibre-gl-js/docs/examples/display-a-popup-on-hover/)** | When a user hovers over a custom marker, show a popup containing more information. | — |
| **[Attach a popup to a marker instance](https://maplibre.org/maplibre-gl-js/docs/examples/attach-a-popup-to-a-marker-instance/)** | Attach a popup to a marker and display it on click. | — |

### 🏷️ Labels & Fonts (7 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Display and style rich text labels](https://maplibre.org/maplibre-gl-js/docs/examples/display-and-style-rich-text-labels/)** | Use the format expression to display country labels in both English and in the local language. | — |
| **[Style labels with Web fonts](https://maplibre.org/maplibre-gl-js/docs/examples/style-labels-with-web-fonts/)** | Apply Web fonts to your style’s text labels. Unlike signed distance field (SDF) glyph sets, Web fonts are available from a variety of providers, or your can make your own using popular tools. This option is suitable for fonts that are only available through a third-party content delivery network (CDN) for technical or legal reasons, as well as fonts that are incompatible with SDF, such as variable fonts. For compatibility with Android and iOS applications, specify equivalent fonts in the style’s font-faces property. | — |
| **[Style labels with font faces](https://maplibre.org/maplibre-gl-js/docs/examples/style-labels-with-font-faces/)** `NEW` | Point your style at the font files used to draw its text labels. The font-faces property names a font file per text-font name, optionally narrowed to a unicode-range, so a style can cover scripts its glyphs server does not. Anything the browser can render text with may be used, and it is understood by Android and iOS as well. Here Georgian and Armenian labels come from font files while the Latin ones still come from the glyphs URL. | — |
| **[Change the case of labels](https://maplibre.org/maplibre-gl-js/docs/examples/change-the-case-of-labels/)** | Use the upcase and downcase expressions to change the case of labels. | — |
| **[Style labels with local fonts](https://maplibre.org/maplibre-gl-js/docs/examples/style-labels-with-local-fonts/)** | Apply local fonts to your style’s text labels. This option is suitable if you don’t need every user to see exactly the same font, or if you want to avoid relying on a third-party content delivery network (CDN). For maximum compatibility, the text-font property should include fonts commonly found on multiple platforms. | — |
| **[Variable label placement](https://maplibre.org/maplibre-gl-js/docs/examples/variable-label-placement/)** | Use text-variable-anchor to allow high priority labels to shift position to stay on the map. | — |
| **[Variable label placement with offset](https://maplibre.org/maplibre-gl-js/docs/examples/variable-label-placement-with-offset/)** | Use text-variable-anchor-offset to allow high priority labels to shift position to stay on the map. | — |

### 🏷️ Internationalization (4 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Change a map's language](https://maplibre.org/maplibre-gl-js/docs/examples/change-a-maps-language/)** | Use setLayoutProperty to switch languages dynamically. | — |
| **[Add support for right-to-left scripts](https://maplibre.org/maplibre-gl-js/docs/examples/add-support-for-right-to-left-scripts/)** | Use the mapbox-gl-rtl-text plugin to support right-to-left languages such as Arabic and Hebrew. | — |
| **[Locale switching](https://maplibre.org/maplibre-gl-js/docs/examples/locale-switching/)** | Show how localization can be applied manually to UI elements. Hover over a control to see the translated tooltip. | — |
| **[Use locally generated ideographs](https://maplibre.org/maplibre-gl-js/docs/examples/use-locally-generated-ideographs/)** | Set localIdeographFontFamily to override the font used for displaying CJK (Chinese, Japanese and Korean) characters, ignoring the map style. This setting must be a CSS font rule specifying fallbacks of on-device fonts. Set localIdeographFontFamily to false to use server-provided fonts, which is much slower. | — |

---

## 📁 Interactivity & Extensions

### 🏷️ Events & Queries (8 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Get features under the mouse pointer](https://maplibre.org/maplibre-gl-js/docs/examples/get-features-under-the-mouse-pointer/)** | Use queryRenderedFeatures to show properties of hovered-over map elements. | [`spatial-query-inspector.md`](../examples/spatial-query-inspector.md) |
| **[Create a hover effect](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-hover-effect/)** | Use events and feature states to create a per feature hover effect. | [`hover-feature-state.md`](../examples/hover-feature-state.md) |
| **[Animate symbol to follow the mouse](https://maplibre.org/maplibre-gl-js/docs/examples/animate-symbol-to-follow-the-mouse/)** | Animate symbol to follow the mouse. | — |
| **[Center the map on a clicked symbol](https://maplibre.org/maplibre-gl-js/docs/examples/center-the-map-on-a-clicked-symbol/)** | Use events and flyTo to center the map on a symbol. | — |
| **[Display Performance Metrics](https://maplibre.org/maplibre-gl-js/docs/examples/display-performance-metrics/)** `NEW` | Measure map performance using built-in events. | — |
| **[Get coordinates of the mouse pointer](https://maplibre.org/maplibre-gl-js/docs/examples/get-coordinates-of-the-mouse-pointer/)** | Show mouse position on hover with pixel and latitude and longitude coordinates. | — |
| **[Select features with boxZoomEnd callback](https://maplibre.org/maplibre-gl-js/docs/examples/select-features-with-boxzoomend-callback/)** | Use the boxZoomEnd callback to select features with Shift-drag instead of fitting the map to the dragged box. | — |
| **[Show polygon information on click](https://maplibre.org/maplibre-gl-js/docs/examples/show-polygon-information-on-click/)** | When a user clicks a polygon, show a popup containing more information. | — |

### 🏷️ Plugins & Integrations (16 Examples)

| Example | Official Doc Description | Standalone Recipe |
| :--- | :--- | :--- |
| **[Get features under the mouse pointer](https://maplibre.org/maplibre-gl-js/docs/examples/get-features-under-the-mouse-pointer/)** | Use queryRenderedFeatures to show properties of hovered-over map elements. | [`spatial-query-inspector.md`](../examples/spatial-query-inspector.md) |
| **[Create a hover effect](https://maplibre.org/maplibre-gl-js/docs/examples/create-a-hover-effect/)** | Use events and feature states to create a per feature hover effect. | [`hover-feature-state.md`](../examples/hover-feature-state.md) |
| **[Animate symbol to follow the mouse](https://maplibre.org/maplibre-gl-js/docs/examples/animate-symbol-to-follow-the-mouse/)** | Animate symbol to follow the mouse. | — |
| **[Center the map on a clicked symbol](https://maplibre.org/maplibre-gl-js/docs/examples/center-the-map-on-a-clicked-symbol/)** | Use events and flyTo to center the map on a symbol. | — |
| **[Display Performance Metrics](https://maplibre.org/maplibre-gl-js/docs/examples/display-performance-metrics/)** `NEW` | Measure map performance using built-in events. | — |
| **[Get coordinates of the mouse pointer](https://maplibre.org/maplibre-gl-js/docs/examples/get-coordinates-of-the-mouse-pointer/)** | Show mouse position on hover with pixel and latitude and longitude coordinates. | — |
| **[Select features with boxZoomEnd callback](https://maplibre.org/maplibre-gl-js/docs/examples/select-features-with-boxzoomend-callback/)** | Use the boxZoomEnd callback to select features with Shift-drag instead of fitting the map to the dragged box. | — |
| **[Show polygon information on click](https://maplibre.org/maplibre-gl-js/docs/examples/show-polygon-information-on-click/)** | When a user clicks a polygon, show a popup containing more information. | — |
| **[Geocode with Nominatim](https://maplibre.org/maplibre-gl-js/docs/examples/geocode-with-nominatim/)** | Geocode with Nominatim and the maplibre-gl-geocoder plugin. | — |
| **[PMTiles source and protocol](https://maplibre.org/maplibre-gl-js/docs/examples/pmtiles-source-and-protocol/)** | Uses the PMTiles plugin and protocol to present a map. | — |
| **[Create deck.gl layer using REST API](https://maplibre.org/maplibre-gl-js/docs/examples/create-deckgl-layer-using-rest-api/)** | Create a deck.gl layer as an overlay from a REST API. | — |
| **[Draw geometries with terra-draw](https://maplibre.org/maplibre-gl-js/docs/examples/draw-geometries-with-terra-draw/)** | Use maplibre-gl-terradraw to draw a geometry in various forms such as point, line or polygon on your map. | — |
| **[Draw polygon with mapbox-gl-draw](https://maplibre.org/maplibre-gl-js/docs/examples/draw-polygon-with-mapbox-gl-draw/)** | Use mapbox-gl-draw to draw a polygon and Turf.js to calculate its area in square meters. | — |
| **[Sync movement of multiple maps](https://maplibre.org/maplibre-gl-js/docs/examples/sync-movement-of-multiple-maps/)** | Synchronize MapLibre GL JS maps with the sync-move plugin. | — |
| **[Toggle deck.gl layer](https://maplibre.org/maplibre-gl-js/docs/examples/toggle-deckgl-layer/)** | Toggle deck.gl layer using maplibre. | — |
| **[Use addProtocol to Transform Feature Properties](https://maplibre.org/maplibre-gl-js/docs/examples/use-addprotocol-to-transform-feature-properties/)** | Reverse country names with addProtocol in plain JavaScript. | — |

---

*(Total indexed examples: 148)*