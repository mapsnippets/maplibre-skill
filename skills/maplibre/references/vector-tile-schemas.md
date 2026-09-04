# MapTiler Planet v4 Vector Tile Schema Reference 🗺️📐

> Exhaustive, high-density technical reference for the **MapTiler Planet v4 vector tile schema**. Covers all 73 distinct source-layers, zoom level distributions, geometry types, and complete attribute/enum dictionaries for professional cartography, custom layer styling, and spatial queries.

---

## 1. Schema Overview & Architecture

MapTiler Planet v4 is the next-generation global vector tileset. Unlike legacy v3 monolithic layers (`transportation`, `poi`), Planet v4 introduces specialized, high-performance layers (`road`, `railway`, `poi_food`, `poi_transport`, `country_border`) with higher geometry precision and sub-pixel generalization.

### Vector Tile Endpoint:
```text
https://api.maptiler.com/tiles/v4/{z}/{x}/{y}.pbf?key=YOUR_MAPTILER_API_KEY
```
* **Tile Format**: Mapbox Vector Tile (`.pbf` MVT protobuf v2.1)
* **Tile Extent**: 4096 × 4096 coordinates per tile
* **Projection**: Spherical Mercator (`EPSG:3857`)
* **Tile Matrix**: Zoom 0 to 15 (with overzooming client interpolation up to zoom 22)

---

## 2. Roads & Street Networks

### `source-layer: 'road'`
> **Layer containting roads.**  
* **Geometry**: `LineString` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the road is publicly accessible. |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the road is part of a bridge, tunnel, or ford. |
| **`class`** | `bus_guideway`, `busway`, `minor`, `motorway`, `primary`, `raceway`, `secondary`, `service`, `tertiary`, `trunk` | Classification category. |
| **`construction`** | `True` | Indicates whether the road is under construction. |
| **`expressway`** | `True` | Indicates whether the road is an expressway (lower-standard motorway). |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`layer`** | free-form / string / number | Defines the relative order of roads in relation to other features (e.g., different road) Min value: - `-10` Max value: - `10` |
| **`network`** | `ca-provincial`, `ca-provincial-arterial`, `ca-transcanada`, `gb-motorway`, `gb-primary`, `gb-trunk`, `ie-motorway`, `ie-national`, `ie-regional`, `us-highway` *(+2 more)* | Describes localized road system categories. |
| **`oneway`** | `True` | Indicates whether the road is a one-way. |
| **`paved`** | `False`, `True` | Indicates whether the road is paved. |
| **`ramp`** | `True` | Indicates whether the road is a ramp or a link to/from the road. |
| **`service`** | `alley`, `drive-through`, `driveway`, `emergency_access`, `parking_aisle`, `slipway` | Describes the service of the road. |
| **`toll`** | `True` | Indicates whether the toll is payable on the road. |

---

### `source-layer: 'road_label'`
> **Layer containing line labels for named roads.**  
* **Geometry**: `Point` | **Zoom Range**: `z6` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the road is publicly accessible. |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the road is part of a bridge, tunnel, or ford. |
| **`class`** | `bus_guideway`, `busway`, `minor`, `motorway`, `primary`, `raceway`, `secondary`, `service`, `tertiary`, `trunk` | Classification category. |
| **`construction`** | `True` | Indicates whether the road is under construction. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`name`** | free-form / string / number | Primary (local or official) name of the road. |
| **`name:{code}`** | free-form / string / number | Localized name of the road in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`network`** | `ca-provincial`, `ca-provincial-arterial`, `ca-transcanada`, `gb-motorway`, `gb-primary`, `gb-trunk`, `ie-motorway`, `ie-national`, `ie-regional`, `road` *(+3 more)* | Describes localized road system categories. |
| **`ref`** | free-form / string / number | Reference code of the road. |
| **`ref_length`** | free-form / string / number | Number of characters of the reference code. Min value: - `0` Max value: - `8` |
| **`route_1_colour`** | free-form / string / number | Colour of the 1st concurrency road. |
| **`route_1_name`** | free-form / string / number | Name of the 1st concurrency road. |
| **`route_1_network`** | free-form / string / number | Describes localized 1st concurrency road system categories. |
| **`route_1_ref`** | free-form / string / number | Reference code of the 1st concurrency road. |
| **`route_2_colour`** | free-form / string / number | Colour of the 2nd concurrency road. |
| **`route_2_name`** | free-form / string / number | Name of the 2nd concurrency road. |
| **`route_2_network`** | free-form / string / number | Describes localized 2nd concurrency road system categories. |
| **`route_2_ref`** | free-form / string / number | Reference code of the 2nd concurrency road. |
| **`route_3_colour`** | free-form / string / number | Colour of the 3rd concurrency road. |
| **`route_3_name`** | free-form / string / number | Name of the 3rd concurrency road. |
| **`route_3_network`** | free-form / string / number | Describes localized 3rd concurrency road system categories. |
| **`route_3_ref`** | free-form / string / number | Reference code of the 3rd concurrency road. |
| **`route_4_colour`** | free-form / string / number | Colour of the 4th concurrency road. |
| **`route_4_name`** | free-form / string / number | Name of the 4th concurrency road. |
| **`route_4_network`** | free-form / string / number | Describes localized 4th concurrency road system categories. |
| **`route_4_ref`** | free-form / string / number | Reference code of the 4th concurrency road. |
| **`route_5_name`** | free-form / string / number | Name of the 5th concurrency road. |
| **`route_5_network`** | free-form / string / number | Describes localized 5th concurrency road system categories. |
| **`route_5_ref`** | free-form / string / number | Reference code of the 5th concurrency road. |
| **`route_6_name`** | free-form / string / number | Name of the 6th concurrency road. |
| **`route_6_network`** | free-form / string / number | Describes localized 6th concurrency road system categories. |
| **`route_6_ref`** | free-form / string / number | Reference code of the 6th concurrency road. |

