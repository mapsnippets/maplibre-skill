# Vector Tile Schemas Reference (Planet v4)

This reference documents the source layers, attributes, classes, and geometry types inside standard MapTiler Planet v4 vector tiles. Use these exact layer names and field keys when writing custom layer styling, layer filters, or click querying.

---

## 1. Core Source Layers Summary

| Source Layer (`source-layer`) | Geometry Type | Description |
| :--- | :--- | :--- |
| `transportation` | LineString, MultiLineString | Roads, highways, paths, railways, ferries, runways |
| `transportation_name` | Point, LineString | Highway shields, road names, exit numbers |
| `building` | Polygon, MultiPolygon | 2D/3D building footprints, architectural parts |
| `water` | Polygon, MultiPolygon | Oceans, lakes, large rivers, swimming pools |
| `waterway` | LineString, MultiLineString | Rivers, streams, canals, ditches |
| `water_name` | Point, LineString | Names for seas, lakes, bays, and rivers |
| `place` | Point | Countries, states, provinces, cities, towns, villages |
| `poi` | Point | Points of interest (restaurants, hotels, shops, monuments) |
| `landcover` | Polygon, MultiPolygon | Forests, grasslands, wetlands, glaciers, ice |
| `landuse` | Polygon, MultiPolygon | Parks, residential areas, commercial, industrial zones |
| `boundary` | LineString, MultiLineString | Administrative boundaries (national, state, county) |
| `contour` | LineString, MultiLineString | Elevation contour lines with metric and imperial units |
| `aeroway` | LineString, Polygon | Airport runways, taxiways, aprons, gates |

---

## 2. Detailed Layer Specifications

### `building`
Used for 2D footprint rendering and 3D architectural extrusions.

* **Key Attributes:**
  * `render_height`: Extrusion height in meters (number).
  * `render_min_height`: Base height above ground in meters (number).
  * `hide_3d`: Boolean flag (`true` or `false`) indicating if a 2D feature should be omitted from 3D extrusion.
  * `colour`: Hex or CSS color string for roof/facade if available.
  * `material`: Material type (e.g. `brick`, `glass`, `concrete`).
  * `level`: Floor count (number).

### `transportation`
Used for styling roads, railways, and transit infrastructure.

* **Key Attributes:**
  * `class`: Classification of the road or track:
    * High-capacity: `motorway`, `trunk`, `primary`, `secondary`, `tertiary`
    * Local: `minor`, `service`, `residential`, `living_street`, `unclassified`
    * Non-motorized: `path`, `pedestrian`, `cycleway`, `footway`, `steps`, `track`
    * Rail: `rail`, `transit`, `subway`, `tram`, `light_rail`, `cable_car`
  * `subclass`: Refined category (e.g., `link`, `driveway`, `parking_aisle`).
  * `ramp`: `1` for freeway on/off ramps, `0` otherwise.
  * `bridge`: `1` for elevated structures, `0` otherwise.
  * `tunnel`: `1` for underground tunnels, `0` otherwise.
  * `level`: Z-ordering level for complex multi-tier interchanges.
  * `surface`: `paved` vs `unpaved`.

### `place`
Used for cartographic labels of geographic entities.

* **Key Attributes:**
  * `class`: `country`, `state`, `province`, `city`, `town`, `village`, `hamlet`, `suburb`, `neighbourhood`, `island`.
  * `name`: Localized display name.
  * `name:latin`: Latin alphabet transliteration.
  * `name:nonlatin`: Original script name.
  * `rank`: Relative visual importance for label collision sorting (lower is more prominent, 1-15).
  * `capital`: `2` for national capital, `4` for regional capital.

### `poi`
Used for specific establishments, facilities, and points of interest.

* **Key Attributes:**
  * `class`: High-level category: `food`, `drink`, `lodging`, `shopping`, `tourism`, `health`, `education`, `transport`, `sport`.
  * `subclass`: Specific amenity (e.g. `restaurant`, `cafe`, `hospital`, `pharmacy`, `school`, `hotel`, `bank`, `museum`).
  * `name`: Facility name.
  * `rank`: Visual collision priority (1–30).

### `boundary`
Used for administrative border visualization.

* **Key Attributes:**
  * `admin_level`:
    * `2`: International country borders
    * `4`: State / Province borders
    * `6`: County / District borders
    * `8`: Municipal borders
  * `disputed`: `1` for geopolitical disputed borders, `0` for recognized borders.
  * `maritime`: `1` for territorial sea borders, `0` for land borders.

### `contour`
Used for topographic elevation contour visualization.

* **Key Attributes:**
  * `ele`: Elevation in meters (integer).
  * `ele_ft`: Elevation in feet (integer).
  * `nth_line`: Index contour hierarchy:
    * `10`: Major index contour (every 100m/500m)
    * `5`: Intermediate contour (every 50m)
    * `1`: Regular contour (every 10m/20m)
