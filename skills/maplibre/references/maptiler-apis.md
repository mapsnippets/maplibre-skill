# MapTiler Cloud REST APIs for MapLibre GL JS

When using raw MapLibre GL JS (not MapTiler SDK), call MapTiler Cloud APIs directly with `fetch()`.

> [MapTiler Cloud API Docs](https://docs.maptiler.com/cloud/api/) · [Cloud Console](https://cloud.maptiler.com/)

---

## Base URL

All endpoints: `https://api.maptiler.com/`

All requests require: `?key=YOUR_MAPTILER_KEY`

---

## Geocoding API

### Forward Geocoding (search by name)

```
GET https://api.maptiler.com/geocoding/{query}.json?key=YOUR_MAPTILER_KEY
```

```js
async function geocodeForward(query) {
  const url = `https://api.maptiler.com/geocoding/${encodeURIComponent(query)}.json?key=YOUR_MAPTILER_KEY&limit=5`;
  const response = await fetch(url);
  const data = await response.json();
  // data.features[0].geometry.coordinates → [lng, lat]
  // data.features[0].place_name → "Prague, Czech Republic"
  // data.features[0].bbox → [minLng, minLat, maxLng, maxLat]
  return data;
}
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | Search text (URL-encoded) |
| `limit` | number | Max results (1-10, default 5) |
| `language` | string | Result language (`en`, `cs`, `de`, etc.) |
| `proximity` | string | Bias results near `lng,lat` |
| `bbox` | string | Limit to bounding box `minLng,minLat,maxLng,maxLat` |
| `types` | string | Filter by type: `country`, `region`, `place`, `address`, `poi` |
| `country` | string | Limit to country codes (`CZ`, `US,CA`) |

### Reverse Geocoding (coordinates to address)

```
GET https://api.maptiler.com/geocoding/{lng},{lat}.json?key=YOUR_MAPTILER_KEY
```

```js
async function geocodeReverse(lng, lat) {
  const url = `https://api.maptiler.com/geocoding/${lng},${lat}.json?key=YOUR_MAPTILER_KEY`;
  const response = await fetch(url);
  const data = await response.json();
  // data.features[0].place_name → "Old Town Square 1, Prague, Czech Republic"
  return data;
}
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `language` | string | Result language |
| `types` | string | Filter by type |
| `limit` | number | Max results |

### Geocoding Response Structure

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [14.4178, 50.1167]
      },
      "properties": {
        "ref": "...",
        "country_code": "cz"
      },
      "place_name": "Prague, Czech Republic",
      "place_type": ["place"],
      "center": [14.4178, 50.1167],
      "bbox": [14.2244, 49.9419, 14.7068, 50.1774]
    }
  ]
}
```

---

## Maps / Styles API

### Vector Style (primary usage)

```
GET https://api.maptiler.com/maps/{style}/style.json?key=YOUR_MAPTILER_KEY
```

Used directly as MapLibre `style` parameter.

### Static Map Images

```
GET https://api.maptiler.com/maps/{style}/static/{lng},{lat},{zoom}/{width}x{height}.png?key=YOUR_MAPTILER_KEY
```

```js
function getStaticMapUrl(lng, lat, zoom, width, height, style = 'streets-v4') {
  return `https://api.maptiler.com/maps/${style}/static/${lng},${lat},${zoom}/${width}x${height}.png?key=YOUR_MAPTILER_KEY`;
}

// Usage
const imgUrl = getStaticMapUrl(14.4178, 50.1167, 12, 800, 600);
```

| Parameter | Description |
|-----------|-------------|
| `@2x` | Append to path for HiDPI |
| `markers` | Add pin markers (see docs) |
| `path` | Add route lines |

---

## Tiles API

### Vector Tiles (TileJSON)

```
GET https://api.maptiler.com/tiles/v3/tiles.json?key=YOUR_MAPTILER_KEY
```

> Note: MapTiler styles already include vector tile sources — you rarely need to add these manually.

### Terrain Tiles (Raster-DEM)

```
GET https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_MAPTILER_KEY
```

```js
map.addSource('terrain', {
  type: 'raster-dem',
  url: 'https://api.maptiler.com/tiles/terrain-rgb-v2/tiles.json?key=YOUR_MAPTILER_KEY',
  tileSize: 256
});
map.setTerrain({ source: 'terrain', exaggeration: 1.5 });
```

### Satellite Tiles

```
GET https://api.maptiler.com/tiles/satellite-v2/tiles.json?key=YOUR_MAPTILER_KEY
```

```js
map.addSource('satellite', {
  type: 'raster',
  url: 'https://api.maptiler.com/tiles/satellite-v2/tiles.json?key=YOUR_MAPTILER_KEY',
  tileSize: 256
});
```

---

## Geolocation API (IP-based)

```
GET https://api.maptiler.com/geolocation/ip.json?key=YOUR_MAPTILER_KEY
```

```js
async function getVisitorLocation() {
  const response = await fetch(
    'https://api.maptiler.com/geolocation/ip.json?key=YOUR_MAPTILER_KEY'
  );
  const data = await response.json();
  // data.longitude, data.latitude
  // data.city, data.country, data.country_code
  return [data.longitude, data.latitude];
}

// Center map on visitor
const coords = await getVisitorLocation();
map.flyTo({ center: coords, zoom: 12 });
```

---

## Coordinates API (CRS transform)

```
GET https://api.maptiler.com/coordinates/search/{query}.json?key=YOUR_MAPTILER_KEY
```

Transform between coordinate reference systems (10,000+ EPSG codes).

```js
// Search for a CRS
const response = await fetch(
  'https://api.maptiler.com/coordinates/search/EPSG:5514.json?key=YOUR_MAPTILER_KEY'
);
```

```
GET https://api.maptiler.com/coordinates/transform/{coords}.json?key=YOUR_MAPTILER_KEY&s_srs=4326&t_srs=5514
```

---

## Error Handling

```js
async function safeApiFetch(url) {
  try {
    const response = await fetch(url);

    if (!response.ok) {
      if (response.status === 401) throw new Error('Invalid API key');
      if (response.status === 403) throw new Error('API key not authorized for this endpoint');
      if (response.status === 429) throw new Error('Rate limit exceeded');
      throw new Error(`API error: ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    console.error('MapTiler API error:', error.message);
    return null;
  }
}
```

---

## Rate Limits

- Free tier: reasonable limits for development
- Paid plans: higher limits, session-based billing available via MapTiler SDK
- When using raw MapLibre + fetch(), each API call counts as a separate request
- For production apps with high traffic, consider MapTiler SDK for session-based billing
