# MapTiler Planet v4 Vector Tile Schema Reference 🗺️📐

> Comprehensive technical reference for the **MapTiler Planet v4 vector tile schema** (OpenMapTiles specification). Describes all vector layers, geometries, field attributes, class filters, and zoom level distributions.

---

## 1. Schema Overview

MapTiler Planet v4 encodes global geospatial features into highly compressed Mapbox Vector Tile (`.pbf`) slices.
Tiles are hosted at:
```text
https://api.maptiler.com/tiles/v4/{z}/{x}/{y}.pbf?key=YOUR_MAPTILER_API_KEY
```

---

## 2. Core Layers Reference

### 1. `transportation`
Contains roads, railways, paths, ferries, and aerial cableways.
* **Geometry Types**: `LineString`, `MultiLineString`.
* **Zoom Range**: Zoom `0` to `14+`.

| Field Name | Type | Allowed Values / Description |
| :--- | :--- | :--- |
| **`class`** | `String` | `'motorway'`, `'trunk'`, `'primary'`, `'secondary'`, `'tertiary'`, `'minor'`, `'path'`, `'service'`, `'track'`, `'raceway'`, `'rail'`, `'transit'`, `'ferry'`, `'aerialway'` |
| **`subclass`** | `String` | Detailed road type (e.g. `'pedestrian'`, `'footway'`, `'cycleway'`, `'steps'`, `'bridleway'`) |
| **`oneway`** | `Number` | `1` = one-way driving direction, `-1` = reverse one-way, `0` = two-way |
| **`ramp`** | `Number` | `1` = highway exit ramp or link road |
| **`brunnel`** | `String` | `'bridge'`, `'tunnel'`, `'ford'` |
| **`surface`** | `String` | `'paved'`, `'unpaved'` |
| **`layer`** | `Number` | Physical vertical stacking index (`-5` to `5`) |
| **`level`** | `Number` | Indoor floor level for transit stations |

---

### 2. `building`
Contains building footprints and heights for 2D cartography and 3D extrusion.
* **Geometry Types**: `Polygon`, `MultiPolygon`.
* **Zoom Range**: Zoom `13` to `14+`.

| Field Name | Type | Description |
| :--- | :--- | :--- |
| **`render_height`** | `Number` | Estimated roof height in meters above ground. Defaults to `levels * 3.5m`. |
| **`render_min_height`**| `Number` | Estimated base height in meters for floating/cantilever structures. |
| **`levels`** | `Number` | Number of above-ground floors. |
| **`min_level`** | `Number` | Starting floor level for split building sections. |
| **`colour`** | `String` | Hex color code or CSS color string from OpenStreetMap tags. |
| **`hide_3d`** | `Boolean` | `true` if this building is covered by a detailed 3D model. |

---

### 3. `water` & `waterway`
Contains oceans, seas, lakes, reservoirs, rivers, and canals.
* **`water`**: `Polygon`, `MultiPolygon` (Zoom `0` to `14+`).
* **`waterway`**: `LineString`, `MultiLineString` (Zoom `3` to `14+`).

| Layer | Field Name | Type | Values / Description |
| :--- | :--- | :--- | :--- |
| `water` | **`class`** | `String` | `'ocean'`, `'sea'`, `'lake'`, `'river'`, `'reservoir'`, `'dock'`, `'swimming_pool'` |
| `water` | **`intermittent`**| `Number` | `1` = seasonal or ephemeral water body |
| `waterway` | **`class`** | `String` | `'river'`, `'stream'`, `'canal'`, `'drain'`, `'ditch'` |
| `waterway` | **`brunnel`** | `String` | `'bridge'`, `'tunnel'` (e.g. subterranean culvert) |

---

### 4. `landuse` & `landcover`
Defines human urban land utilization and natural surface coverage.
* **Geometry Types**: `Polygon`, `MultiPolygon`.

| Field Name | Type | Allowed Values |
| :--- | :--- | :--- |
| **`class`** | `String` | Urban: `'residential'`, `'commercial'`, `'industrial'`, `'retail'`, `'railway'`, `'cemetery'`, `'hospital'`, `'school'`, `'military'`, `'park'`.<br>Natural: `'forest'`, `'wood'`, `'grass'`, `'sand'`, `'rock'`, `'glacier'`, `'wetland'`, `'scrub'`. |

---

### 5. `place`
Contains populated settlements, administrative divisions, and geographic regions.
* **Geometry Types**: `Point`.
* **Zoom Range**: Zoom `0` to `14+`.

| Field Name | Type | Allowed Values / Description |
| :--- | :--- | :--- |
| **`class`** | `String` | `'country'`, `'state'`, `'province'`, `'city'`, `'town'`, `'village'`, `'hamlet'`, `'suburb'`, `'neighbourhood'`, `'isolated_dwelling'`, `'island'` |
| **`name`** | `String` | Local language place name |
| **`name:en`** | `String` | English place name |
| **`name:latin`**| `String` | Latinized transliteration |
| **`capital`** | `Number` | `2` = national capital, `4` = state/provincial capital |
| **`population`**| `Number` | Population count used for label font scaling |
| **`rank`** | `Number` | Label priority rank (`1` = highest priority) |

---

### 6. `poi` (Points of Interest)
Commercial businesses, civic amenities, transit stops, and tourism attractions.
* **Geometry Types**: `Point`.
* **Zoom Range**: Zoom `10` to `14+`.

| Field Name | Type | Allowed Values / Description |
| :--- | :--- | :--- |
| **`class`** | `String` | `'food'`, `'shop'`, `'lodging'`, `'health'`, `'education'`, `'tourism'`, `'park'`, `'transit'`, `'service'` |
| **`subclass`** | `String` | Specific category: `'restaurant'`, `'cafe'`, `'fast_food'`, `'bar'`, `'pub'`, `'supermarket'`, `'bakery'`, `'hotel'`, `'motel'`, `'hospital'`, `'pharmacy'`, `'bank'`, `'atm'`, `'school'`, `'university'`, `'museum'`, `'attraction'`, `'fuel'`, `'cinema'`, `'police'` |
| **`name`** | `String` | Local name |
| **`name:en`** | `String` | English name |

---

### 7. `boundary`
International and domestic political administrative boundaries.
* **Geometry Types**: `LineString`, `MultiLineString`.

| Field Name | Type | Description |
| :--- | :--- | :--- |
| **`admin_level`** | `Number` | `2` = Country border, `4` = State / Regional border, `6` = County border |
| **`disputed`** | `Number` | `1` = Disputed international boundary line |
| **`maritime`** | `Number` | `1` = Maritime territorial water boundary (usually hidden or dashed) |
