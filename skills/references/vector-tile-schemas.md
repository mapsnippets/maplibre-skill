# Vector Tile Schemas Reference Catalog

This reference documents the complete suite of vector tile schemas and source layers available across MapTiler datasets. Use these exact layer names, attribute keys, and values when querying features, writing data-driven styling expressions, or applying layer filters.

> **Upstream Authority:** All vector tile schemas and layer field definitions conform to the official specifications in [`maptiler/maptiler-skills`](https://github.com/maptiler/maptiler-skills).

---

## 📑 Catalog of Vector Schemas

1. [Planet v4 (Global Basemap)](#1-planet-v4-global-basemap)
2. [Outdoor & Recreation Schema](#2-outdoor--recreation-schema)
3. [Elevation Contours Schema](#3-elevation-contours-schema)
4. [3D Buildings Schema](#4-3d-buildings-schema)
5. [Ocean & Bathymetry Schema](#5-ocean--bathymetry-schema)
6. [Cadastre & Land Parcels Schema](#6-cadastre--land-parcels-schema)
7. [Landcover & Environmental Schema](#7-landcover--environmental-schema)
8. [Countries & Administrative Boundaries](#8-countries--administrative-boundaries)
9. [National Open Data Schemas](#9-national-open-data-schemas)

---

## 1. Planet v4 (Global Basemap)

The primary global vector basemap schema powering `streets-v4`, `dataviz-v4`, and `base-v4`.

| Source Layer (`source-layer`) | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`transportation`** | LineString | Roads, railways, paths. Attributes: `class` (`motorway`, `trunk`, `primary`, `secondary`, `tertiary`, `minor`, `path`, `rail`, `transit`), `ramp`, `bridge`, `tunnel`, `level`, `surface` (`paved`/`unpaved`). |
| **`transportation_name`** | LineString, Point | Street names, highway route shields, ref codes: `name`, `name:latin`, `name:nonlatin`, `ref`, `shield`, `route_1`, `route_2`. |
| **`building`** | Polygon | 2D/3D building footprints: `render_height`, `render_min_height`, `hide_3d`, `colour`, `material`, `level`. |
| **`water`** | Polygon | Oceans, seas, lakes, reservoirs: `class` (`ocean`, `lake`, `river`, `swimming_pool`), `intermittent`. |
| **`waterway`** | LineString | Rivers, streams, canals: `class` (`river`, `stream`, `canal`, `ditch`), `name`, `name:latin`. |
| **`water_name`** | Point, LineString | Labels for oceans, seas, bays, lakes, and rivers: `name`, `name:latin`, `class`. |
| **`place`** | Point | Geographic labels: `class` (`country`, `state`, `province`, `city`, `town`, `village`, `suburb`, `neighbourhood`), `name`, `name:latin`, `rank` (1–15), `capital` (`2` for national, `4` for regional). |
| **`poi`** | Point | Points of interest: `class` (`food`, `drink`, `lodging`, `shopping`, `tourism`, `health`, `education`, `transport`), `subclass` (`restaurant`, `cafe`, `hotel`, `hospital`, `school`, `bank`, `pharmacy`, `museum`), `name`, `rank`. |
| **`landuse`** | Polygon | Urban and land use polygons: `class` (`park`, `residential`, `commercial`, `industrial`, `cemetery`, `hospital`, `school`, `glacier`, `pitch`, `sand`). |
| **`landcover`** | Polygon | Natural land cover: `class` (`wood`, `grass`, `scrub`, `wetland`, `crop`, `snow`, `barren`). |
| **`boundary`** | LineString | Political borders: `admin_level` (`2` country, `4` state, `6` county, `8` municipality), `disputed` (`0`/`1`), `maritime` (`0`/`1`). |
| **`aeroway`** | LineString, Polygon | Airport runways, taxiways, aprons, gates: `class` (`runway`, `taxiway`, `apron`, `gate`, `helipad`). |
| **`aerodrome_label`** | Point | Airport names and IATA codes: `name`, `iata`, `icao`, `ele`. |

---

## 2. Outdoor & Recreation Schema

Powers `outdoor-v4` and specialized topographic, trail, and alpine maps.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`trail`** | LineString | Hiking paths and mountaineering routes with Swiss Alpine Club (SAC) scale: `sac_scale` (`hiking`, `mountain_hiking`, `demanding_mountain_hiking`, `alpine_hiking`, `demanding_alpine_hiking`, `difficult_alpine_hiking`), `name`, `visibility`, `trail_visibility`. |
| **`cycling_route`** | LineString | Marked cycling networks: `network` (`icn` International, `ncn` National, `rcn` Regional, `lcn` Local), `route` (`bicycle`, `mtb`), `name`, `colour`, `ref`. |
| **`ski_piste`** | LineString, Polygon | Downhill and cross-country ski runs: `piste:type` (`downhill`, `nordic`, `skitour`, `sled`, `snow_park`), `piste:difficulty` (`novice` / green, `easy` / blue, `intermediate` / red, `advanced` / black, `expert` / orange, `freeride`), `piste:grooming`, `name`. |
| **`via_ferrata`** | LineString | Protected climbing routes: `via_ferrata_scale` (difficulty 1–6 / A–F), `name`. |
| **`mountain_peak`** | Point | Mountain summits and passes: `name`, `name:latin`, `ele` (elevation in meters), `ele_ft`, `prominence`, `rank`. |
| **`outdoor_poi`** | Point | Campsites, mountain huts, alpine shelters, viewpoints, guideposts, springs: `class` (`camp_site`, `alpine_hut`, `wilderness_hut`, `shelter`, `viewpoint`, `guidepost`, `spring`, `drinking_water`, `picnic_site`, `cave_entrance`). |

---

## 3. Elevation Contours Schema

Provides global vector isolines extracted from high-resolution DEMs.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`contour`** | LineString | Topographic elevation isolines.<br>• `ele`: Elevation in meters (integer).<br>• `ele_ft`: Elevation in feet (integer).<br>• `nth_line`: Hierarchy flag:<br>&nbsp;&nbsp;- `10`: Major index contour (rendered thick with text labels)<br>&nbsp;&nbsp;- `5`: Intermediate contour<br>&nbsp;&nbsp;- `1`: Minor contour line |

---

## 4. 3D Buildings Schema

Dedicated global high-density building footprints with rich 3D architectural properties.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`building`** | Polygon | Building polygons and architectural parts.<br>• `render_height`: Extrusion height in meters (float/integer).<br>• `render_min_height`: Base elevation height in meters (for bridges, cantilevers, skywalks).<br>• `hide_3d`: `true` if feature should be suppressed during 3D rendering.<br>• `colour`: Hex color code for roof or facade.<br>• `material`: Construction material (`glass`, `brick`, `concrete`, `wood`, `metal`).<br>• `roof_shape`: `flat`, `gabled`, `hipped`, `pyramidal`, `dome`, `skillion`.<br>• `roof_colour`: Hex roof color.<br>• `level`: Number of floors/storeys. |

---

## 5. Ocean & Bathymetry Schema

Provides underwater topography and depth contours for marine and bathymetric mapping.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`contour`** | Polygon | Ocean depth polygons: `depth` (negative integer in meters, e.g. `-20`, `-100`, `-1000`, `-5000`). |
| **`contour_line`** | LineString | Bathymetric depth isolines: `depth` (depth in meters), `depth_ft` (depth in feet). |
| **`marine_name`** | Point | Names for oceans, seas, trenches, ridges, underwater basins: `name`, `name:latin`, `class` (`ocean`, `sea`, `trench`, `ridge`, `basin`). |

---

## 6. Cadastre & Land Parcels Schema

Provides authoritative property boundaries, land ownership parcels, and parcel identification numbers.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`parcel`** | Polygon | Property parcel boundary: `parcel_id`, `cadastral_code`, `area_sqm`, `zoning_type`. |
| **`parcel_boundary`** | LineString | Border lines dividing adjacent land plots. |
| **`parcel_label`** | Point | Centroid labels containing official parcel numbers and plot IDs: `ref`, `label_text`. |

---

## 7. Landcover & Environmental Schema

High-resolution global classification of natural and agricultural land types.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`landcover`** | Polygon | Land classifications: `class` (`tree_cover` / forest, `shrubland`, `grassland`, `cropland` / agriculture, `wetland`, `mangroves`, `moss_lichen`, `bare_sparse`, `urban_builtup`, `water_bodies`, `snow_ice`). |

---

## 8. Countries & Administrative Boundaries

Specialized boundaries and administrative polygons with standard international identifiers.

| Source Layer | Geometry | Key Attributes & Description |
| :--- | :--- | :--- |
| **`country`** | Polygon | Country boundaries and polygon fills: `iso_a2` (e.g. `US`, `FR`, `DE`), `iso_a3` (`USA`, `FRA`, `DEU`), `iso_n3` (numeric code), `name`, `name:latin`, `continent`, `subregion`, `population`. |
| **`admin`** | LineString | Cleaned boundary lines with international recognition filters: `admin_level` (`2` Country, `4` State/Province, `6` County/District), `disputed` (`0`/`1`). |

---

## 9. National Open Data Schemas

Dedicated vector schemas for regional national mapping agencies:

* **Swiss swisstopo (`ch-swisstopo-lbm`)**: Official Swiss national vector dataset with Swiss cadastral and topographic precision.
* **UK Ordnance Survey (`uk-openzoomstack`)**: Complete OS Open Zoomstack with British National Grid alignment.
* **Netherlands Kadaster (`nl-cartiqo`)**: Official Dutch topography and topographic vector layers.
* **Japan GSI (`jp-gsi` / `jp-gsi-building`)**: Geospatial Information Authority of Japan vector datasets with high-precision urban building models.
* **French Cadastre (`fr-cadastre`)**: Official DGFiP French parcel mapping.