---

### `source-layer: 'road_exit'`
> **Layer containing point labels of road exits.**  
* **Geometry**: `Point` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `minor`, `motorway`, `primary`, `secondary`, `tertiary`, `trunk` | Classification category. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`layer`** | free-form / string / number | Defines the relative order of road exits in relation to other features (e.g., road) Min value: - `-1` Max value: - `5` |
| **`name`** | free-form / string / number | Primary (local or official) name of the road exit. |
| **`name:{code}`** | free-form / string / number | Localized name of the road exit in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`ref`** | free-form / string / number | Reference code of the road exit. |
| **`ref_length`** | free-form / string / number | Number of characters of the reference code Min value: - `1` Max value: - `8` |

---

### `source-layer: 'pathway'`
> **Layer containing pathways.**  
* **Geometry**: `LineString` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the pathway is publicly accessible. |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the pathway is part of a bridge, tunnel, or ford. |
| **`class`** | `bridleway`, `corridor`, `cycleway`, `footway`, `path`, `pedestrian`, `platform`, `steps`, `track` | Classification category. |
| **`construction`** | `True` | Indicates whether the pathway is under construction. |
| **`indoor`** | `True` | Indicates whether the pathway is located inside a building. |
| **`layer`** | free-form / string / number | Defines the relative order of pathway in relation to other features (e.g., roads) Min value: - `-10` Max value: - `10` |
| **`level`** | free-form / string / number | Defines the absolute vertical level (floor level) of the pathway. Min value: - `-19` Max value: - `56` |
| **`oneway`** | `True` | Indicates whether the pathway is a one-way. |
| **`paved`** | `False`, `True` | Indicates whether the pathway is paved. |

---

### `source-layer: 'pathway_label'`
> **Layer containing line labels for named pathways.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the pathway is publicly accessible. |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the pathway is part of a bridge, tunnel, or ford. |
| **`class`** | `bridleway`, `corridor`, `cycleway`, `footway`, `path`, `pedestrian`, `platform`, `steps`, `track` | Classification category. |
| **`construction`** | `True` | Indicates whether the pathway is under construction. |
| **`name`** | free-form / string / number | Primary (local or official) name of the pathway. |
| **`name:{code}`** | free-form / string / number | Localized name of the pathway in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`unpaved`** | `True` | Indicates whether the pathway is unpaved. |

---

### `source-layer: 'pedestrian'`
> **Layer containing polygons of pedestrian areas.**  
* **Geometry**: `LineString` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the pedestrian area is publicly accessible. |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the pedestrian area is part of abridge, tunnel, or ford. |
| **`class`** | `bridleway`, `corridor`, `cycleway`, `footway`, `path`, `pedestrian`, `platform`, `steps`, `track` | Classification category. |
| **`construction`** | `True` | Indicates whether the pedestrian area is under construction. |
| **`indoor`** | `True` | Indicates whether the pedestrian area is located inside a building. |
| **`layer`** | free-form / string / number | Defines the relative order of pedestrian areas in relation to other features (e.g., roads) Min value: - `1` Max value: - `9` |
| **`level`** | free-form / string / number | Defines the absolute vertical level (floor level) of the pathway. Min value: - `-6` Max value: - `5` |
| **`paved`** | `False`, `True` | Indicates whether the pedestrian area is paved. |

---

### `source-layer: 'pedestrian_label'`
> **Layer containing point labels for named pedestrian areas.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the pedestrian area is publicly accessible. |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the pedestrian area is part of abridge, tunnel, or ford. |
| **`class`** | `bridleway`, `corridor`, `cycleway`, `footway`, `path`, `pedestrian`, `platform`, `steps`, `track` | Classification category. |
| **`construction`** | `True` | Indicates whether the pedestrian area is under construction. |
| **`name`** | free-form / string / number | Primary (local or official) name of the pedestrian area. |
| **`name:{code}`** | free-form / string / number | Localized name of the pedestrian area in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`unpaved`** | `True` | Indicates whether the pedestrian area is unpaved. |

---

### `source-layer: 'parking'`
> **Layer containing polygons of parking areas and parking spaces.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the parking is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification category for parking with its subclass values. |
| **`parking`** | `carports`, `garage`, `garage_boxes`, `half_on_kerb`, `lane`, `layby`, `multi-storey`, `on_kerb`, `other`, `parking` *(+5 more)* | Classification categories for the parking areas types. |
| **`parking_space`** | `ambulance`, `bicycle`, `boat_trailer`, `bus`, `car_sharing`, `caravan`, `carpool`, `charging`, `coach`, `compact` *(+21 more)* | Classification categories for the parking spaces. |

---

### `source-layer: 'traffic_control'`
> **Layer containing lines of traffic control features like barriers, kerbs, bumps.**  
* **Geometry**: `Point` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `roads`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the traffic control is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for the traffic control features with their subclass values. |
| **`lowered`** | `True` | Indicates whether the `class=protection, subclass=kerb` is lowered. |
| **`ref`** | free-form / string / number | Reference code of the traffic control. |

---

### `source-layer: 'pier'`
> **Layer containing pier polygons.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z9` to `z15–15+ (z22)` | **Block**: `roads`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

## 3. Railways & Transit Infrastructure

### `source-layer: 'railway'`
> **Layer containing railways.**  
* **Geometry**: `LineString` | **Zoom Range**: `z6` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`brunnel`** | `bridge`, `tunnel` | Describes whether the railway is part of a bridge or tunnel. |
| **`class`** | `abandoned`, `disused`, `funicular`, `light_rail`, `miniature`, `monorail`, `narrow_gauge`, `preserved`, `rail`, `tram`, `turntable` | Classification category. |
| **`construction`** | `True` | Indicates whether the railway is under construction. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`ref`** | free-form / string / number | Reference code of the railway route. |
| **`service`** | `crossover`, `other`, `siding`, `slipway`, `spur`, `yard` | Describes the service of the railway. |
| **`usage`** | `branch`, `crane`, `freight`, `industrial`, `leisure`, `main`, `military`, `other`, `science`, `test`, `tourism` | Describes the usage of the railway. |

---

### `source-layer: 'railway_label'`
> **Layer containing line labels for named railways.**  
* **Geometry**: `Point` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`brunnel`** | `bridge`, `tunnel` | Describes whether the railway is part of a bridge or tunnel. |
| **`class`** | `abandoned`, `disused`, `funicular`, `light_rail`, `miniature`, `monorail`, `narrow_gauge`, `preserved`, `rail`, `tram`, `turntable` | Classification category. |
| **`construction`** | `True` | Indicates whether the railway is under construction. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`name`** | free-form / string / number | Primary (local or official) name of the railway. |
| **`name:{code}`** | free-form / string / number | Localized name of the railway in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`ref`** | free-form / string / number | Reference code of the railway route. |
| **`service`** | `crossover`, `other`, `siding`, `slipway`, `spur`, `yard` | Describes the service of the railway. |

---

### `source-layer: 'aerialway'`
> **Layer containing aerialway features.**  
* **Geometry**: `LineString` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the aerialway is publicly accessible. |
| **`class`** | `cable_car`, `chair_lift`, `drag_lift`, `gondola`, `goods`, `j-bar`, `magic_carpet`, `mixed_lift`, `platter`, `rope_tow`, `t-bar`, `zip_line` | Classification category. |
| **`ref`** | free-form / string / number | Reference code of the aerialway. |

---

### `source-layer: 'aerialway_label'`
> **Layer containing line labels for named aerialway features.**  
* **Geometry**: `Point` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the aerialway is publicly accessible. |
| **`class`** | `cable_car`, `chair_lift`, `drag_lift`, `gondola`, `goods`, `j-bar`, `magic_carpet`, `mixed_lift`, `platter`, `rope_tow`, `t-bar`, `zip_line` | Classification category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the aerialway. |
| **`name:{code}`** | free-form / string / number | Localized name of the aerialway in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`ref`** | free-form / string / number | Reference code of the aerialway. |

---

### `source-layer: 'ferry'`
> **Layer containing lines of ferry routes.**  
* **Geometry**: `LineString` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the ferry is publicly accessible. |
| **`ref`** | free-form / string / number | Reference code of the ferry route. |

---

### `source-layer: 'ferry_label'`
> **Layer containing line labels for named ferry routes.**  
* **Geometry**: `Point` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the ferry is publicly accessible. |
| **`name`** | free-form / string / number | Primary (local or official) name of the ferry route. |
| **`name:{code}`** | free-form / string / number | Localized name of the ferry route in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`ref`** | free-form / string / number | Reference code of the ferry route. |

---

### `source-layer: 'aviation'`
> **Layer containing polygons of aviation features.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the aviation facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for aviation facility with their subclass values. |
| **`construction`** | `True` | Indicates whether the aviation facility is under construction. |
| **`faa`** | free-form / string / number | The Federal Aviation Administration location identifier. |
| **`iata`** | free-form / string / number | 3-character code issued by the International Air Transport Association. |
| **`icao`** | free-form / string / number | 4-letter code issued by the International Civil Aviation Organization. |
| **`ref`** | free-form / string / number | Reference code of the aviation facility. |

---

### `source-layer: 'aviation_line'`
> **Layer containing lines of aviation features.**  
* **Geometry**: `LineString` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the aviation facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for aviation facility with their subclass values. |
| **`construction`** | `True` | Indicates whether the aviation facility is under construction. |
| **`faa`** | free-form / string / number | The Federal Aviation Administration location identifier. |
| **`iata`** | free-form / string / number | 3-character code issued by the International Air Transport Association. |
| **`icao`** | free-form / string / number | 4-letter code issued by the International Civil Aviation Organization. |
| **`ref`** | free-form / string / number | Reference code of the aviation facility. |

---

### `source-layer: 'bridge'`
> **Layer containing polygons of bridges.**  
* **Geometry**: `LineString` | **Zoom Range**: `z8` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `wildlife_crossing` | Classification category. |
| **`construction`** | `True` | Indicates whether the bridge is under construction. |
| **`layer`** | free-form / string / number | Defines the relative order of bridges in relation to other features (e.g., roads) Min value: - `-2` Max value: - `10` |

---

### `source-layer: 'bridge_label'`
> **Layer containing label points of bridges.**  
* **Geometry**: `Point` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `transit`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `wildlife_crossing` | Classification category. |
| **`construction`** | `True` | Indicates whether the bridge is under construction. |
| **`layer`** | free-form / string / number | Defines the relative order of bridges in relation to other features (e.g., roads) Min value: - `-2` Max value: - `10` |
| **`name`** | free-form / string / number | Primary (local or official) name of the bridge. |
| **`name:{code}`** | free-form / string / number | Localized name of the bridge in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

## 4. Buildings & Addresses

### `source-layer: 'building'`
> **Layer containing polygons of building or building part footprints.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`height`** | free-form / string / number | Approximate height (in meters), derived from building levels or height attributes (or building part). Min value: - `1` Max value: - `850` |
| **`height_min`** | free-form / string / number | Approximate base height (in meters), derived from minimum levels or minimum height attributes of the building or its parts. Min value: - `0` Max value: - `583` |
| **`underground`** | `True` | Indicates whether the building is underground. |

---

### `source-layer: 'building_number'`
> **Layer containing points representing building address labels.**  
* **Geometry**: `Point` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`number`** | free-form / string / number | Address number. |

---

## 5. Water & Hydrography

### `source-layer: 'water'`
> **Layer containing polygons of water bodies including seas, lakes, reservoirs or rivers.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `water`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class & subclass`** | free-form / string / number | Classification category. |
| **`covered`** | `True` | Indicates whether the water body is covered. (Most often covered reservoirs.) |
| **`intermittent`** | `True` | Indicates whether the water body is intermittent. |

---

### `source-layer: 'waterway'`
> **Layer containing lines of linear water features such as rivers, streams and canals.**  
* **Geometry**: `LineString` | **Zoom Range**: `z9` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`brunnel`** | `bridge`, `ford`, `tunnel` | Describes whether the waterway segment is part of a bridge, tunnel, or ford. |
| **`class`** | `canal`, `ditch`, `drain`, `river`, `stream` | Classification category. |
| **`intermittent`** | `True` | Indicates whether the waterway is intermittent. |

---

### `source-layer: 'water_centroid'`
> **Layer containing point labels for named water bodies.**  
* **Geometry**: `Point` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `water`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`area`** | free-form / string / number | Area of the water body in square kilometers, measured in the Web Mercator (EPSG:3857) projection. Min value: - `0` Max value: - `4254074` |
| **`class`** | `bay`, `canal`, `ditch`, `drain`, `lagoon`, `lake`, `ocean`, `pond`, `reef`, `reservoir`, `river`, `sea`, `strait`, `stream` | Classification category. |
| **`ipq`** | free-form / string / number | Isoperimetric quotient in percent, ratio of polygon area to the area of a circle with the same perimeter. A perfect circle has an IPQ of 100%. Min value: - `0` Max value: - `100` |
| **`length`** | free-form / string / number | Length of the corresponding water label line from `water_label` in kilometers, measured in the Web Mercator (EPSG:3857) projection. Min value: - `0` Max value: - `12942` |
| **`name`** | free-form / string / number | Primary (local or official) name of the water body. |
| **`name:{code}`** | free-form / string / number | Localized name of the water body in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | `1`, `2`, `3`, `4`, `10` | Importance of the water label. Lower values indicate greater importance. |
| **`rotation`** | free-form / string / number | Clockwise rotation in degrees representing the tangent angle at the midpoint of the label line from `water_label`, suitable for use with the `text-rotate` style property. Min value: - `-70` Max value: - `70` |
| **`worldview`** | `recognized`, `unrecognized` | Defines the geopolitical perspective from which a feature is represented. | worldview |              description               | |-----------|----------------------------------------| | `ch`      | geopolitical world view of Switzerland | | `us`      | geopolitical world view of USA         | |

---

### `source-layer: 'water_label'`
> **Layer containing line labels for named water bodies.**  
* **Geometry**: `Point` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`area`** | free-form / string / number | Area of the water body in square kilometers, measured in the Web Mercator (EPSG:3857) projection. Min value: - `0` Max value: - `4254074` |
| **`class`** | `bay`, `canal`, `ditch`, `drain`, `lagoon`, `lake`, `ocean`, `pond`, `reef`, `reservoir`, `river`, `sea`, `strait`, `stream` | Classification category. |
| **`ipq`** | free-form / string / number | Isoperimetric quotient in percent, ratio of polygon area to the area of a circle with the same perimeter. A perfect circle has an IPQ of 100%. Min value: - `0` Max value: - `100` |
| **`length`** | free-form / string / number | Length of the water label line in kilometers, measured in the Web Mercator (EPSG:3857) projection. Min value: - `0` Max value: - `12942` |
| **`name`** | free-form / string / number | Primary (local or official) name of the water body. |
| **`name:{code}`** | free-form / string / number | Localized name of the water body in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | `1`, `2`, `3`, `4`, `10` | Importance of the water label. Lower values indicate greater importance. |
| **`rotation`** | free-form / string / number | Clockwise rotation in degrees representing the tangent angle at the midpoint of the label line, suitable for use with the `text-rotate` style property. Min value: - `-70` Max value: - `70` |
| **`worldview`** | `recognized`, `unrecognized` | Defines the geopolitical perspective from which a feature is represented. | worldview | description                            | |-----------|----------------------------------------| | `ch`      | geopolitical world view of Switzerland | | `us`      | geopolitical world view of USA         | |

---

### `source-layer: 'dam'`
> **Layer containing polygons of dam structures built across a river or stream to impound the water.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `builtup`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'wetland'`
> **Layer containing polygons of wetlands, marshes and tidal flats.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `nature`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

## 6. Administrative Boundaries & Place Labels

### `source-layer: 'country_border'`
> **Layer containing lines of country borders.**  
* **Geometry**: `LineString` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`admin_l`** | free-form / string / number | ISO 3166-1 Alpha-2 code of the country to the left of the border. |
| **`admin_r`** | free-form / string / number | ISO 3166-1 Alpha-2 code of the country to the right of the border. |
| **`maritime`** | `False`, `True` | Indicates whether the border is maritime. |

---

### `source-layer: 'country_border_disputed'`
> **Layer containing lines of disputed country borders.**  
* **Geometry**: `LineString` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`claimed_by`** | free-form / string / number | ISO 3166-1 Alpha-2 code of the country that claims the border. |
| **`disputed_by`** | free-form / string / number | ISO 3166-1 Alpha-2 code of the country that disputes the border. |
| **`disputed_name`** | free-form / string / number | Primary (local) name of the disputed border or area. |
| **`maritime`** | `False`, `True` | Indicates whether the border is maritime. |
| **`worldview`** | `disputed`, `recognized`, `unrecognized` | Defines the geopolitical perspective from which a feature is represented. | worldview | description                            | |-----------|----------------------------------------| | `ch`      | geopolitical world view of Switzerland | | `us`      | geopolitical world view of USA         | |

---

### `source-layer: 'sub_border'`
> **Layer containing lines of country subdivision borders.**  
* **Geometry**: `LineString` | **Zoom Range**: `z2` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`admin_level`** | `30`, `40`, `50`, `60`, `61`, `70`, `80`, `90`, `100`, `110` | Numerical representation of an administrative unit for country subdivisions. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`maritime`** | `False`, `True` | Indicates whether the border is maritime. |

---

### `source-layer: 'country_label'`
> **Layer containing point labels of each country.**  
* **Geometry**: `Point` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`name`** | free-form / string / number | Primary (local or official) name of the country. |
| **`name:{code}`** | free-form / string / number | Localized name of the country in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | free-form / string / number | Importance of the country. Lower values indicate greater importance. Min value: - `0` Max value: - `5` |

---

### `source-layer: 'country_disputed_label'`
> **Layer containing point labels of disputed country.**  
* **Geometry**: `Point` | **Zoom Range**: `z3` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`name`** | free-form / string / number | Primary (local) name of the disputed country. |
| **`name:{code}`** | free-form / string / number | Primary (local) name of the disputed country in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`worldview`** | `disputed`, `recognized`, `unrecognized` | Defines the geopolitical perspective from which a feature is represented. | worldview | description                            | |-----------|----------------------------------------| | `ch`      | geopolitical world view of Switzerland | | `us`      | geopolitical world view of USA         | |

---

### `source-layer: 'continent_label'`
> **Layer containing label points of each continent.**  
* **Geometry**: `Point` | **Zoom Range**: `z0` to `z3` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`name`** | free-form / string / number | Primary (local or official) name of the continent. |
| **`name:{code}`** | free-form / string / number | Localized name of the continent in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'city_label'`
> **Layer containing point labels for cities.**  
* **Geometry**: `Point` | **Zoom Range**: `z3` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`capital`** | `20`, `30`, `40`, `50`, `60`, `61`, `70`, `80`, `90`, `100` | Numerical representation of an administrative unit from country to neighbourhood. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`name`** | free-form / string / number | Primary (local or official) name of the city. |
| **`name:{code}`** | free-form / string / number | Localized name of the city in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | free-form / string / number | Importance of the city. Lower values indicate greater importance. Min value: - `1` Max value: - `7` |

---

### `source-layer: 'town_label'`
> **Layer containing point labels for towns.**  
* **Geometry**: `Point` | **Zoom Range**: `z6` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`capital`** | `20`, `30`, `40`, `50`, `60`, `61`, `70`, `80`, `90`, `100` | Numerical representation of an administrative unit from country to neighbourhood. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`name`** | free-form / string / number | Primary (local or official) name of the town. |
| **`name:{code}`** | free-form / string / number | Localized name of the town in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | free-form / string / number | Importance of the town. Lower values indicate greater importance. Min value: - `1` Max value: - `11` |

---

### `source-layer: 'place_label'`
> **Layer containing point labels of places (excluding `town` and `city`).**  
* **Geometry**: `Point` | **Zoom Range**: `z9` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `hamlet`, `isolated_dwelling`, `neighbourhood`, `quarter`, `suburb`, `village` | Classification category. |
| **`iso_a2`** | free-form / string / number | Country code in ISO 3166-1 Alpha-2 format. |
| **`name`** | free-form / string / number | Primary (local or official) name of the place. |
| **`name:{code}`** | free-form / string / number | Localized name of the place in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | free-form / string / number | Importance of the place. Lower values indicate greater importance. Min value: - `1` Max value: - `40` |

---

### `source-layer: 'island_label'`
> **Layer containing point labels for named islands, islets or atolls.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `atoll`, `island`, `islet` | Classification category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the island, islet or atoll. |
| **`name:{code}`** | free-form / string / number | Localized name of the island, islet or atoll in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | free-form / string / number | Importance of the island. Lower values indicate greater importance. Min value: - `5` Max value: - `14` |

---

### `source-layer: 'archipelago_label'`
> **Layer containing line labels for named archipelagos.**  
* **Geometry**: `Point` | **Zoom Range**: `z3` to `z15–15+ (z22)` | **Block**: `administrative`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`name`** | free-form / string / number | Primary (local or official) name of the archipelago. |
| **`name:{code}`** | free-form / string / number | Localized name of the archipelago in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`rank`** | free-form / string / number | Importance of the archipelago. Lower values indicate greater importance. Min value: - `1` Max value: - `7` |
| **`worldview`** | `recognized`, `unrecognized` | Defines the geopolitical perspective from which a feature is represented. | worldview | description                            | |-----------|----------------------------------------| | `ch`      | geopolitical world view of Switzerland | | `us`      | geopolitical world view of USA         | |

---

## 7. Urban Landuse & Zoning

### `source-layer: 'residential'`
> **Layer containing polygons of residential areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `builtup`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'commercial'`
> **Layer containing polygons of commercial areas, used for services and trade (tertiary sector).**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `commercial`, `marketplace`, `retail` | Classification category. |

---

### `source-layer: 'industrial'`
> **Layer containing polygons of industrial and utility areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z8` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `brownfield`, `industrial`, `landfill`, `plant`, `quarry`, `wastewater_plant` | Classification category. |

---

### `source-layer: 'farmland'`
> **Layer containing polygons of agricultural and farmland areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z8` to `z15–15+ (z22)` | **Block**: `nature`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'cemetery'`
> **Layer containing polygons of cemeteries and graveyards.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `builtup`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'hospital'`
> **Layer containing polygons of hospital areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `builtup`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'education'`
> **Layer containing polygons of educational facilities.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `college`, `kindergarten`, `library`, `school`, `university` | Classification category. |

---

### `source-layer: 'military'`
> **Layer containing polygons of military or defense-related areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z8` to `z15–15+ (z22)` | **Block**: `builtup`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'construction'`
> **Layer containing polygons of areas under construction.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z8` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `civic`, `commercial`, `education`, `industrial`, `leisure`, `medical`, `military`, `other`, `religious`, `residential`, `transportation` | Classification categories for the construction areas. |

---

### `source-layer: 'leisure'`
> **Layer containing polygons of leisure, recreation and sport-related areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z10` to `z15–15+ (z22)` | **Block**: `builtup`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `dog_park`, `pitch`, `playground`, `recreation_ground`, `sports_centre`, `stadium`, `theme_park`, `track`, `winter_sports`, `zoo` | Classification category. |

---

## 8. Natural Landcover & Physical Geography

### `source-layer: 'forest'`
> **Layer containing polygons of generalized forested areas. Unlike the `wood` layer, it provides simplified geometry suitable for large-scale maps.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z0` to `z9` | **Block**: `nature`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'wood'`
> **Layer containing polygons of woodland and tree-covered areas. Unlike `forest` layer, it provides high-detail geometry suitable for large-scale maps.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z0` to `z15–15+ (z22)` | **Block**: `nature`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'grass'`
> **Layer containing polygons of grass and meadow areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `allotments`, `fell`, `garden`, `golf_course`, `grass`, `grassland`, `heath`, `meadow`, `park`, `recreation_ground`, `shrubbery`, `tundra`, `village_green` | Classification category. |

---

### `source-layer: 'vegetation'`
> **Layer containing polygons of general vegetation areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z0` to `z9` | **Block**: `nature`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'sand'`
> **Layer containing polygons of sandy terrains such as beaches and dunes.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `beach`, `dune`, `sand` | Classification category. |

---

### `source-layer: 'rock'`
> **Layer containing polygons of rocky areas, bare rocks and scree.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z7` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `bare_rock`, `rock`, `scree` | Classification category. |

---

### `source-layer: 'ice'`
> **Layer containing polygons of scrub or bushland areas.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z0` to `z9` | **Block**: `nature`

*This layer does not define distinct custom properties (pure geometry/fill classification).*

---

### `source-layer: 'tree'`
> **Layer containing points of individual trees.**  
* **Geometry**: `Point` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`name`** | free-form / string / number | Primary (local or official) name of the tree (if available). |
| **`name:{code}`** | free-form / string / number | Localized name of the tree (if available) in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'protected_area'`
> **Layer containing polygons of protected or restricted natural areas, classified by protection type.**  
* **Geometry**: `Polygon` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `habitat_species_management_area`, `national_park`, `natura_2000`, `natural_monument_or_feature`, `nature_reserve`, `protected_area`, `protected_area_with_sustainable_use_of_natural_resources`, `protected_landscape_seascape`, `strict_nature_reserve`, `water_protection_area`, `wilderness_area` | Classification category. |

---

### `source-layer: 'protected_area_major_label'`
> **Layer containing centerlines of major protected areas used for labeling.**  
* **Geometry**: `Point` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `habitat_species_management_area`, `national_park`, `natura_2000`, `natural_monument_or_feature`, `nature_reserve`, `protected_area`, `protected_area_with_sustainable_use_of_natural_resources`, `protected_landscape_seascape`, `strict_nature_reserve`, `water_protection_area`, `wilderness_area` | Classification category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the protected area. |
| **`name:{code}`** | free-form / string / number | Localized name of the protected area in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'protected_area_minor_label'`
> **Layer containing label points of minor or local named protected areas.**  
* **Geometry**: `Point` | **Zoom Range**: `z4` to `z15–15+ (z22)` | **Block**: `nature`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `habitat_species_management_area`, `national_park`, `natura_2000`, `natural_monument_or_feature`, `nature_reserve`, `protected_area`, `protected_area_with_sustainable_use_of_natural_resources`, `protected_landscape_seascape`, `strict_nature_reserve`, `water_protection_area`, `wilderness_area` | Classification category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the protected area. |
| **`name:{code}`** | free-form / string / number | Localized name of the protected area in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

## 9. Points of Interest (POI Modules)

### `source-layer: 'poi_food'`
> **Layer containing points of interest related to foods and beverages.**  
* **Geometry**: `Point` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class`** | `bar`, `biergarten`, `cafe`, `canteen`, `fast_food`, `food_court`, `ice_cream`, `pub`, `restaurant` | Classification category. |
| **`cuisine`** | free-form / string / number | Categories of types of foods and beverages. |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_shopping'`
> **Layer containing points of interest related to shopping and craft facilities.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class & subclass`** | free-form / string / number | Classification categories for shopping POIs with their subclass values. |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_healthcare'`
> **Layer containing points of interest related to health.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`class & subclass`** | free-form / string / number | Classification categories for healthcare POIs with their subclass values. |
| **`healthcare`** | free-form / string / number | Healthcare category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_transport'`
> **Layer containing points of interest related to transportation.**  
* **Geometry**: `Point` | **Zoom Range**: `z13` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the transportation facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for transportational POIs with their subclass values. |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`parking`** | `carports`, `garage`, `garage_boxes`, `half_on_kerb`, `lane`, `layby`, `multi-storey`, `on_kerb`, `other`, `parking` *(+5 more)* | Classification categories for the parking areas types. |
| **`parking_space`** | `ambulance`, `bicycle`, `boat_trailer`, `bus`, `car_sharing`, `caravan`, `carpool`, `charging`, `coach`, `compact` *(+20 more)* | Classification categories for the parking spaces. |
| **`ref`** | free-form / string / number | Reference code of the transportation facility. |

---

### `source-layer: 'poi_station'`
> **Layer containing points of interest related to public transport stations.**  
* **Geometry**: `Point` | **Zoom Range**: `z5` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`agg_stop`** | `True` | Indicates whether station is a main platform of public transport stops (buses, trams, and subways). |
| **`class & subclass`** | free-form / string / number | Classification categories of POI transport stations with their subclass values. |
| **`construction`** | `True` | Indicates whether the station is under construction. |
| **`customary_units`** | `ft`, `m` | Indicates whether elevation units of station is customary to use. |
| **`ele`** | free-form / string / number | Elevation in meters. Min value: - `-378` Max value: - `8376` |
| **`ele_ft`** | free-form / string / number | Elevation in feet. Min value: - `-1240` Max value: - `27480` |
| **`faa`** | free-form / string / number | The Federal Aviation Administration location identifier. |
| **`iata`** | free-form / string / number | 3-character code issued by the International Air Transport Association. |
| **`icao`** | free-form / string / number | 4-letter code issued by the International Civil Aviation Organization. |
| **`name`** | free-form / string / number | Primary (local or official) name of the station. |
| **`name:{code}`** | free-form / string / number | Localized name of the station in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`network`** | free-form / string / number | Network name or abbreviation. |
| **`ref`** | free-form / string / number | Reference code of the station. |

---

### `source-layer: 'poi_accommodation'`
> **Layer containing points of interest related to accommodation.**  
* **Geometry**: `Point` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the accommodation is publicly accessible. |
| **`class`** | `apartment`, `bed_and_breakfast`, `camp_site`, `caravan_site`, `chalet`, `guest_house`, `hostel`, `hotel`, `motel` | Classification category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the accommodation. |
| **`name:{code}`** | free-form / string / number | Localized name of the accommodation in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`stars`** | `0`, `1`, `2`, `3`, `4`, `5` | Quality classification of the accommodation. |

---

### `source-layer: 'poi_tourism'`
> **Layer containing points of interest related to tourism.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the tourist facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for tourist POIs with their subclass values. |
| **`direction`** | free-form / string / number | Clockwise rotation in degrees representing the angle of the label (class=viewpoint), suitable for use with the `text-rotate` style property. Min value: - `0` Max value: - `360` |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_culture'`
> **Layer containing points of interest related to culture.**  
* **Geometry**: `Point` | **Zoom Range**: `z13` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for cultural POIs with their subclass values. |
| **`name`** | free-form / string / number | Primary (local or official) name of the cultural POIs. |
| **`name:{code}`** | free-form / string / number | Localized name of the cultural POIs in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_education'`
> **Layer containing points of interest related to education.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for educational POIs with their subclass values. |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_public'`
> **Layer containing points of interest related to public facilities.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the facility is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categories for public POIs with their subclass values. |
| **`name`** | free-form / string / number | Primary (local or official) name of the facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'poi_sport'`
> **Layer containing points of interest related to sports.**  
* **Geometry**: `Point` | **Zoom Range**: `z12` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the sport facility is publicly accessible. |
| **`class`** | free-form / string / number | Classification category. |
| **`name`** | free-form / string / number | Primary (local or official) name of the sport facility. |
| **`name:{code}`** | free-form / string / number | Localized name of the sport facility in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |

---

### `source-layer: 'street_furniture'`
> **Layer containing points of street furniture like gates, crossings, street lamps.**  
* **Geometry**: `Point` | **Zoom Range**: `z15` to `z15–15+ (z22)` | **Block**: `poi`

| Field Name | Type / Allowed Values | Description |
| :--- | :--- | :--- |
| **`access`** | `conditional`, `no`, `yes` | Indicates whether the street furniture is publicly accessible. |
| **`class & subclass`** | free-form / string / number | Classification categoryies for the street furniture POI with their subclass values. |
| **`direction`** | free-form / string / number | Clockwise rotation in degrees representing the angle of the label (`class=street, subclass=crossing`), suitable for use with the `text-rotate` style property. Min value: - `-90` Max value: - `90` |
| **`lowered`** | `True` | Indicates whether the `class=protection, subclass=kerb` is lowered. |
| **`name`** | free-form / string / number | Primary (local or official) name of the street furniture. |
| **`name:{code}`** | free-form / string / number | Localized name of the street furniture in a specific language (when a translation exists that is different from the primary `name`). `{code}` represents [ISO 639 language code](#languages). |
| **`ref`** | free-form / string / number | Reference code of the street furniture. |

---

## 10. Multi-Lingual Name Translation Fields (`name:*`)

All label layers (`*_label`, `place_label`, `poi_*`, `water_name`, `road_label`) support automatic multi-lingual transliteration using ISO 639-1 language code suffixes:

| Attribute | Format | Description |
| :--- | :--- | :--- |
| **`name`** | String | Default local native name in the local script/alphabet (e.g. `Praha`, `Tokyo` in Kanji). |
| **`name:latin`** | String | Latinized transliteration of the local name for Western map styles. |
| **`name:nonlatin`**| String | Non-Latin original script name (if primary `name` is Latin). |
| **`name:en`** | String | English name translation. |
| **`name:de`**, **`name:fr`**, **`name:es`**, ... | String | Dedicated translation into German, French, Spanish, Chinese, Japanese, etc. |

```javascript
// Example MapLibre expression rendering English name with local fallback:
'text-field': [
  'coalesce',
  ['get', 'name:en'],
  ['get', 'name:latin'],
  ['get', 'name']
]
```
